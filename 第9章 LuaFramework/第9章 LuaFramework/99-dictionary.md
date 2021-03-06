##使用字典

####UseDictionary.cs
```csharp
    using UnityEngine;
    using System.Collections.Generic;
    using LuaInterface;
    
    public class TestAccount
    {
        public int id;
        public string name;
        public int sex;
    
        public TestAccount(int id, string name, int sex)
        {
            this.id = id;
            this.name = name;
            this.sex = sex;
        }
    }
    
    public class UseDictionary : MonoBehaviour 
    {
        Dictionary<int, TestAccount> map = new Dictionary<int, TestAccount>();
    
        string script =
            @"              
                function TestDict(map)                        
                    local iter = map:GetEnumerator() 
                    
                    while iter:MoveNext() do
                        local v = iter.Current.Value
                        print('id: '..v.id ..' name: '..v.name..' sex: '..v.sex)                                
                    end
    
                    local flag, account = map:TryGetValue(1, nil)
    
                    if flag then
                        print('TryGetValue result ok: '..account.name)
                    end
    
                    local keys = map.Keys
                    iter = keys:GetEnumerator()
                    print('------------print dictionary keys---------------')
                    while iter:MoveNext() do
                        print(iter.Current)
                    end
                    print('----------------------over----------------------')
    
                    local values = map.Values
                    iter = values:GetEnumerator()
                    print('------------print dictionary values---------------')
                    while iter:MoveNext() do
                        print(iter.Current.name)
                    end
                    print('----------------------over----------------------')                
    
                    print('kick '..map[2].name)
                    map:Remove(2)
                    iter = map:GetEnumerator() 
    
                    while iter:MoveNext() do
                        local v = iter.Current.Value
                        print('id: '..v.id ..' name: '..v.name..' sex: '..v.sex)                                
                    end
                end                        
            ";
    
    	void Awake () 
        {
            map.Add(1, new TestAccount(1, "水水", 0));
            map.Add(2, new TestAccount(2, "王伟", 1));
            map.Add(3, new TestAccount(3, "王芳", 0));
    
            LuaState luaState = new LuaState();
            luaState.Start();
            BindMap(luaState);
    
            luaState.DoString(script, "UseDictionary.cs");
            LuaFunction func = luaState.GetFunction("TestDict");
            func.BeginPCall();
            func.Push(map);
            func.PCall();
            func.EndPCall();
    
            func.Dispose();
            func = null;
            luaState.CheckTop();
            luaState.Dispose();
            luaState = null;
        }
    
        //示例方式，正常导出无需手写下面代码
        void BindMap(LuaState L)
        {
            L.BeginModule(null);
            TestAccountWrap.Register(L);
            L.BeginModule("System");
            L.BeginModule("Collections");
            L.BeginModule("Generic");
            System_Collections_Generic_Dictionary_int_TestAccountWrap.Register(L);
            System_Collections_Generic_KeyValuePair_int_TestAccountWrap.Register(L);
            L.BeginModule("Dictionary");
            System_Collections_Generic_Dictionary_int_TestAccount_KeyCollectionWrap.Register(L);
            System_Collections_Generic_Dictionary_int_TestAccount_ValueCollectionWrap.Register(L);
            L.EndModule();
            L.EndModule();
            L.EndModule();
            L.EndModule();
            L.EndModule();
        }
    }
```

&emsp;

####System_Collections_Generic_Dictionary_int_TestAccountWrap.cs

