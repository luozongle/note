[TOC]

## 一、示例
SpringBoot 2.0.x及其以上可以这么玩
下面脚本看起来有点鸡肋，完全是因为springboot 2.1.x 以下不支持set nx ex同时使用

```Java
String script = "return redis.call('SET',KEYS[1], ARGV[1],'EX',ARGV[2],'NX')";
DefaultRedisScript<Boolean> setRedisScript = new DefaultRedisScript<>();
setRedisScript.setScriptText(script);
setRedisScript.setResultType(Boolean.class);
List<String> keys = Collections.singletonList("name");
Boolean result = stringRedisTemplate.execute(setRedisScript, keys, "zhangsan", String.valueOf(100));
```

当然，也可以把配置文件放到单独的一个文件中
```Java
DefaultRedisScript<Boolean> setRedisScript = new DefaultRedisScript<>();
setRedisScript.setScriptSource(new ResourceScriptSource(new ClassPathResource("redis/set.lua")));
setRedisScript.setResultType(Boolean.class);
List<String> keys = Collections.singletonList("name");
Boolean result = stringRedisTemplate.execute(setRedisScript, keys, "zhangsan", String.valueOf(100));
```



但是如果要执行lua脚本，redisTemplate只能用string序列化value，所以如果注入的时候如果没有明确value的RedisSerializer是string，例如下面

```Java
    @Autowired
    private RedisTemplate<String, Object> redisTemplate;
```

就需要类似于下面的方法，手动指定要使用的RedisSerializer
```Java
@SuppressWarnings("unchecked")
public Boolean setNxAlertInterval(Object value, Long interval, TaskType type, Object ... idsForKey) {
    String key = messageCacheKey(type, FunctionType.ALERT_INTERVAL, idsForKey);
    if (interval > 0 ) {
      return redisTemplate.execute(setRedisScript,redisTemplate.getStringSerializer(),
                                   (RedisSerializer<Boolean>) redisTemplate.getValueSerializer(),
                                   Collections.singletonList(key), value.toString(), interval.toString());
    }
    return false;
}

```

