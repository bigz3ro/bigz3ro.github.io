---
layout: post
title: Kết hợp $append và relation giữa các bảng .
date:   2017-05-03 22:04
categories: linux
---

# Giới thiệu
# H1 Kết hợp $append và relation giữa các bảng để lấy dữ liệu trả về dạng JSON .
..* Khi bạn sử dụng Laravel để xáy dựng Resful API . Thì ta sẽ có những lúc ta muốn lấy dữ liệu 
trả về có chứa field data của 1 bảng quan hệ trong cùng Response trả về . Mình đã được gợi ý là 
sử dụng 1 trick rất hữu ích đó là: sử dụng biến $append của Laravel kết hợp với relation tới bảng
khác chứa dữ liệu mà bạn muốn lấy.
..* Nói như vậy sẽ rất khó hiểu mình sẽ demo 1 ví dụ thực tế.

```php
	<?php

	namespace App\Modules\Lost\Models;

	use Illuminate\Database\Eloquent\Model;
	use Carbon\Carbon as Carbon;
	use Validator;
	use Auth;
	use App\Helpers\ResTools;
	use App\Helpers\ValidateTools;
	use App\Helpers\Tools;
	use App\Modules\Atoken\Models\Atoken;
	use App\Modules\BillOfLanding\Models\BillOfLanding;
	use App\Modules\ChatLost\Models\ChatLost;


	class Lost extends Model{
		/**
		 * The table associated with the model.
		 *
		 * @var string
		 */
		protected $table = 'lost';
		public $timestamps = false;
		#
		protected $appends = [
			'bol_code'
		];
		/**
		 * The attributes that are mass assignable.
		 *
		 * @var array
		 */
		protected $fillable = [
			'bill_of_landing_id',
			'description'
		];

		/**
		 * The attributes excluded from the model's JSON form.
		 *
		 * @var array
		 */
		protected $hidden = [
			'billOfLanding',
		];

		public static $fieldDescriptions = [
			'bill_of_landing_id' => 'int,required',
			'description' => 'str'
		];

		public static $searchFields = ['check_date', 'bill_of_landing_id'];
		
		public function billOfLanding(){
			return $this->belongsTo('App\Modules\BillOfLanding\Models\BillOfLanding', 'bill_of_landing_id');
		}
		
		public function chatLost(){
			return $this->hasMany('App\Modules\ChatLost\Models\ChatLost', 'lost_id');
		}
		
		#Gía trị bạn muốn lấy là trường code trong bảng BillOfLanding
		#Tên function phải viết dạng cameCase trùng với key bol_code trong $append
		#Sử dụng kết hợp $hidden để dấu dữ liệu trả ra từ response đó là billOfLanding do 
		# lấy bảng quan hệ
		public function getBolCodeAttribute($value){
			if($this->bill_of_landing_id && $this->billOfLanding){
				return $this->billOfLanding->code;
			}
			return null;
	    }
		
		public static function list($params=[], $keyword=null, $orderBy='-id'){
			try{
				//Default orderby theo id => parse ['id', 'desc']
				$orderBy = Tools::parseOrderBy($orderBy);
				$listItem = Lost::where($params);

				if($keyword && strlen($keyword) >= 1){
					if(is_numeric($keyword)){
						$listItem->where('id', intval($keyword));
					}else{
						$listItem = $listItem->where(function($query) use ($keyword){
							foreach(self::$searchFields as $key => $field){
								$query->orWhere($field, 'ilike', '%' . $keyword . '%');
							}
						});
					}
				}
				$listItem = $listItem ->
					orderBy($orderBy[0], $orderBy[1])->
					paginate(config('app.page_size'));
				
				$extra = [
					'listBillLost' => BillOfLanding::whereNull('order_id')->whereNull('address_id')->get()
				];
				return ResTools::lst($listItem, $extra);
			}
			catch(\Exception $e){return ResTools::criticalErr(Tools::errMessage($e));}
			catch(\Error $e){return ResTools::criticalErr(Tools::errMessage($e));}
		}

		public static function obj($params=null, $original=false){
			try{
				$result = null;
				if($params === null){
					return $result;
				}
			
				if(gettype($params) === "integer"){
					$result = self::find($params);
					$result->chatLost;
				}else if(gettype($params) === "string"){
					$result = null;
				}else{
					if(count($params) === 1 && array_key_exists("id", $params)){
						$result = self::find(intval($params['id']));
					}else{
						$result = self::where($params)->first();
					}
				}
				if(!$result){
					return ResTools::err(
						trans('messages.item_not_exist'),
						ResTools::$ERROR_CODES['NOT_FOUND']
					);
				}
				// $extra = [
				// 	'listMessage' => $result->chatLost
				// ];
				return ResTools::obj($result, $original);
			}
			catch(\Exception $e){return ResTools::criticalErr(Tools::errMessage($e));}
			catch(\Error $e){return ResTools::criticalErr(Tools::errMessage($e));}
		}

		public static function addItem($input){
			try{
				// Tools::pre($input);die;
				if(array_key_exists('success', $input) && array_key_exists('status_code', $input) && $input['status_code'] !== 200){
		            return $input;
				}
				$checkDuplicate = self::where(["bill_of_landing_id" => $input["bill_of_landing_id"]])->first();
				if($checkDuplicate){
					return ResTools::err(
						trans('messages.duplicate_item'),
						ResTools::$ERROR_CODES['BAD_REQUEST']
					);
				}
				
				$item = self::create($input);
				return ResTools::obj($item, trans('messages.add_success'));
			}
			catch(\Exception $e){return ResTools::criticalErr(Tools::errMessage($e));}
			catch(\Error $e){return ResTools::criticalErr(Tools::errMessage($e));}
		}

		public function save(array $options = array()){
			# Auto increase order (if order exist)
	        $colums = $this->getConnection()->getSchemaBuilder()->getColumnListing($this->getTable());
			if(in_array('updated_at', $colums)){
				$this->updated_at = Tools::nowDateTime();
			}
			if(!$this->exists){
				if(in_array('created_at', $colums)){
					$this->created_at = Tools::nowDateTime();
				}
				if(self::count() > 0){
					$largestIdItem = self::orderBy('id', 'desc')->first();
					$this->id = $largestIdItem->id + 1;
				}else{
					$this->id = 1;
				}
				if(in_array('order', $colums)){
					if($this->order === 0){
						$largestOrderItem = self::orderBy('order', 'desc')->first();
						if($largestOrderItem){
							$this->order = $largestOrderItem->order + 1;
						}else{
							$this->order = 1;
						}
					}
				}
			}
			// before save code
			parent::save();
			// after save code
		}
	}
```

Vậy là xong rồi .Bạn có thể áp dụng nó trong những trường hợp bạn cần lấy
dữ liệu trả về dạng JSON mà chứa dữ liệu từ bảng quan hệ khác . Tuy nhiên cách này 
chưa phải tối ưu nhất . Mình sẽ học hỏi thêm và chia sẻ cùng các bạn.