```csharp
	//this source code was auto-generated by tolua#, do not modify it
	using System;
	using LuaInterface;
	
	public class System_Collections_Generic_Dictionary_int_TestAccountWrap
	{
		public static void Register(LuaState L)
		{
			L.BeginClass(typeof(System.Collections.Generic.Dictionary<int,TestAccount>), typeof(System.Object), "AccountMap");
			L.RegFunction(".geti", get_Item);
			L.RegFunction("get_Item", get_Item);
			L.RegFunction(".seti", set_Item);
			L.RegFunction("set_Item", set_Item);
			L.RegFunction("Add", Add);
			L.RegFunction("Clear", Clear);
			L.RegFunction("ContainsKey", ContainsKey);
			L.RegFunction("ContainsValue", ContainsValue);
			L.RegFunction("GetObjectData", GetObjectData);
			L.RegFunction("OnDeserialization", OnDeserialization);
			L.RegFunction("Remove", Remove);
			L.RegFunction("TryGetValue", TryGetValue);
			L.RegFunction("GetEnumerator", GetEnumerator);
			L.RegFunction("New", _CreateSystem_Collections_Generic_Dictionary_int_TestAccount);
			L.RegVar("this", _this, null);
			L.RegFunction("__tostring", ToLua.op_ToString);
			L.RegVar("Count", get_Count, null);
			L.RegVar("Comparer", get_Comparer, null);
			L.RegVar("Keys", get_Keys, null);
			L.RegVar("Values", get_Values, null);
			L.EndClass();
		}
	
		[MonoPInvokeCallbackAttribute(typeof(LuaCSFunction))]
		static int _CreateSystem_Collections_Generic_Dictionary_int_TestAccount(IntPtr L)
		{
			try
			{
				int count = LuaDLL.lua_gettop(L);
	
				if (count == 0)
				{
					System.Collections.Generic.Dictionary<int,TestAccount> obj = new System.Collections.Generic.Dictionary<int,TestAccount>();
					ToLua.PushObject(L, obj);
					return 1;
				}
				else if (count == 1 && TypeChecker.CheckTypes(L, 1, typeof(int)))
				{
					int arg0 = (int)LuaDLL.luaL_checknumber(L, 1);
					System.Collections.Generic.Dictionary<int,TestAccount> obj = new System.Collections.Generic.Dictionary<int,TestAccount>(arg0);
					ToLua.PushObject(L, obj);
					return 1;
				}
				else if (count == 1 && TypeChecker.CheckTypes(L, 1, typeof(System.Collections.Generic.IDictionary<int,TestAccount>)))
				{
					System.Collections.Generic.IDictionary<int,TestAccount> arg0 = (System.Collections.Generic.IDictionary<int,TestAccount>)ToLua.CheckObject(L, 1, typeof(System.Collections.Generic.IDictionary<int,TestAccount>));
					System.Collections.Generic.Dictionary<int,TestAccount> obj = new System.Collections.Generic.Dictionary<int,TestAccount>(arg0);
					ToLua.PushObject(L, obj);
					return 1;
				}
				else if (count == 1 && TypeChecker.CheckTypes(L, 1, typeof(System.Collections.Generic.IEqualityComparer<int>)))
				{
					System.Collections.Generic.IEqualityComparer<int> arg0 = (System.Collections.Generic.IEqualityComparer<int>)ToLua.CheckObject(L, 1, typeof(System.Collections.Generic.IEqualityComparer<int>));
					System.Collections.Generic.Dictionary<int,TestAccount> obj = new System.Collections.Generic.Dictionary<int,TestAccount>(arg0);
					ToLua.PushObject(L, obj);
					return 1;
				}
				else if (count == 2 && TypeChecker.CheckTypes(L, 1, typeof(int), typeof(System.Collections.Generic.IEqualityComparer<int>)))
				{
					int arg0 = (int)LuaDLL.luaL_checknumber(L, 1);
					System.Collections.Generic.IEqualityComparer<int> arg1 = (System.Collections.Generic.IEqualityComparer<int>)ToLua.CheckObject(L, 2, typeof(System.Collections.Generic.IEqualityComparer<int>));
					System.Collections.Generic.Dictionary<int,TestAccount> obj = new System.Collections.Generic.Dictionary<int,TestAccount>(arg0, arg1);
					ToLua.PushObject(L, obj);
					return 1;
				}
				else if (count == 2 && TypeChecker.CheckTypes(L, 1, typeof(System.Collections.Generic.IDictionary<int,TestAccount>), typeof(System.Collections.Generic.IEqualityComparer<int>)))
				{
					System.Collections.Generic.IDictionary<int,TestAccount> arg0 = (System.Collections.Generic.IDictionary<int,TestAccount>)ToLua.CheckObject(L, 1, typeof(System.Collections.Generic.IDictionary<int,TestAccount>));
					System.Collections.Generic.IEqualityComparer<int> arg1 = (System.Collections.Generic.IEqualityComparer<int>)ToLua.CheckObject(L, 2, typeof(System.Collections.Generic.IEqualityComparer<int>));
					System.Collections.Generic.Dictionary<int,TestAccount> obj = new System.Collections.Generic.Dictionary<int,TestAccount>(arg0, arg1);
					ToLua.PushObject(L, obj);
					return 1;
				}
				else
				{
					return LuaDLL.luaL_throw(L, "invalid arguments to ctor method: System.Collections.Generic.Dictionary<int,TestAccount>.New");
				}
			}
			catch(Exception e)
			{
				return LuaDLL.toluaL_exception(L, e);
			}
		}
	
		[MonoPInvokeCallbackAttribute(typeof(LuaCSFunction))]
		static int _get_this(IntPtr L)
		{
			try
			{
				ToLua.CheckArgsCount(L, 2);
				System.Collections.Generic.Dictionary<int,TestAccount> obj = (System.Collections.Generic.Dictionary<int,TestAccount>)ToLua.CheckObject(L, 1, typeof(System.Collections.Generic.Dictionary<int,TestAccount>));
				int arg0 = (int)LuaDLL.luaL_checknumber(L, 2);
				TestAccount o = obj[arg0];
				ToLua.PushObject(L, o);
				return 1;
	
			}
			catch(Exception e)
			{
				return LuaDLL.toluaL_exception(L, e);
			}
		}
	
		[MonoPInvokeCallbackAttribute(typeof(LuaCSFunction))]
		static int _set_this(IntPtr L)
		{
			try
			{
				ToLua.CheckArgsCount(L, 3);
				System.Collections.Generic.Dictionary<int,TestAccount> obj = (System.Collections.Generic.Dictionary<int,TestAccount>)ToLua.CheckObject(L, 1, typeof(System.Collections.Generic.Dictionary<int,TestAccount>));
				int arg0 = (int)LuaDLL.luaL_checknumber(L, 2);
				TestAccount arg1 = (TestAccount)ToLua.CheckObject(L, 3, typeof(TestAccount));
				obj[arg0] = arg1;
				return 0;
	
			}
			catch(Exception e)
			{
				return LuaDLL.toluaL_exception(L, e);
			}
		}
	
		[MonoPInvokeCallbackAttribute(typeof(LuaCSFunction))]
		static int _this(IntPtr L)
		{
			try
			{
				LuaDLL.lua_pushvalue(L, 1);
				LuaDLL.tolua_bindthis(L, _get_this, _set_this);
				return 1;
			}
			catch(Exception e)
			{
				return LuaDLL.toluaL_exception(L, e);
			}
		}
	
		[MonoPInvokeCallbackAttribute(typeof(LuaCSFunction))]
		static int get_Item(IntPtr L)
		{
			try
			{
				ToLua.CheckArgsCount(L, 2);
				System.Collections.Generic.Dictionary<int,TestAccount> obj = (System.Collections.Generic.Dictionary<int,TestAccount>)ToLua.CheckObject(L, 1, typeof(System.Collections.Generic.Dictionary<int,TestAccount>));
				int arg0 = (int)LuaDLL.luaL_checknumber(L, 2);
				TestAccount o = obj[arg0];
				ToLua.PushObject(L, o);
				return 1;
			}
			catch(Exception e)
			{
				return LuaDLL.toluaL_exception(L, e);
			}
		}
	
		[MonoPInvokeCallbackAttribute(typeof(LuaCSFunction))]
		static int set_Item(IntPtr L)
		{
			try
			{
				ToLua.CheckArgsCount(L, 3);
				System.Collections.Generic.Dictionary<int,TestAccount> obj = (System.Collections.Generic.Dictionary<int,TestAccount>)ToLua.CheckObject(L, 1, typeof(System.Collections.Generic.Dictionary<int,TestAccount>));
				int arg0 = (int)LuaDLL.luaL_checknumber(L, 2);
				TestAccount arg1 = (TestAccount)ToLua.CheckObject(L, 3, typeof(TestAccount));
				obj[arg0] = arg1;
				return 0;
			}
			catch(Exception e)
			{
				return LuaDLL.toluaL_exception(L, e);
			}
		}
	
		[MonoPInvokeCallbackAttribute(typeof(LuaCSFunction))]
		static int Add(IntPtr L)
		{
			try
			{
				ToLua.CheckArgsCount(L, 3);
				System.Collections.Generic.Dictionary<int,TestAccount> obj = (System.Collections.Generic.Dictionary<int,TestAccount>)ToLua.CheckObject(L, 1, typeof(System.Collections.Generic.Dictionary<int,TestAccount>));
				int arg0 = (int)LuaDLL.luaL_checknumber(L, 2);
				TestAccount arg1 = (TestAccount)ToLua.CheckObject(L, 3, typeof(TestAccount));
				obj.Add(arg0, arg1);
				return 0;
			}
			catch(Exception e)
			{
				return LuaDLL.toluaL_exception(L, e);
			}
		}
	
		[MonoPInvokeCallbackAttribute(typeof(LuaCSFunction))]
		static int Clear(IntPtr L)
		{
			try
			{
				ToLua.CheckArgsCount(L, 1);
				System.Collections.Generic.Dictionary<int,TestAccount> obj = (System.Collections.Generic.Dictionary<int,TestAccount>)ToLua.CheckObject(L, 1, typeof(System.Collections.Generic.Dictionary<int,TestAccount>));
				obj.Clear();
				return 0;
			}
			catch(Exception e)
			{
				return LuaDLL.toluaL_exception(L, e);
			}
		}
	
		[MonoPInvokeCallbackAttribute(typeof(LuaCSFunction))]
		static int ContainsKey(IntPtr L)
		{
			try
			{
				ToLua.CheckArgsCount(L, 2);
				System.Collections.Generic.Dictionary<int,TestAccount> obj = (System.Collections.Generic.Dictionary<int,TestAccount>)ToLua.CheckObject(L, 1, typeof(System.Collections.Generic.Dictionary<int,TestAccount>));
				int arg0 = (int)LuaDLL.luaL_checknumber(L, 2);
				bool o = obj.ContainsKey(arg0);
				LuaDLL.lua_pushboolean(L, o);
				return 1;
			}
			catch(Exception e)
			{
				return LuaDLL.toluaL_exception(L, e);
			}
		}
	
		[MonoPInvokeCallbackAttribute(typeof(LuaCSFunction))]
		static int ContainsValue(IntPtr L)
		{
			try
			{
				ToLua.CheckArgsCount(L, 2);
				System.Collections.Generic.Dictionary<int,TestAccount> obj = (System.Collections.Generic.Dictionary<int,TestAccount>)ToLua.CheckObject(L, 1, typeof(System.Collections.Generic.Dictionary<int,TestAccount>));
				TestAccount arg0 = (TestAccount)ToLua.CheckObject(L, 2, typeof(TestAccount));
				bool o = obj.ContainsValue(arg0);
				LuaDLL.lua_pushboolean(L, o);
				return 1;
			}
			catch(Exception e)
			{
				return LuaDLL.toluaL_exception(L, e);
			}
		}
	
		[MonoPInvokeCallbackAttribute(typeof(LuaCSFunction))]
		static int GetObjectData(IntPtr L)
		{
			try
			{
				ToLua.CheckArgsCount(L, 3);
				System.Collections.Generic.Dictionary<int,TestAccount> obj = (System.Collections.Generic.Dictionary<int,TestAccount>)ToLua.CheckObject(L, 1, typeof(System.Collections.Generic.Dictionary<int,TestAccount>));
				System.Runtime.Serialization.SerializationInfo arg0 = (System.Runtime.Serialization.SerializationInfo)ToLua.CheckObject(L, 2, typeof(System.Runtime.Serialization.SerializationInfo));
				System.Runtime.Serialization.StreamingContext arg1 = (System.Runtime.Serialization.StreamingContext)ToLua.CheckObject(L, 3, typeof(System.Runtime.Serialization.StreamingContext));
				obj.GetObjectData(arg0, arg1);
				return 0;
			}
			catch(Exception e)
			{
				return LuaDLL.toluaL_exception(L, e);
			}
		}
	
		[MonoPInvokeCallbackAttribute(typeof(LuaCSFunction))]
		static int OnDeserialization(IntPtr L)
		{
			try
			{
				ToLua.CheckArgsCount(L, 2);
				System.Collections.Generic.Dictionary<int,TestAccount> obj = (System.Collections.Generic.Dictionary<int,TestAccount>)ToLua.CheckObject(L, 1, typeof(System.Collections.Generic.Dictionary<int,TestAccount>));
				object arg0 = ToLua.ToVarObject(L, 2);
				obj.OnDeserialization(arg0);
				return 0;
			}
			catch(Exception e)
			{
				return LuaDLL.toluaL_exception(L, e);
			}
		}
	
		[MonoPInvokeCallbackAttribute(typeof(LuaCSFunction))]
		static int Remove(IntPtr L)
		{
			try
			{
				ToLua.CheckArgsCount(L, 2);
				System.Collections.Generic.Dictionary<int,TestAccount> obj = (System.Collections.Generic.Dictionary<int,TestAccount>)ToLua.CheckObject(L, 1, typeof(System.Collections.Generic.Dictionary<int,TestAccount>));
				int arg0 = (int)LuaDLL.luaL_checknumber(L, 2);
				bool o = obj.Remove(arg0);
				LuaDLL.lua_pushboolean(L, o);
				return 1;
			}
			catch(Exception e)
			{
				return LuaDLL.toluaL_exception(L, e);
			}
		}
	
		[MonoPInvokeCallbackAttribute(typeof(LuaCSFunction))]
		static int TryGetValue(IntPtr L)
		{
			try
			{
				ToLua.CheckArgsCount(L, 3);
				System.Collections.Generic.Dictionary<int,TestAccount> obj = (System.Collections.Generic.Dictionary<int,TestAccount>)ToLua.CheckObject(L, 1, typeof(System.Collections.Generic.Dictionary<int,TestAccount>));
				int arg0 = (int)LuaDLL.luaL_checknumber(L, 2);
				TestAccount arg1 = null;
				bool o = obj.TryGetValue(arg0, out arg1);
				LuaDLL.lua_pushboolean(L, o);
				ToLua.PushObject(L, arg1);
				return 2;
			}
			catch(Exception e)
			{
				return LuaDLL.toluaL_exception(L, e);
			}
		}
	
		[MonoPInvokeCallbackAttribute(typeof(LuaCSFunction))]
		static int GetEnumerator(IntPtr L)
		{
			try
			{
				ToLua.CheckArgsCount(L, 1);
				System.Collections.Generic.Dictionary<int,TestAccount> obj = (System.Collections.Generic.Dictionary<int,TestAccount>)ToLua.CheckObject(L, 1, typeof(System.Collections.Generic.Dictionary<int,TestAccount>));
				System.Collections.Generic.Dictionary<int,TestAccount>.Enumerator o = obj.GetEnumerator();
				ToLua.Push(L, o);
				return 1;
			}
			catch(Exception e)
			{
				return LuaDLL.toluaL_exception(L, e);
			}
		}
	
		[MonoPInvokeCallbackAttribute(typeof(LuaCSFunction))]
		static int get_Count(IntPtr L)
		{
			object o = null;
	
			try
			{
				o = ToLua.ToObject(L, 1);
				System.Collections.Generic.Dictionary<int,TestAccount> obj = (System.Collections.Generic.Dictionary<int,TestAccount>)o;
				int ret = obj.Count;
				LuaDLL.lua_pushinteger(L, ret);
				return 1;
			}
			catch(Exception e)
			{
				return LuaDLL.toluaL_exception(L, e, o == null ? "attempt to index Count on a nil value" : e.Message);
			}
		}
	
		[MonoPInvokeCallbackAttribute(typeof(LuaCSFunction))]
		static int get_Comparer(IntPtr L)
		{
			object o = null;
	
			try
			{
				o = ToLua.ToObject(L, 1);
				System.Collections.Generic.Dictionary<int,TestAccount> obj = (System.Collections.Generic.Dictionary<int,TestAccount>)o;
				System.Collections.Generic.IEqualityComparer<int> ret = obj.Comparer;
				ToLua.PushObject(L, ret);
				return 1;
			}
			catch(Exception e)
			{
				return LuaDLL.toluaL_exception(L, e, o == null ? "attempt to index Comparer on a nil value" : e.Message);
			}
		}
	
		[MonoPInvokeCallbackAttribute(typeof(LuaCSFunction))]
		static int get_Keys(IntPtr L)
		{
			object o = null;
	
			try
			{
				o = ToLua.ToObject(L, 1);
				System.Collections.Generic.Dictionary<int,TestAccount> obj = (System.Collections.Generic.Dictionary<int,TestAccount>)o;
				System.Collections.Generic.Dictionary<int,TestAccount>.KeyCollection ret = obj.Keys;
				ToLua.PushObject(L, ret);
				return 1;
			}
			catch(Exception e)
			{
				return LuaDLL.toluaL_exception(L, e, o == null ? "attempt to index Keys on a nil value" : e.Message);
			}
		}
	
		[MonoPInvokeCallbackAttribute(typeof(LuaCSFunction))]
		static int get_Values(IntPtr L)
		{
			object o = null;
	
			try
			{
				o = ToLua.ToObject(L, 1);
				System.Collections.Generic.Dictionary<int,TestAccount> obj = (System.Collections.Generic.Dictionary<int,TestAccount>)o;
				System.Collections.Generic.Dictionary<int,TestAccount>.ValueCollection ret = obj.Values;
				ToLua.PushObject(L, ret);
				return 1;
			}
			catch(Exception e)
			{
				return LuaDLL.toluaL_exception(L, e, o == null ? "attempt to index Values on a nil value" : e.Message);
			}
		}
	}
```

