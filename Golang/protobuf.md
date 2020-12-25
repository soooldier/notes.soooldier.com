### 基础

1.  `protoc`命令
    1.  编译命令，根据`.proto`文件转换成对应语言的sdk，但默认不支持go语言，需安装插件`protoc-gen-go`
    2.  例子`protoc --go_out=plugins=grpc,paths=source_relative:. ./proto/*.proto`
2.  `.proto`格式文件
    1.  注意`go_package`的定义，它会对最后生成的go代码import的package产生影响





*   [Protobuf 的 import 功能在 Go 项目中的实践](https://studygolang.com/articles/25743)
*   [protoc-gen-go](https://github.com/golang/protobuf/tree/master/protoc-gen-go)
*   [protobuf与protoc-gen-go](https://studygolang.com/articles/12673)
*   [proto格式规范](https://developers.google.com/protocol-buffers/docs/style)
*   [proto3语法](https://developers.google.com/protocol-buffers/docs/proto3)

