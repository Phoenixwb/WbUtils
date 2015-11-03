package common
{
	import flash.utils.Dictionary;
	
	/**  
	 * @author: W.Beta  
	 */  
	public class HashMap
	{
		private var _has_change:Boolean;
		private var _length:int;
		private var _keys:Array;
		private var _values:Array;
		private var _items:Array;
		
		private var _dict:Dictionary;
		private var _destroyed:Boolean;
		public function HashMap(dict:Dictionary = null)
		{
			super();
			_dict = dict;
			if(_dict == null)
			{
				_dict = new Dictionary();
			}
		}
		
		public function set_value(key:*,val:*):*
		{
			_has_change = true;
			_dict[key] = val;
			return val;
		}
		
		public function get_value(key:*):*
		{
			return _dict[key];
		}
		
		public function pop(key:*):void
		{
			_has_change = true;
			delete _dict[key];
		}
	
		public function reset(dict:Dictionary):void
		{
			_dict = dict;
			_has_change = true;
		}
	
		public function get dict():Dictionary
		{
			return _dict;
		}

		public function get length():int
		{
			update();
			return _length;
		}
		
		public function keys():Array
		{
			update();
			return _keys;
		}
		
		public function values():Array
		{
			update();
			return _values;
		}
		
		public function items():Array
		{
			update();
			return _items;
		}
		
		private function update():void
		{
			if(_has_change)
			{
				if(_keys == null)
				{
					_keys = new Array();
					_values = new Array();
					_items = new Array();
				}
				_keys.splice(0,_keys.length);
				_values.splice(0,_values.length);
				_items.splice(0,_items.length);
				var len:int;
				for (var key:* in _dict) 
				{
					len ++;
					_keys.push(key);
					var val:* = _dict[key];
					_values.push(val);
					_items.push([key,val]);
				}
				_length = len;
				_has_change = false;
			}
		}
	
		public function clear():void
		{
			for (var key:* in _dict) 
			{
				delete _dict[key];
			}
			_has_change = true;
		}
	
		public function dispose():void
		{
			clear();
			_dict = null;
			_destroyed = true;
		}
		
	}
}
