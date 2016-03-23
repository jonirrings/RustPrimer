# 33-05-data-struct-linked_list

tags： RustPrimer

----------------

## 链表
链表是一种物理存储单元上非连续、非顺序的存储结构，数据元素的逻辑顺序是通过链表中的指针链接次序实现的。链表由一系列结点（链表中每一个元素称为结点）组成，结点可以在运行时动态生成。每个结点包括两个部分：一个是存储数据元素的数据域，另一个是存储下一个结点地址的指针域。 相比于线性表顺序结构，操作复杂。由于不必须按顺序存储，链表在插入的时候可以达到O(1)的复杂度，比另一种线性表顺序表快得多，但是查找一个节点或者访问特定编号的节点则需要O(n)的时间，而线性表和顺序表相应的时间复杂度分别是O(logn)和O(1)。

>使用链表结构可以克服数组链表需要预先知道数据大小的缺点，链表结构可以充分利用计算机内存空间，实现灵活的内存动态管理。但是链表失去了数组随机读取的优点，同时链表由于增加了结点的指针域，空间开销比较大。链表最明显的好处就是，常规数组排列关联项目的方式可能不同于这些数据项目在记忆体或磁盘上顺序，数据的存取往往要在不同的排列顺序中转换。链表允许插入和移除表上任意位置上的节点，但是不允许随机存取。链表有很多种不同的类型：单向链表，双向链表以及循环链表。

下面看我们一步步实现链表：

* 定义链表结构

```rust
use List::*;

enum List {
    // Cons: 包含一个元素和一个指向下一个节点的指针的元组结构
    Cons(u32, Box<List>),
    // Nil: 表示一个链表节点的末端
    Nil,
}
```

* 实现链表的方法
```rust
impl List {
    // 创建一个空链表
    fn new() -> List {
        // `Nil` 是 `List`类型的。因为前面我们使用了 `use List::*;`
        // 所以不需要 List::Nil 这样使用
        Nil
    }

    // 在前面加一个元素节点，并且链接旧的链表和返回新的链表
    fn prepend(self, elem: u32) -> List {
        // `Cons` 也是 List 类型的
        Cons(elem, Box::new(self))
    }

    // 返回链表的长度
    fn len(&self) -> u32 {
        // `self` 的类型是 `&List`, `*self` 的类型是 `List`,
        // 匹配一个类型 `T` 好过匹配一个引用 `&T`
        match *self { 
            // 因为`self`是借用的，所以不能转移 tail 的所有权
            // 因此使用 tail 的引用
            Cons(_, ref tail) => 1 + tail.len(),
            // 基本规则：所以空的链表长度都是0
            Nil => 0
        }
    }

    // 返回连链表的字符串表达形式
    fn stringify(&self) -> String {
        match *self {
            Cons(head, ref tail) => {
                // `format!` 和 `print!` 很像
                // 但是返回一个堆上的字符串去替代打印到控制台
                format!("{}, {}", head, tail.stringify())
            },
            Nil => {
                format!("Nil")
            },
        }
    }
}
```

* 代码测试
```rust
fn main() {
    let mut list = List::new();

    list = list.prepend(1);
    list = list.prepend(2);
    list = list.prepend(3);

    println!("linked list has length: {}", list.len());
    println!("{}", list.stringify());
}
```

* 练习
基于以上代码实现一个双向链表。