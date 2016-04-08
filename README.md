# learn_rust
Rust学习笔记,学到哪写到哪,排序不论先后

###字符串操作

字符串分两种: String, &str.

代码中写 "hello" 其实得到的是一个&str. 不是String,类型不一样.

String 是会实际分配一块堆, &str是一个引用,所以String变成&str是非常快速廉价的,所以直接&一个String即可得到&str,【也可以调用String.as_ref()来得到&str],但是&str要通过.to_owned()变成String的时候,其实有一个copy的动作.因此必须明确调用 &str.to_owned()来时刻提醒这里有个昂贵的操作.

String 可以直接与&str相加, 因为实现了一个 Add<&str>的trait. 写起来比较方便:
```rust
let mut s1 = "hello".to_owned();
let s2 = s1 + "world"; //
```

###rust文档查看

* 要看一个对象有那些方法可以调用,需要查看rust的api文档.

一般可以看某个mod 里面有那些静态方法, 或者struct里面有哪些实例方法.

但是有一些特殊的情况,文档中不一定看得到,比如 ToString 这个trait里面, 有to_string()这个方法. 文档下面说明了,对任何实现 Display 这个trait的,都默认实现了, 但是你看任何对象的api,都看不出来可以调用 .to_string() 方法.



###外部包的依赖与引用
通过Cargo.toml中的 [dependencies] 来添加依赖;在程序中通过
```rust
extern crate rand; // 这样就引入到了根mod
```
来引入这个库.

一般多在main.rs或者lib.rs中引入各个库.

但是如果有些库,只有内部某个mod会使用,那么可以在该mod内部进行依赖,如:
```rust
mod {
    extern crate rand; // 这样只在本mod内可见

    use self::rand::*; // 前面加self:: 作用域限定
}
```

事实上这个方法更加推荐,可以缩小对其他mod的污染.


### 宏

* 宏可以展开,调用命令:
```
cargo rustc  -- -Z unstable-options --pretty expanded
```
就可以看到宏被扩展以后的效果. 这样可以根据清楚的知道最终生成的代码是什么样子的.

如果代码中有多个bin或者example, 可以加参数指定:

```
cargo rustc --bin show_people -- -Z unstable-options --pretty expanded
```

这样就对 src/bin/show_people.rs 进行相关的宏展开.
