

在Redis执行lua脚本的两种方式：  
1. 直接执行：
   eval "return redis.call('set', KEYS[1], ARGV[1])" 1 lua-key lua-value  
2. 先预编译，再调用执行：  
    - 预编译：script load "lua脚本"  
    - 执行：evalsha sha1串  
    ```shell
        127.0.0.1:6379> script load "return redis.call('set', KEYS[1], ARGV[1])"
        "55b22c0d0cedf3866879ce7c854970626dcef0c3"
        127.0.0.1:6379> evalsha 55b22c0d0cedf3866879ce7c854970626dcef0c3 1 evalsha001 001
        OK
        127.0.0.1:6379> get evalsha001
        "001"
    ```
   

    


