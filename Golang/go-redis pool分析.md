1.  `checkMinIdleConns` 检查最小空闲连接，如不够则创建新连接。

    相关变量：

    *   `idleConnsLen` 空闲连接计数，可能不准
*   `MinIdleConns` 配置变量
    
调用时机：
    
*   连接池创建 `NewConnPool`
    *   获取连接 `popIdle`
    *   连接清除 `removeConn`
    
2.  `ReapStaleConns` 检查是否有过期连接

    相关变量：

    *   `IdleTimeout` 连接空闲时间，上一次操作距离现在的时间，默认5分钟
    *   `IdleCheckFrequency` 空闲连接检查频率，默认1分钟

    调用时机：

    *   连接池创建 `NewConnPool`

    基本流程：

    1.  获取`idleConns`中第一个连接
    2.  判断是否过期连接，依据`IdleTimeout`和`MaxConnAge`
    3.  如不是过期连接，则退出本轮监测（因为第一个连接都不过期，其它的肯定不过期）
    4.  如是过期连接，从`conns`中删掉，且关掉连接。再回到操作1

    

    

    