&emsp;


####System_Collections_Generic_KeyValuePair_int_TestAccountWrap.cs

```csharp
	//this source code was auto-generated by tolua#, do not modify it
	using System;
	using LuaInterface;
	
	public class System_Collections_Generic_KeyValuePair_int_TestAccountWrap
	{
		public static void Register(LuaState L)
		{
			L.BeginClass(typeof(System.Collections.Generic.KeyValuePair<int,TestAccount>), null, "KeyValuePair_int_TestAccount");
			L.RegFunction("ToString", ToString);
			L.RegFunction("New", _CreateSystem_Collections_Generic_KeyValuePair_int_TestAccount);
			L.RegFunction("__tostring", Lua_ToString);
			L.RegVar("Key", get_Key, null);
			L.RegVar("Value", get_Value, null);
			L.EndClass();
		}
	
		[MonoPInvokeCallbackAttribute(typeof(LuaCSFunction))]
		static int _CreateSystem_Collections_Generic_KeyValuePair_int_TestAccount(IntPtr L)
		{
			try
			{
				int count = LuaDLL.lua_gettop(L);
	
				if (count == 2)
				{
					int arg0 = (int)LuaDLL.luaL_checknumber(L, 1);
					TestAccount arg1 = (TestAccount)ToLua.CheckObject(L, 2, typeof(TestAccount));
					System.Collections.Generic.KeyValuePair<int,TestAccount> obj = new System.Collections.Generic.KeyValuePair<int,TestAccount>(arg0, arg1);
					ToLua.PushValue(L, obj);
					return 1;
				}
				else if (count == 0)
				{
					System.Collections.Generic.KeyValuePair<int,TestAccount> obj = new System.Collections.Generic.KeyValuePair<int,TestAccount>();
					ToLua.PushValue(L, obj);
					return 1;
				}
				else
				{
					return LuaDLL.luaL_throw(L, "invalid arguments to ctor method: System.Collections.Generic.KeyValuePair<int,TestAccount>.New");
				}
			}
			catch(Exception e)
			{
				return LuaDLL.toluaL_exception(L, e);
			}
		}
	
		[MonoPInvokeCallbackAttribute(typeof(LuaCSFunction))]
		static int ToString(IntPtr L)
		{
			try
			{
				ToLua.CheckArgsCount(L, 1);
				System.Collections.Generic.KeyValuePair<int,TestAccount> obj = (System.Collections.Generic.KeyValuePair<int,TestAccount>)ToLua.CheckObject(L, 1, typeof(System.Collections.Generic.KeyValuePair<int,TestAccount>));
				string o = obj.ToString();
				LuaDLL.lua_pushstring(L, o);
				ToLua.SetBack(L, 1, obj);
				return 1;
			}
			catch(Exception e)
			{
				return LuaDLL.toluaL_exception(L, e);
			}
		}
	
		[MonoPInvokeCallbackAttribute(typeof(LuaCSFunction))]
		static int Lua_ToString(IntPtr L)
		{
			object obj = ToLua.ToObject(L, 1);
	
			if (obj != null)
			{
				LuaDLL.lua_pushstring(L, obj.ToString());
			}
			else
			{
				LuaDLL.lua_pushnil(L);
			}
	
			return 1;
		}
	
		[MonoPInvokeCallbackAttribute(typeof(LuaCSFunction))]
		static int get_Key(IntPtr L)
		{
			object o = null;
	
			try
			{
				o = ToLua.ToObject(L, 1);
				System.Collections.Generic.KeyValuePair<int,TestAccount> obj = (System.Collections.Generic.KeyValuePair<int,TestAccount>)o;
				int ret = obj.Key;
				LuaDLL.lua_pushinteger(L, ret);
				return 1;
			}
			catch(Exception e)
			{
				return LuaDLL.toluaL_exception(L, e, o == null ? "attempt to index Key on a nil value" : e.Message);
			}
		}
	
		[MonoPInvokeCallbackAttribute(typeof(LuaCSFunction))]
		static int get_Value(IntPtr L)
		{
			object o = null;
	
			try
			{
				o = ToLua.ToObject(L, 1);
				System.Collections.Generic.KeyValuePair<int,TestAccount> obj = (System.Collections.Generic.KeyValuePair<int,TestAccount>)o;
				TestAccount ret = obj.Value;
				ToLua.PushObject(L, ret);
				return 1;
			}
			catch(Exception e)
			{
				return LuaDLL.toluaL_exception(L, e, o == null ? "attempt to index Value on a nil value" : e.Message);
			}
		}
	}
```

