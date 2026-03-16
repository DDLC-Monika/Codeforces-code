# 平衡树

## `__gnu_pbds::tree`

```
#include <ext/pb_ds/assoc_container.hpp>  // 因为 tree 定义在这里 所以需要包含这个头文件
#include <ext/pb_ds/tree_policy.hpp>
using namespace __gnu_pbds;
__gnu_pbds::tree<Key, Mapped, Cmp_Fn = std::less<Key>, Tag = rb_tree_tag,
                 Node_Update = null_tree_node_update,
                 Allocator = std::allocator<char>>
```

## 模板形参

- `Key`: 储存的元素类型，如果想要存储多个相同的 `Key` 元素，则需要使用类似于 `std::pair` 和 `struct` 的方法，并配合使用 `lower_bound` 和 `upper_bound` 成员函数进行查找

- `Mapped`: 映射规则（Mapped-Policy）类型，如果要指示关联容器是 **集合**，类似于存储元素在 `std::set` 中，此处填入 `null_type`，低版本 `g++` 此处为 `null_mapped_type`；如果要指示关联容器是 **带值的集合**，类似于存储元素在 `std::map` 中，此处填入类似于 `std::map<Key, Value>` 的 `Value` 类型

- `Cmp_Fn`: 关键字比较函子，例如 `std::less<Key>`

- Tag: 选择使用何种底层数据结构类型，默认是

   

  ```
  rb_tree_tag
  ```

  ．

  ```
  __gnu_pbds
  ```

   

  提供不同的三种平衡树，分别是：

  - `rb_tree_tag`：红黑树，一般使用这个，后两者的性能一般不如红黑树
  - `splay_tree_tag`：splay 树
  - `ov_tree_tag`：有序向量树，只是一个由 `vector` 实现的有序结构，类似于排序的 `vector` 来实现平衡树，性能取决于数据想不想卡你

- `Node_Update`：用于更新节点的策略，默认使用 `null_node_update`，若要使用 `order_of_key` 和 `find_by_order` 方法，需要使用 `tree_order_statistics_node_update`

- `Allocator`：空间分配器类型

## 构造方式



```
__gnu_pbds::tree<std::pair<int, int>, __gnu_pbds::null_type,
                 std::less<std::pair<int, int>>, __gnu_pbds::rb_tree_tag,
                 __gnu_pbds::tree_order_statistics_node_update>
    trr;
```

## 成员函数

- `insert(x)`：向树中插入一个元素 `x`，返回 `std::pair<point_iterator, bool>`，其中第一个元素代表插入位置的迭代器，第二个元素代表是否插入成功．
- `erase(x)`：从树中删除一个元素/迭代器 `x`．如果 `x` 是迭代器，则返回指向 `x` 下一个的迭代器（如果 `x` 是 `end()` 则返回 `end()`）；如果 `x` 是 `Key`，则返回是否删除成功（如果不存在则删除失败）．
- `order_of_key(x)`：返回严格小于 `x` 的元素个数（以 `Cmp_Fn` 作为比较逻辑），即从 0![0](data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7) 开始的排名．
- `find_by_order(x)`：返回 `Cmp_Fn` 比较的排名所对应元素的迭代器．
- `lower_bound(x)`：返回第一个不小于 `x` 的元素所对应的迭代器（以 `Cmp_Fn` 作为比较逻辑）．
- `upper_bound(x)`：返回第一个严格大于 `x` 的元素所对应的迭代器（以 `Cmp_Fn` 作为比较逻辑）．
- `join(x)`：将 `x` 树并入当前树，`x` 树被清空（必须确保两树的 **比较函数** 和 **元素类型** 相同）．
- `split(x,b)`：以 `Cmp_Fn` 比较，小于等于 `x` 的属于当前树，其余的属于 `b` 树．
- `empty()`：返回是否为空．
- `size()`：返回大小．

