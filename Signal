package 
{
	import flash.utils.Dictionary;
	
	import avmplus.getQualifiedClassName;

	/**
	 * Signal
	 */
	public class Signal
	{
		private var _classes:Array;
		private var _listenersMap:Dictionary;
		private var _listeners:Array;
		private var _canSort:Boolean;
		private var _dispatching:Boolean;
		private var _needSort:Boolean;
		private var _needClear:Boolean = false;
		private var _needDispose:Boolean = false;
		
		public function Signal(...classes)
		{
			_classes = classes;
			_listenersMap = new Dictionary();
			_listeners = new Array();
		}
		
		public function add(listener:Function, priority:int = 0):Function{
			var data:ListenerData;
			CONFIG::DEBUG{
				data = _listenersMap[listener];
				if(data){
					throw new Error("重复添加同一个监听");
				}
			}
			data = new ListenerData();
			data.priority = priority;
			_listenersMap[listener] = data;
			var index:int = _listeners.indexOf(listener);
			if(index > -1){
				_listeners.splice(index,1);
			}
			_listeners.push(listener);
			if(!_canSort && _listeners.length > 1)
				_canSort = true;
			
			return listener;
		}
		
		public function remove(listener:Function):Function{
			if(_listenersMap[listener] == null){
				return null;
			}
			delete _listenersMap[listener];
			var index:int = _listeners.indexOf(listener);
			if(index > -1){
				_listeners.splice(index,1);
			}
			if(_listeners.length < 2){
				_canSort = false;
			}
			return listener;
		}
		
		public function dispatch(...values):Signal{
			if(_listeners == null || _listeners.length < 1)
				return this;
			CONFIG::DEBUG{
				if (values.length != _classes.length) {
					throw new Error("错误的参数长度，dispatch时应该与new时声明的参数长度一致! 应为: " + _classes.length + "个 实为: " + values.length + "个");
				}
				
				var i:int, len:int;
				for (i = 0, len = values.length; i < len; ++i) {
					if(values[i] is _classes[i] || values[i]  == null) 
						continue;
					throw new Error("错误的函数类型，dispatch时应该与new时声明的参数类型一致! 应为: " + _classes[i] + " 实为: " + getQualifiedClassName(values[i]));
				}
			}
			if(_needSort && _canSort)
				sortListeners();
			
			var curListeners:Array = _listeners.concat();
			_dispatching = true;
			for each(var f:Function in curListeners){
				f.apply(f,values);
			}
			_dispatching = false;
			curListeners.length = 0;
			curListeners = null;
			
			if(_needClear){
				clear();
				_needClear = false;
			}
			if(_needDispose){
				dispose();
				_needDispose = false;
			}
			return this;
		}
		
		public function set needSort(bool:Boolean):void{
			_needSort = bool;
		}
		
		public function get needSort():Boolean{
			return _needSort;
		}
		
		private function sortListeners():void{
			_listeners.sort(sort_by_priority);
			_canSort = false;
		}
		
		private function sort_by_priority(fun1:Function, fun2:Function):Number {
			var data1:ListenerData = _listenersMap[fun1];
			var data2:ListenerData = _listenersMap[fun2];
			if(data2.priority == data1.priority){
				return 0;
			}else if(data2.priority > data1.priority){
				return -1;
			}else{
				return 1;
			}
		}
		
		public function clear():void{
			if(_dispatching){
				_needClear = true;
			}else{
				for (var key:* in _listenersMap) 
				{
					delete _listenersMap[key];
				}
				_listeners.length = 0;
			}
		}
		
		public function dispose():void{
			if(_dispatching){
				_needDispose = true;
			}else{
				if(_classes)
					_classes.length = 0;
				_classes = null;
				_listeners.length = 0;
				_listeners = null;
				for(var key:* in _listenersMap) 
				{
					delete _listenersMap[key];
				}
				_listenersMap = null;
			}
		}
	}
}
class ListenerData{
	public var priority:int;
}
