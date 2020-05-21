# 分布式锁

## 问题
- 是否设置过期时间，如果设置，如何保证 过期在主动释放；如果不设置，如何保证 获取锁进程阻塞/进程down掉 释放锁。
- 在锁中写入过期时间信息，检测并主动释放锁；在获取锁->释放锁中两次redis操作，无法保证独占性。
- redis 单点故障

## 具备属性
- 独占 安全性
- 防锁死 活性
- 容错 淡点故障

## 解决方案

- 单实例redis

	- 使用lua脚本

```
SET resource_name my_random_value NX PX 3000
```
这个命令仅在不存在key的时候才能被执行成功（NX选项），并且这个key有一个30秒的自动失效时间（PX属性）。这个key的值是“my_random_value”(一个随机值），这个值在所有的客户端必须是唯一的，所有同一key的获取者（竞争者）这个值都不能一样。

value的值必须是随机数主要是为了更安全的释放锁，释放锁的时候使用脚本告诉Redis:只有key存在并且存储的值和我指定的值一样才能告诉我删除成功。可以通过以下Lua脚本实现：

```lua
if redis.call("get",KEYS[1]) == ARGV[1] then
    return redis.call("del",KEYS[1])
else
    return 0
end
```

	- 使用redis事物

```redis
watch lock_key
get lock_key

if lock_key == nil||lock_key.expire_time < now {
	multi
	set lock_key {expire_time: now}
	if exec == nil{
		// 获取锁失败
		return false
	} else {
		return true
	}
}else{
	// 没获取到锁
	unwatch
	return 
}

```
- 分布式，Redlock算法，[算法描述](http://redis.cn/topics/distlock.html)、[RedLock安全性](http://antirez.com/news/101)