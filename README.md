# enumgen

本仓库提供为 go 开发中的 “枚举” 类型提供类型转换的生成方式

## 使用场景

日常开发中会这样定义、使用枚举类型

```go
// money.go
type Money int64 

const (
	MoneyCNY Money = iota + 1
	MoneyUSD
)

// service.go
o := Order{
	Amount: req.Amount
	Money: int64(req.Money)
}
```
这里有两个小问题，就是类型每次都是手写，一是看不出原本的类型是什么，二是编码过程是不流畅的

如果把上面强制类型转换的代码改成这样，是不是就好看许多？

```go
// service.go
o := Order{
	Amount: req.Amount
	Money: req.Money.Int64()
}
```
比起明面的强制类型转换，使用方法包装后，行为更加明确，编码过程也更为流畅

## 基础使用

使用下方命令安装
```shell
go install github.com/yikakia/enumgen
```

假设有枚举类型的定义文件
```go
package money
type Money int64

const (
	MoneyCNY Money = iota + 1
	MoneyUSD
)
```
在其对应目录下，使用如下命令
```shell
enumgen -type Money
```

即可为枚举类型生成基础数据类型转换，及 `String() string` 的方法

后续有新增枚举，直接使用 `go generate` 命令即可

## 更多使用
命令格式：
```shell
enumgen [flags] -type T [directory]
enumgen [flags] -type T files... # 必须在同一个包中
```

更多参数:
```
-linecomment
使用行内注释作为 String 方法的展示
-output string
输出的文件名; 默认为 srcdir/<type>_string.go
-tags string
逗号分割的用于 go build 命令的参数
-trimprefix prefix
移除 String 方法中的特定前缀
-type string
逗号分割的一系列名字，必填
```

## 关于
本项目实现参考 [stringer](https://golang.org/x/tools/cmd/stringer)