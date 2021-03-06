## 规律

当n = 1时， 数组arr = [A]，全排列结果为
> [A];

当n = 2时， 数组arr = [A, B]，全排列结果为
> [A, B]
   [B, A];

当n = 3时， 数组arr = [A, B, C]，全排列结果为
> [A, B, C]
   [A, C, B]
   [B, A, C]
   [B, C, A]
   [C, A, B]
   [C, B, A]

到n = 3时可以看出全排列有以下规律

1. 固定第一个元素，将剩下的元素进行全排列处理；
2. 将第一个元素与依次与第i（1<i<=arr.length）个元素互换，将剩下的元素进行全排列处理；
3. 结束

很适合使用递归解决。只要写一个全排列函数permutation，能固定一个下标为i的元素，剩下元素再进行全排列即可。

## js实现

在ES5中使用闭包将全排列函数封装。

``` javascript
// 数组全排列
function Permutation(arr) {
    this.len = 0;    // 存储全排列次数
    this.arr = arr.concat();   // 传入的数组
    this.result = [];    // 存储全排列结果

    // 首次创建对象时初始化方法
    if (typeof Permutation.run == 'undefined') {
        Permutation.prototype.start = function() {
            this.run(0);
        }

        // 递归函数(核心方法)，index为数组下标
        Permutation.prototype.run = function(index) {
            // 单遍历到数组末端时，将结果储存在result数组中，全排列次数加1
            if (index == this.arr.length - 1) {
                this.result.push(this.arr.slice());
                this.len++;
                return;
            }

            for(let i = index; i < this.arr.length; i++) {
                this.swap(this.arr, index, i);      // 与下标为i的元素交换位置
                this.run(index + 1);                // 剩下元素全排列
                this.swap(this.arr, index, i);      // 复原数组
            }
        }

        // 交换位置
        Permutation.prototype.swap = function(array, i, j) {
            var t;
            t = array[i];
            array[i] = array[j]; 
            array[j] = t;
        }
    }
}

var per = new Permutation(['A', 'B', 'C']);
per.start();
console.log(per.result);
console.log(per.len);
// [ [ 'A', 'B', 'C' ],
//   [ 'A', 'C', 'B' ],
//   [ 'B', 'A', 'C' ],
//   [ 'B', 'C', 'A' ],
//   [ 'C', 'B', 'A' ],
//   [ 'C', 'A', 'B' ] ]
// 6
```
以上ES5代码使用动态原型模式创建对象，主要是想让函数封装的尽量像一个类。在ES6中有class，语法可以更加简洁高效。

``` javascript
  // ES6
  class Permutation {
    constructor(arr) {
        this.arr = Array.from(arr);
        this.result = [];
        this.len = 0;
        this.run(0);
    }

    run(index) {
        if (index == this.arr.length - 1) {
            this.result.push(Array.from(this.arr));
            this.len++;
            return;
        }
        for(let i = index; i < this.arr.length; i++) {
            [this.arr[index], this.arr[i]] = [this.arr[i], this.arr[index]];
            this.run(index + 1);
            [this.arr[index], this.arr[i]] = [this.arr[i], this.arr[index]];
        }
    }
  }

  let p = new Permutation(["A", "B", "C"]);
  console.log(p.result);
  console.log(p.len);
  // [ [ 'A', 'B', 'C' ],
  //   [ 'A', 'C', 'B' ],
  //   [ 'B', 'A', 'C' ],
  //   [ 'B', 'C', 'A' ],
  //   [ 'C', 'B', 'A' ],
  //   [ 'C', 'A', 'B' ] ]
  // 6
```

以上就是全排列的js实现。