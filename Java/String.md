*   Code point 代表一个unicode字符编码
*   Code unit 最小的表示code point的单元
    *   UTF-8：变长字节序列表示字符；某个字符使用（1-4）个字节来表示，那么它的code unit为1字节。
    *   UTF-16：变长字节序列表示字符；使用2个或者4个字节表示字符，code unit为2字节。
    *   UTF-32：定长（4字节）序列表示字符；code unit为4字节。

Java的String使用UTF-16编码方式。

*   `String.length()` 返回字符串中code unit（char）的数量。
*   `String.codePointCount()` 返回字符串中unicode字符数。

