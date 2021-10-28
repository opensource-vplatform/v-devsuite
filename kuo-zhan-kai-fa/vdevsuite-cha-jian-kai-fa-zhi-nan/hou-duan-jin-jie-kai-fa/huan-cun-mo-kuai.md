---
description: 缓存数据模块包含两个接口ICache、ICacheManager
---

# 缓存模块

首选需要拿到缓存管理接口

```bash
ICacheManager: ICacheManager cacheManager = VDS.getIntance().getCacheManager();
```

获取缓存空间（所有key都在这个空间内部）

缓存有效期不能小于0，如果等于0，就是各缓存实现的默认值。建议不要使用0参数

```java
 ICache cache = cacheManager.getCache("my-cache"/*缓存名称*/, 600/*缓存有效期（秒*/);
```

添加缓存： cache.put("key1", "value1");

获取缓存： cache.get("key1");

移除缓存： cache.remove("key1");

判断key： cache.containsKey("key1");

清空缓存： cache.clear();

```java
ICacheManager cacheManager = VDS.getIntance().getCacheManager();
ICache<String, Serializable> cache = cacheManager.getCache("my-cache"/** 缓存名称 **/, 600/** 缓存有效期（秒） **/);
cache.put("myname", "jiqj");
String name = (String)cache.get("key1");
log.info("我的名字:" + name);
cache.remove("myname");
String name = (String)cache.get("key1");
log.info("我的名字:" + name);

boolean exist = cache.containsKey("myname");
//清空缓存：
// cache.clear(); 比较少使用

```