&emsp;


####System_Collections_Generic_Dictionary_int_TestAccount_KeyCollectionWrap.cs

```csharp
	//this source code was auto-generated by tolua#, do not modify it
	using System;
	using LuaInterface;
	
	public class System_Collections_Generic_Dictionary_int_TestAccount_KeyCollectionWrap
	{
		public static void Register(LuaState L)
		{
			L.BeginClass(typeof(System.Collections.Generic.Dictionary<int,TestAccount>.KeyCollection), typeof(System.Object), "KeyCollection");
			L.RegFunction("CopyTo", CopyTo);
			L.RegFunction("GetEnumerator", GetEnumerator);
			L.RegFunction("New", _CreateSystem_Collections_Generic_Dictionary_int_TestAccount_KeyCollection);
			L.RegFunction("__tostring", ToLua.op_ToString);
			L.RegVar("Count", get_Count, null);
			L.EndClass();
		}
	
		[MonoPInvokeCallbackAttribute(typeof(LuaCSFunction))]
		static int _CreateSystem_Collections_Generic_Dictionary_int_TestAccount_KeyCollection(IntPtr L)
		{
			try
			{
				int count = LuaDLL.lua_gettop(L);
	
				if (count == 1)
				{
					System.Collections.Generic.Dictionary<int,TestAccount> arg0 = (System.Collections.Generic.Dictionary<int,TestAccount>)ToLua.CheckObject(L, 1, typeof(System.Collections.Generic.Dictionary<int,TestAccount>));
					System.Collections.Generic.Dictionary<int,TestAccount>.KeyCollection obj = new System.Collections.Generic.Dictionary<int,TestAccount>.KeyCollection(arg0);
					ToLua.PushObject(L, obj);
					return 1;
				}
				else
				{
					return LuaDLL.luaL_throw(L, "invalid arguments to ctor method: System.Collections.Generic.Dictionary<int,TestAccount>.KeyCollection.New");
				}
			}
			catch(Exception e)
			{
				return LuaDLL.toluaL_exception(L, e);
			}
		}
	
		[MonoPInvokeCallbackAttribute(typeof(LuaCSFunction))]
		static int CopyTo(IntPtr L)
		{
			try
			{
				ToLua.CheckArgsCount(L, 3);
				System.Collections.Generic.Dictionary<int,TestAccount>.KeyCollection obj = (System.Collections.Generic.Dictionary<int,TestAccount>.KeyCollection)ToLua.CheckObject(L, 1, typeof(System.Collections.Generic.Dictionary<int,TestAccount>.KeyCollection));
				int[] arg0 = ToLua.CheckNumberArray<int>(L, 2);
				int arg1 = (int)LuaDLL.luaL_checknumber(L, 3);
				obj.CopyTo(arg0, arg1);
				return 0;
			}
			catch(Exception e)
			{
				return LuaDLL.toluaL_exception(L, e);
			}
		}
	
		[MonoPInvokeCallbackAttribute(typeof(LuaCSFunction))]
		static int GetEnumerator(IntPtr L)
		{
			try
			{
				ToLua.CheckArgsCount(L, 1);
				System.Collections.Generic.Dictionary<int,TestAccount>.KeyCollection obj = (System.Collections.Generic.Dictionary<int,TestAccount>.KeyCollection)ToLua.CheckObject(L, 1, typeof(System.Collections.Generic.Dictionary<int,TestAccount>.KeyCollection));
				System.Collections.Generic.Dictionary<int,TestAccount>.KeyCollection.Enumerator o = obj.GetEnumerator();
				ToLua.Push(L, o);
				return 1;
			}
			catch(Exception e)
			{
				return LuaDLL.toluaL_exception(L, e);
			}
		}
	
		[MonoPInvokeCallbackAttribute(typeof(LuaCSFunction))]
		static int get_Count(IntPtr L)
		{
			object o = null;
	
			try
			{
				o = ToLua.ToObject(L, 1);
				System.Collections.Generic.Dictionary<int,TestAccount>.KeyCollection obj = (System.Collections.Generic.Dictionary<int,TestAccount>.KeyCollection)o;
				int ret = obj.Count;
				LuaDLL.lua_pushinteger(L, ret);
				return 1;
			}
			catch(Exception e)
			{
				return LuaDLL.toluaL_exception(L, e, o == null ? "attempt to index Count on a nil value" : e.Message);
			}
		}
	}
```