<details class="warning" data-original-document-end="2281" data-original-document-start="2119" open="" data-review-enabled="true" style="box-sizing: inherit; background-color: rgb(255, 255, 255); border: 0.075rem solid rgb(255, 145, 0); border-radius: 0.2rem; box-shadow: rgba(0, 0, 0, 0.05) 0px 4.4px 11px 0px, rgba(0, 0, 0, 0.1) 0px 0px 1.1px 0px; color: rgba(0, 0, 0, 0.87); display: flow-root; font-size: 0.64rem; margin: 1.5625em 0px 0px; padding: 0px 0.6rem; break-inside: avoid; transition: box-shadow 125ms ease 0s; overflow: visible; font-family: &quot;Fira Sans&quot;, -apple-system, BlinkMacSystemFont, Helvetica, Arial, sans-serif; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary style="box-sizing: border-box; background-color: rgba(255, 145, 0, 0.1); border-top: none; border-right: none; border-bottom: none; border-left: 0.2rem none; border-image: initial; font-weight: 700; margin: 0px -0.6rem; padding: 0.4rem 1.8rem 0.4rem 2rem; position: relative; cursor: pointer; display: block; min-height: 1rem; overflow: hidden; border-top-left-radius: 0.1rem; border-top-right-radius: 0.1rem; -webkit-tap-highlight-color: transparent; outline: none;">注意</summary><p style="box-sizing: border-box;"><code style="box-sizing: inherit; font-feature-settings: &quot;kern&quot;; font-family: &quot;Fira Mono&quot;, SFMono-Regular, Consolas, Menlo, monospace; font-size: 0.85em; color: rgb(54, 70, 78); direction: ltr; font-variant-ligatures: none; background-color: rgb(245, 245, 245); border-radius: 0.1rem; -webkit-box-decoration-break: clone; box-decoration-break: clone; padding: 0px 0.294118em; word-break: break-word; -webkit-tap-highlight-color: transparent; outline: none;">join(x)</code><span>&nbsp;</span>函数需要保证并入树的键的值域与被并入树的键的值域<span>&nbsp;</span><strong style="box-sizing: inherit;">不相交</strong>（也就是说并入树内所有值必须全部大于/小于当前树内的所有值），否则会抛出<span>&nbsp;</span><code style="box-sizing: inherit; font-feature-settings: &quot;kern&quot;; font-family: &quot;Fira Mono&quot;, SFMono-Regular, Consolas, Menlo, monospace; font-size: 0.85em; color: rgb(54, 70, 78); direction: ltr; font-variant-ligatures: none; background-color: rgb(245, 245, 245); border-radius: 0.1rem; -webkit-box-decoration-break: clone; box-decoration-break: clone; padding: 0px 0.294118em; word-break: break-word; -webkit-tap-highlight-color: transparent; outline: none;">join_error</code><span>&nbsp;</span>异常．</p><p style="box-sizing: border-box; margin-bottom: 0.6rem;">如果要合并两棵值域有交集的树，需要将一棵树的元素一一插入到另一棵树中．</p></details>

## 示例



```
// Common Header Simple over C++11
#include <iostream>
using namespace std;
using ll = long long;
using ull = unsigned long long;
using ld = long double;
using pii = pair<int, int>;
#include <ext/pb_ds/assoc_container.hpp>
#include <ext/pb_ds/tree_policy.hpp>
__gnu_pbds::tree<pair<int, int>, __gnu_pbds::null_type, less<pair<int, int>>,
                 __gnu_pbds::rb_tree_tag,
                 __gnu_pbds::tree_order_statistics_node_update>
    trr;

int main() {
  int cnt = 0;
  trr.insert(make_pair(1, cnt++));
  trr.insert(make_pair(5, cnt++));
  trr.insert(make_pair(4, cnt++));
  trr.insert(make_pair(3, cnt++));
  trr.insert(make_pair(2, cnt++));
  // 树上元素 {(1,0), (2,4), (3,3), (4,2), (5,1)}

  auto it = trr.lower_bound(make_pair(2, 0));
  trr.erase(it);
  // 树上元素 {(1,0), (3,3), (4,2), (5,1)}

  // 输出排名 0 1 2 3 中的排名 1 的元素的 first
  auto it2 = trr.find_by_order(1);
  cout << (*it2).first << endl;  // 输出：3

  // 输出其排名
  int pos = trr.order_of_key(*it2);
  cout << pos << endl;  // 输出：1

  // 按照 it2 分裂 trr
  decltype(trr) newtr;
  trr.split(*it2, newtr);
  for (auto i = newtr.begin(); i != newtr.end(); ++i) {
    cout << (*i).first << ' ';  // 输出：4 5
  }
  cout << endl;

  // 将 newtr 树并入 trr 树，newtr 树被清空．
  trr.join(newtr);
  for (auto i = trr.begin(); i != trr.end(); ++i) {
    cout << (*i).first << ' ';  // 输出：1 3 4 5
  }
  cout << endl;
  cout << newtr.size() << endl;  // 输出：0

  return 0;
}
```