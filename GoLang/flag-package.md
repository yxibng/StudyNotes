# Golang flag 包使用

## Overview

> flag 包实现了命令行参数的解析.

## Command line flag syntax

```bash
-flag
-flag=x
-flag x  // non-boolean flags only
```

```
go run xxx.go -flag 123
go run xxx.go -flag=true
go run xxx.go -flag 1233
go run xxx.go --flag "some string"
```

## Usage

使用步骤

1. 定义flag
	
	* 通过`flag.String()`, `Bool()`, `Int()` 等`flag.Xxx()`方法，该种方式返回一个相应的指针
	   
	   ```go
	   import "flag"	
	   var ip = flag.Int("flagname", 1234, "help message for flagname")
	   ```
	   
   * 通过`flag.XxxVar()`方法将flag绑定到一个变量，该种方式返回值类型，如
	
		```go
		var flagvar int
		func init() {
			flag.IntVar(&flagvar, "flagname", 1234, "help message for flagname")
		}
	 	```

   * 通过`flag.Var()`绑定自定义类型，自定义类型需要实现Value接口(Receives必须为指针)，如

		```go
		flag.Var(&flagVal, "name", "help message for flagname")
		```
		
		
2. 调用`flag.Parse()`解析命令行参数到对应的flag

	```go
	flag.Parse()
	```
	解析函数将会在碰到第一个非flag命令行参数时停止，非flag命令行参数是指不满足命令行语法的参数，如命令行参数为`cmd --flag=true abc`则第一个非flag命令行参数为“abc”
	
3. 调用Parse解析后，就可以直接使用flag本身(指针类型)或者绑定的变量了(值类型)
	
	```go
	fmt.Println("ip has value ", *ip)
	fmt.Println("flagvar has value ", flagvar)
	```
	还可通过flag.Args(), flag.Arg(i)来获取非flag命令行参数
	

## Example

> flag.go

1. 一个名为`species`的string flag, 默认值为`"gopher"`

	```go
	var species = flag.String("species", "gopher", "the species we are studying")
	```
	执行命令
	
	```bash
	$ go run flag.go -species "go learner"
	$ go run flag.go -species someString
	$ go run flag.go -species=someString
	
	```

2. 两个flag绑定同一个变量,一般用于同时实现完整flag参数和对应简化版flag参数，需要注意初始化顺序和默认值

	```go
	var gopherType string
	
	func init() {
	  const (
	    defaultGopher = "pocket"
	    usage         = "the variety of gopher"
	  )
	  flag.StringVar(&gopherType, "gopher_type", defaultGopher, usage)
	  flag.StringVar(&gopherType, "g", defaultGopher, usage+"(shorthand)")
	}
	```

	执行命令
	
	```bash
	$ go run flag.go -gopher_type somestring
	$ go run flag.go -g someString
	$ go run flag.go -g=someString
	```

3. 用户自定义flag 类型,一个`time.Duration`数组

	```go
	package main
	
	import (
		"errors"
		"flag"
		"fmt"
		"strings"
		"time"
	)
	
	type interval []time.Duration
	
	// String is the method to format the flag's value, part of the flag.Value interface.
	// The String method's output will be used in diagnostics.
	func (i *interval) String() string {
		return fmt.Sprint(*i)
	}
	
	// Set is the method to set the flag value, part of the flag.Value interface.
	// Set's argument is a string to be parsed to set the flag.
	// It's a comma-separated list, so we split it.
	func (i *interval) Set(value string) error {
	
		if len(*i) > 0 {
			return errors.New("interval flag already set")
		}
		for _, dt := range strings.Split(value, ",") {
			duration, err := time.ParseDuration(dt)
			if err != nil {
				return err
			}
			*i = append(*i, duration)
		}
		return nil
	}
	
	// Define a flag to accumulate durations. Because it has a special type,
	// we need to use the Var function and therefore create the flag during
	// init.
	var intervalFlag interval
	
	func init() {
		// Tie the command-line flag to the intervalFlag variable and
		// set a usage message.
		flag.Var(&intervalFlag, "deltaT", "comma-separated list of intervals to use between events")
	}
	
	func main() {
		// All the interesting pieces are with the variables declared above, but
		// to enable the flag package to see the flags defined there, one must
		// execute, typically at the start of main (not init!):
		flag.Parse()
	
		fmt.Println(intervalFlag)
		// We don't run it here because this is not a main function and
		// the testing suite has already parsed the flags.
	}
	```
	
	执行命令
	
	```bash
	$ go run flag.go -deltaT 61m,72h,80s
	[1h1m0s 72h0m0s 1m20s]
	```
	
	