&emsp;



####System_Collections_Generic_Dictionary_int_TestAccount_ValueCollectionWrap.cs

```csharp
	//this source code was auto-generated by tolua#, do not modify it
	using System;
	using LuaInterface;
	
	public class System_Collections_Generic_Dictionary_int_TestAccount_ValueCollectionWrap
	{
		public static void Register(LuaState L)
		{
			L.BeginClass(typeof(System.Collections.Generic.Dictionary<int,TestAccount>.ValueCollection), typeof(System.Object), "ValueCollection");
			L.RegFunction("CopyTo", CopyTo);
			L.RegFunction("GetEnumerator", GetEnumerator);
			L.RegFunction("New", _CreateSystem_Collections_Generic_Dictionary_int_TestAccount_ValueCollection);
			L.RegFunction("__tostring", Lua_ToString);
			L.RegVar("Count", get_Count, null);
			L.EndClass();
		}
	
		[MonoPInvokeCallbackAttribute(typeof(LuaCSFunction))]
		static int _CreateSystem_Collections_Generic_Dictionary_int_TestAccount_ValueCollection(IntPtr L)
		{
			try
			{
				int count = LuaDLL.lua_gettop(L);
	
				if (count == 1)
				{
					System.Collections.Generic.Dictionary<int,TestAccount> arg0 = (System.Collections.Generic.Dictionary<int,TestAccount>)ToLua.CheckObject(L, 1, typeof(System.Collections.Generic.Dictionary<int,TestAccount>));
					System.Collections.Generic.Dictionary<int,TestAccount>.ValueCollection obj = new System.Collections.Generic.Dictionary<int,TestAccount>.ValueCollection(arg0);
					ToLua.PushObject(L, obj);
					return 1;
				}
				else
				{
					return LuaDLL.luaL_throw(L, "invalid arguments to ctor method: System.Collections.Generic.Dictionary<int,TestAccount>.ValueCollection.New");
				}
			}
			catch(Exception e)
			{
				return LuaDLL.toluaL_exception(L, e);
			}
		}
	
		[MonoPInvokeCallbackAttribute(typeof(LuaCSFunction))]
		static int CopyTo(IntPtr L)
		{
			try
			{
				ToLua.CheckArgsCount(L, 3);
				System.Collections.Generic.Dictionary<int,TestAccount>.ValueCollection obj = (System.Collections.Generic.Dictionary<int,TestAccount>.ValueCollection)ToLua.CheckObject(L, 1, typeof(System.Collections.Generic.Dictionary<int,TestAccount>.ValueCollection));
				TestAccount[] arg0 = ToLua.CheckObjectArray<TestAccount>(L, 2);
				int arg1 = (int)LuaDLL.luaL_checknumber(L, 3);
				obj.CopyTo(arg0, arg1);
				return 0;
			}
			catch(Exception e)
			{
				return LuaDLL.toluaL_exception(L, e);
			}
		}
	
		[MonoPInvokeCallbackAttribute(typeof(LuaCSFunction))]
		static int GetEnumerator(IntPtr L)
		{
			try
			{
				ToLua.CheckArgsCount(L, 1);
				System.Collections.Generic.Dictionary<int,TestAccount>.ValueCollection obj = (System.Collections.Generic.Dictionary<int,TestAccount>.ValueCollection)ToLua.CheckObject(L, 1, typeof(System.Collections.Generic.Dictionary<int,TestAccount>.ValueCollection));
				System.Collections.Generic.Dictionary<int,TestAccount>.ValueCollection.Enumerator o = obj.GetEnumerator();
				ToLua.Push(L, o);
				return 1;
			}
			catch(Exception e)
			{
				return LuaDLL.toluaL_exception(L, e);
			}
		}
	
		[MonoPInvokeCallbackAttribute(typeof(LuaCSFunction))]
		static int Lua_ToString(IntPtr L)
		{
			object obj = ToLua.ToObject(L, 1);
	
			if (obj != null)
			{
				LuaDLL.lua_pushstring(L, obj.ToString());
			}
			else
			{
				LuaDLL.lua_pushnil(L);
			}
	
			return 1;
		}
	
		[MonoPInvokeCallbackAttribute(typeof(LuaCSFunction))]
		static int get_Count(IntPtr L)
		{
			object o = null;
	
			try
			{
				o = ToLua.ToObject(L, 1);
				System.Collections.Generic.Dictionary<int,TestAccount>.ValueCollection obj = (System.Collections.Generic.Dictionary<int,TestAccount>.ValueCollection)o;
				int ret = obj.Count;
				LuaDLL.lua_pushinteger(L, ret);
				return 1;
			}
			catch(Exception e)
			{
				return LuaDLL.toluaL_exception(L, e, o == null ? "attempt to index Count on a nil value" : e.Message);
			}
		}
	}
```

🔚