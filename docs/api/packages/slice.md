# Slice

slice 包包含操作切片的方法集合。

<div STYLE="page-break-after: always;"></div>

## 源码:

- [https://github.com/duke-git/lancet/blob/main/slice/slice.go](https://github.com/duke-git/lancet/blob/main/slice/slice.go)
- [https://github.com/duke-git/lancet/blob/main/slice/slice_concurrent.go](https://github.com/duke-git/lancet/blob/main/slice/slice_concurrent.go)

<div STYLE="page-break-after: always;"></div>

## 用法:

```go
import (
    "github.com/duke-git/lancet/v2/slice"
)
```

<div STYLE="page-break-after: always;"></div>

## 目录

-   [AppendIfAbsent](#AppendIfAbsent)
-   [Contain](#Contain)
-   [ContainBy](#ContainBy)
-   [ContainSubSlice](#ContainSubSlice)
-   [Chunk](#Chunk)
-   [Compact](#Compact)
-   [Concat](#Concat)
-   [Count](#Count)
-   [CountBy](#CountBy)
-   [Difference](#Difference)
-   [DifferenceBy](#DifferenceBy)
-   [DifferenceWith](#DifferenceWith)
-   [DeleteAt](#DeleteAt)
-   [DeleteRange](#DeleteRange)
-   [Drop](#Drop)
-   [DropRight](#DropRight)
-   [DropWhile](#DropWhile)
-   [DropRightWhile](#DropRightWhile)
-   [Every](#Every)
-   [Equal](#Equal)
-   [EqualWith](#EqualWith)
-   [EqualUnordered](#EqualUnordered)
-   [Filter](#Filter)
-   [FilterConcurrent](#FilterConcurrent)
-   [Find<sup>deprecated</sup>](#Find)
-   [FindBy](#FindBy)
-   [FindLast<sup>deprecated</sup>](#FindLast)
-   [FindLastBy](#FindLastBy)
-   [Flatten](#Flatten)
-   [FlattenDeep](#FlattenDeep)
-   [ForEach](#ForEach)
-   [ForEachConcurrent](#ForEachConcurrent)
-   [ForEachWithBreak](#ForEachWithBreak)
-   [GroupBy](#GroupBy)
-   [GroupWith](#GroupWith)
-   [IntSlice<sup>deprecated</sup>](#IntSlice)
-   [InterfaceSlice<sup>deprecated</sup>](#InterfaceSlice)
-   [Intersection](#Intersection)
-   [InsertAt](#InsertAt)
-   [IndexOf](#IndexOf)
-   [LastIndexOf](#LastIndexOf)
-   [Map](#Map)
-   [MapConcurrent](#MapConcurrent)
-   [FilterMap](#FilterMap)
-   [FlatMap](#FlatMap)
-   [Merge](#Merge)
-   [Reverse](#Reverse)
-   [ReverseCopy](#ReverseCopy)
-   [Reduce<sup>deprecated</sup>](#Reduce)
-   [ReduceConcurrent](#ReduceConcurrent)
-   [ReduceBy](#ReduceBy)
-   [ReduceRight](#ReduceRight)
-   [Replace](#Replace)
-   [ReplaceAll](#ReplaceAll)
-   [Repeat](#Repeat)
-   [Shuffle](#Shuffle)
-   [ShuffleCopy](#ShuffleCopy)
-   [IsAscending](#IsAscending)
-   [IsDescending](#IsDescending)
-   [IsSorted](#IsSorted)
-   [IsSortedByKey](#IsSortedByKey)
-   [Sort](#Sort)
-   [SortBy](#SortBy)
-   [SortByField](#SortByField)
-   [Some](#Some)
-   [StringSlice<sup>deprecated</sup>](#StringSlice)
-   [SymmetricDifference](#SymmetricDifference)
-   [ToSlice](#ToSlice)
-   [ToSlicePointer](#ToSlicePointer)
-   [Unique](#Unique)
-   [UniqueBy](#UniqueBy)
-   [UniqueByComparator](#UniqueByComparator)
-   [UniqueByField](#UniqueByField)
-   [UniqueByConcurrent](#UniqueByConcurrent)
-   [Union](#Union)
-   [UnionBy](#UnionBy)
-   [UpdateAt](#UpdateAt)
-   [Without](#Without)
-   [KeyBy](#KeyBy)
-   [Join](#Join)
-   [Partition](#Partition)
-   [SetToDefaultIf](#SetToDefaultIf)
-   [Break](#Break)
-   [RightPadding](#RightPadding)
-   [LeftPadding](#LeftPadding)
-   [Frequency](#Frequency)
-   [JoinFunc](#JoinFunc)
-   [ConcatBy](#ConcatBy)


<div STYLE="page-break-after: always;"></div>


## 文档

### <span id="AppendIfAbsent">AppendIfAbsent</span>

<p>当前切片中不包含值时，将该值追加到切片中</p>

<b>函数签名:</b>

```go
func AppendIfAbsent[T comparable](slice []T, item T) []T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/GNdv7Jg2Taj)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    result1 := slice.AppendIfAbsent([]string{"a", "b"}, "b")
    result2 := slice.AppendIfAbsent([]string{"a", "b"}, "c")

    fmt.Println(result1)
    fmt.Println(result2)

    // Output:
    // [a b]
    // [a b c]
}
```

### <span id="Contain">Contain</span>

<p>判断slice是否包含value</p>

<b>函数签名:</b>

```go
func Contain[T comparable](slice []T, target T) bool
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/_454yEHcNjf)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    result1 := slice.Contain([]string{"a", "b", "c"}, "a")
    result2 := slice.Contain([]int{1, 2, 3}, 4)

    fmt.Println(result1)
    fmt.Println(result2)

    // Output:
    // true
    // false
}
```

### <span id="ContainBy">ContainBy</span>

<p>根据predicate函数判断切片是否包含某个值。</p>

<b>函数签名:</b>

```go
func ContainBy[T any](slice []T, predicate func(item T) bool) bool
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/49tkHfX4GNc)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    type foo struct {
        A string
        B int
    }

    array1 := []foo{{A: "1", B: 1}, {A: "2", B: 2}}
    result1 := slice.ContainBy(array1, func(f foo) bool { return f.A == "1" && f.B == 1 })
    result2 := slice.ContainBy(array1, func(f foo) bool { return f.A == "2" && f.B == 1 })

    array2 := []string{"a", "b", "c"}
    result3 := slice.ContainBy(array2, func(t string) bool { return t == "a" })
    result4 := slice.ContainBy(array2, func(t string) bool { return t == "d" })

    fmt.Println(result1)
    fmt.Println(result2)
    fmt.Println(result3)
    fmt.Println(result4)

    // Output:
    // true
    // false
    // true
    // false
}
```

### <span id="ContainSubSlice">ContainSubSlice</span>

<p>判断slice是否包含subslice</p>

<b>函数签名:</b>

```go
func ContainSubSlice[T comparable](slice, subSlice []T) bool
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/bcuQ3UT6Sev)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    result1 := slice.ContainSubSlice([]string{"a", "b", "c"}, []string{"a", "b"})
    result2 := slice.ContainSubSlice([]string{"a", "b", "c"}, []string{"a", "d"})

    fmt.Println(result1)
    fmt.Println(result2)

    // Output:
    // true
    // false
}
```

### <span id="Chunk">Chunk</span>

<p>按照size参数均分slice</p>

<b>函数签名:</b>

```go
func Chunk[T any](slice []T, size int) [][]T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/b4Pou5j2L_C)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    arr := []string{"a", "b", "c", "d", "e"}

    result1 := slice.Chunk(arr, 1)
    result2 := slice.Chunk(arr, 2)
    result3 := slice.Chunk(arr, 3)
    result4 := slice.Chunk(arr, 4)
    result5 := slice.Chunk(arr, 5)

    fmt.Println(result1)
    fmt.Println(result2)
    fmt.Println(result3)
    fmt.Println(result4)
    fmt.Println(result5)

    // Output:
    // [[a] [b] [c] [d] [e]]
    // [[a b] [c d] [e]]
    // [[a b c] [d e]]
    // [[a b c d] [e]]
    // [[a b c d e]]
}
```

### <span id="Compact">Compact</span>

<p>去除slice中的假值（false values are false, nil, 0, ""）</p>

<b>函数签名:</b>

```go
func Compact[T comparable](slice []T) []T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/pO5AnxEr3TK)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    result1 := slice.Compact([]int{0})
    result2 := slice.Compact([]int{0, 1, 2, 3})
    result3 := slice.Compact([]string{"", "a", "b", "0"})
    result4 := slice.Compact([]bool{false, true, true})

    fmt.Println(result1)
    fmt.Println(result2)
    fmt.Println(result3)
    fmt.Println(result4)

    // Output:
    // []
    // [1 2 3]
    // [a b 0]
    // [true true]
}
```

### <span id="Concat">Concat</span>

<p>创建一个新的切片，将传入的切片拼接起来返回。</p>

<b>函数签名:</b>

```go
func Concat[T any](slices ...[]T) []T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/gPt-q7zr5mk)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    result1 := slice.Concat([]int{1, 2}, []int{3, 4})
    result2 := slice.Concat([]string{"a", "b"}, []string{"c"}, []string{"d"})

    fmt.Println(result1)
    fmt.Println(result2)

    // Output:
    // [1 2 3 4]
    // [a b c d]
}
```

### <span id="Count">Count</span>

<p>返回切片中指定元素的个数</p>

<b>函数签名:</b>

```go
func Count[T comparable](slice []T, item T) int
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/Mj4oiEnQvRJ)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    nums := []int{1, 2, 3, 3, 4}

    result1 := slice.Count(nums, 1)
    result2 := slice.Count(nums, 3)

    fmt.Println(result1)
    fmt.Println(result2)

    // Output:
    // 1
    // 2
}
```

### <span id="CountBy">CountBy</span>

<p>遍历切片，对每个元素执行函数predicate. 返回符合函数返回值为true的元素的个数.</p>

<b>函数签名:</b>

```go
func CountBy[T any](slice []T, predicate func(index int, item T) bool) int
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/tHOccTMDZCC)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    nums := []int{1, 2, 3, 4, 5}

    isEven := func(i, num int) bool {
        return num%2 == 0
    }

    result := slice.CountBy(nums, isEven)

    fmt.Println(result)

    // Output:
    // 2
}
```

### <span id="Difference">Difference</span>

<p>创建一个切片，其元素不包含在另一个给定切片中</p>

<b>函数签名:</b>

```go
func Difference[T comparable](slice, comparedSlice []T) []T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/VXvadzLzhDa)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    s1 := []int{1, 2, 3, 4, 5}
    s2 := []int{4, 5, 6}

    result := slice.Difference(s1, s2)

    fmt.Println(result)

    // Output:
    // [1 2 3]
}
```

### <span id="DifferenceBy">DifferenceBy</span>

<p>将两个slice中的每个元素调用iteratee函数，并比较它们的返回值，如果不相等返回在slice中对应的值</p>

<b>函数签名:</b>

```go
func DifferenceBy[T comparable](slice []T, comparedSlice []T, iteratee func(index int, item T) T) []T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/DiivgwM5OnC)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    s1 := []int{1, 2, 3, 4, 5}
    s2 := []int{3, 4, 5}

    addOne := func(i int, v int) int {
        return v + 1
    }

    result := slice.DifferenceBy(s1, s2, addOne)

    fmt.Println(result)

    // Output:
    // [1 2]
}
```

### <span id="DifferenceWith">DifferenceWith</span>

<p>接受比较器函数，该比较器被调用以将切片的元素与值进行比较。 结果值的顺序和引用由第一个切片确定</p>

<b>函数签名:</b>

```go
func DifferenceWith[T any](slice []T, comparedSlice []T, comparator func(value, otherValue T) bool) []T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/v2U2deugKuV)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    s1 := []int{1, 2, 3, 4, 5}
    s2 := []int{4, 5, 6, 7, 8}

    isDouble := func(v1, v2 int) bool {
        return v2 == 2*v1
    }

    result := slice.DifferenceWith(s1, s2, isDouble)

    fmt.Println(result)

    // Output:
    // [1 5]
}
```

### <span id="DeleteAt">DeleteAt</span>

<p>删除切片中指定索引的元素（不修改原切片）。</p>

<b>函数签名:</b>

```go
func DeleteAt[T any](slice []T, index int) []T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/800B1dPBYyd)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    chars := []string{"a", "b", "c", "d", "e"}

    result1 := slice.DeleteAt(chars, 0)
    result2 := slice.DeleteAt(chars, 4)
    result3 := slice.DeleteAt(chars, 5)
    result4 := slice.DeleteAt(chars, 6)

    fmt.Println(result1)
    fmt.Println(result2)
    fmt.Println(result3)
    fmt.Println(result4)

    // Output:
    // [b c d e]
    // [a b c d]
    // [a b c d]
    // [a b c d]

}
```

### <span id="DeleteRange">DeleteRange</span>

<p>删除切片中指定索引范围的元素（不修改原切片）。</p>

<b>函数签名:</b>

```go
func DeleteRange[T any](slice []T, start, end int) []T 
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/945HwiNrnle)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    chars := []string{"a", "b", "c", "d", "e"}

    result1 := DeleteRange(chars, 0, 0)
    result2 := DeleteRange(chars, 0, 1)
    result3 := DeleteRange(chars, 0, 3)
    result4 := DeleteRange(chars, 0, 4)
    result5 := DeleteRange(chars, 0, 5)

    fmt.Println(result1)
    fmt.Println(result2)
    fmt.Println(result3)
    fmt.Println(result4)
    fmt.Println(result5)

    // Output:
    // [a b c d e]
    // [b c d e]
    // [d e]
    // [e]
    // []

}
```

### <span id="Drop">Drop</span>

<p>从切片的头部删除n个元素。</p>

<b>函数签名:</b>

```go
func Drop[T any](slice []T, n int) []T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/jnPO2yQsT8H)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    result1 := slice.Drop([]string{"a", "b", "c"}, 0)
    result2 := slice.Drop([]string{"a", "b", "c"}, 1)
    result3 := slice.Drop([]string{"a", "b", "c"}, -1)
    result4 := slice.Drop([]string{"a", "b", "c"}, 4)

    fmt.Println(result1)
    fmt.Println(result2)
    fmt.Println(result3)
    fmt.Println(result4)

    // Output:
    // [a b c]
    // [b c]
    // [a b c]
    // []
}
```

### <span id="DropRight">DropRight</span>

<p>从切片的尾部删除n个元素。</p>

<b>函数签名:</b>

```go
func DropRight[T any](slice []T, n int) []T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/8bcXvywZezG)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    result1 := slice.DropRight([]string{"a", "b", "c"}, 0)
    result2 := slice.DropRight([]string{"a", "b", "c"}, 1)
    result3 := slice.DropRight([]string{"a", "b", "c"}, -1)
    result4 := slice.DropRight([]string{"a", "b", "c"}, 4)

    fmt.Println(result1)
    fmt.Println(result2)
    fmt.Println(result3)
    fmt.Println(result4)

    // Output:
    // [a b c]
    // [a b]
    // [a b c]
    // []
}
```

### <span id="DropWhile">DropWhile</span>

<p>从切片的头部删除n个元素，这个n个元素满足predicate函数返回true。</p>

<b>函数签名:</b>

```go
func DropWhile[T any](slice []T, predicate func(item T) bool) []T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/4rt252UV_qs)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    result1 := slice.DropWhile(numbers, func(n int) bool {
        return n != 2
    })
    result2 := slice.DropWhile(numbers, func(n int) bool {
        return true
    })
    result3 := slice.DropWhile(numbers, func(n int) bool {
        return n == 0
    })

    fmt.Println(result1)
    fmt.Println(result2)
    fmt.Println(result3)

    // Output:
    // [2 3 4 5]
    // []
    // [1 2 3 4 5]
}
```

### <span id="DropRightWhile">DropRightWhile</span>

<p>从切片的尾部删除n个元素，这个n个元素满足predicate函数返回true。</p>

<b>函数签名:</b>

```go
func DropRightWhile[T any](slice []T, predicate func(item T) bool) []T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/6wyK3zMY56e)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    numbers := []int{1, 2, 3, 4, 5}

    result1 := slice.DropRightWhile(numbers, func(n int) bool {
        return n != 2
    })
    result2 := slice.DropRightWhile(numbers, func(n int) bool {
        return true
    })
    result3 := slice.DropRightWhile(numbers, func(n int) bool {
        return n == 0
    })

    fmt.Println(result1)
    fmt.Println(result2)
    fmt.Println(result3)

    // Output:
    // [1 2]
    // []
    // [1 2 3 4 5]
}
```

### <span id="Every">Every</span>

<p>如果切片中的所有值都通过谓词函数，则返回true。 函数签名应该是func(index int, value any) bool</p>

<b>函数签名:</b>

```go
func Every[T any](slice []T, predicate func(index int, item T) bool) bool
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/R8U6Sl-j8cD)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    nums := []int{1, 2, 3, 5}

    isEven := func(i, num int) bool {
        return num%2 == 0
    }

    result := slice.Every(nums, isEven)

    fmt.Println(result)

    // Output:
    // false
}
```

### <span id="Equal">Equal</span>

<p>检查两个切片是否相等，相等条件：切片长度相同，元素顺序和值都相同</p>

<b>函数签名:</b>

```go
func Equal[T comparable](slice1, slice2 []T) bool
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/WcRQJ37ifPa)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    s1 := []int{1, 2, 3}
    s2 := []int{1, 2, 3}
    s3 := []int{1, 3, 2}

    result1 := slice.Equal(s1, s2)
    result2 := slice.Equal(s1, s3)

    fmt.Println(result1)
    fmt.Println(result2)

    // Output:
    // true
    // false
}
```

### <span id="EqualWith">EqualWith</span>

<p>检查两个切片是否相等，相等条件：对两个切片的元素调用比较函数comparator，返回true</p>

<b>函数签名:</b>

```go
func EqualWith[T, U any](slice1 []T, slice2 []U, comparator func(T, U) bool) bool
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/b9iygtgsHI1)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    s1 := []int{1, 2, 3}
    s2 := []int{2, 4, 6}

    isDouble := func(a, b int) bool {
        return b == a*2
    }

    result := slice.EqualWith(s1, s2, isDouble)

    fmt.Println(result)

    // Output:
    // true
}
```

### <span id="EqualUnordered">EqualUnordered</span>

<p>检查两个切片是否相等，元素数量相同，值相等，不考虑元素顺序。</p>

<b>函数签名:</b>

```go
func EqualUnordered[T comparable](slice1, slice2 []T) bool
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/n8fSc2w8ZgX)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    result1 := slice.EqualUnordered([]int{1, 2, 3}, []int{3, 2, 1})
    result2 := slice.EqualUnordered([]int{1, 2, 3}, []int{4, 5, 6})

    fmt.Println(result1)
    fmt.Println(result2)

    // Output:
    // true
    // false
}
```

### <span id="Filter">Filter</span>

<p>返回切片中通过predicate函数真值测试的所有元素</p>

<b>函数签名:</b>

```go
func Filter[T any](slice []T, predicate func(index int, item T) bool) []T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/SdPna-7qK4T)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    nums := []int{1, 2, 3, 4, 5}

    isEven := func(i, num int) bool {
        return num%2 == 0
    }

    result := slice.Filter(nums, isEven)

    fmt.Println(result)

    // Output:
    // [2 4]
}
```

### <span id="FilterConcurrent">FilterConcurrent</span>

<p>对slice并发执行filter操作。</p>

<b>函数签名:</b>

```go
func FilterConcurrent[T any](slice []T, predicate func(index int, item T) bool, numThreads int) []T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/t_pkwerIRVx)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    nums := []int{1, 2, 3, 4, 5}

    isEven := func(i, num int) bool {
        return num%2 == 0
    }

    result := slice.FilterConcurrent(nums, isEven, 2)

    fmt.Println(result)

    // Output:
    // [2 4]
}
```

### <span id="Find">Find</span>

<p>遍历slice的元素，返回第一个通过predicate函数真值测试的元素</p>

> ⚠️ 本函数已弃用，使用`FindBy`代替。

<b>函数签名:</b>

```go
func Find[T any](slice []T, predicate func(index int, item T) bool) (*T, bool)
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/CBKeBoHVLgq)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    nums := []int{1, 2, 3, 4, 5}

    isEven := func(i, num int) bool {
        return num%2 == 0
    }

    result, ok := slice.Find(nums, isEven)

    fmt.Println(*result)
    fmt.Println(ok)

    // Output:
    // 2
    // true
}
```

### <span id="FindBy">FindBy</span>

<p>遍历slice的元素，返回第一个通过predicate函数真值测试的元素</p>

<b>函数签名:</b>

```go
func FindBy[T any](slice []T, predicate func(index int, item T) bool) (v T, ok bool)
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/n1lysBYl-GB)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    nums := []int{1, 2, 3, 4, 5}

    isEven := func(i, num int) bool {
        return num%2 == 0
    }

    result, ok := slice.FindBy(nums, isEven)

    fmt.Println(result)
    fmt.Println(ok)

    // Output:
    // 2
    // true
}
```

### <span id="FindLast">FindLast</span>

<p>遍历slice的元素，返回最后一个通过predicate函数真值测试的元素。</p>

> ⚠️ 本函数已弃用，使用`FindLastBy`代替。

<b>函数签名:</b>

```go
func FindLast[T any](slice []T, predicate func(index int, item T) bool) (*T, bool)
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/FFDPV_j7URd)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    nums := []int{1, 2, 3, 4, 5}

    isEven := func(i, num int) bool {
        return num%2 == 0
    }

    result, ok := slice.FindLast(nums, isEven)

    fmt.Println(*result)
    fmt.Println(ok)

    // Output:
    // 4
    // true
}
```

### <span id="FindLastBy">FindLastBy</span>

<p>从遍历slice的元素，返回最后一个通过predicate函数真值测试的元素。</p>

<b>函数签名:</b>

```go
func FindLastBy[T any](slice []T, predicate func(index int, item T) bool) (v T, ok bool)
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/8iqomzyCl_s)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    nums := []int{1, 2, 3, 4, 5}

    isEven := func(i, num int) bool {
        return num%2 == 0
    }

    result, ok := slice.FindLastBy(nums, isEven)

    fmt.Println(result)
    fmt.Println(ok)

    // Output:
    // 4
    // true
}
```

### <span id="Flatten">Flatten</span>

<p>将切片压平一层</p>

<b>函数签名:</b>

```go
func Flatten(slice any) any
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/hYa3cBEevtm)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    arrs := [][][]string{{{"a", "b"}}, {{"c", "d"}}}

    result := slice.Flatten(arrs)

    fmt.Println(result)

    // Output:
    // [[a b] [c d]]
}
```

### <span id="FlattenDeep">FlattenDeep</span>

<p>flattens slice recursive.</p>

<b>函数签名:</b>

```go
func FlattenDeep(slice any) any
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/yjYNHPyCFaF)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    arrs := [][][]string{{{"a", "b"}}, {{"c", "d"}}}

    result := slice.FlattenDeep(arrs)

    fmt.Println(result)

    // Output:
    // [a b c d]
}
```

### <span id="ForEach">ForEach</span>

<p>遍历切片的元素并为每个元素调用iteratee函数</p>

<b>函数签名:</b>

```go
func ForEach[T any](slice []T, iteratee func(index int, item T))
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/DrPaa4YsHRF)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    nums := []int{1, 2, 3}

    var result []int
    addOne := func(_ int, v int) {
        result = append(result, v+1)
    }

    slice.ForEach(nums, addOne)

    fmt.Println(result)

    // Output:
    // [2 3 4]
}
```

### <span id="ForEachConcurrent">ForEachConcurrent</span>

<p>对slice并发执行foreach操作。</p>

<b>函数签名:</b>

```go
func ForEachConcurrent[T any](slice []T, iteratee func(index int, item T), numThreads int)
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/kT4XW7DKVoV)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    nums := []int{1, 2, 3, 4, 5, 6, 7, 8}

    result := make([]int, len(nums))

    addOne := func(index int, value int) {
        result[index] = value + 1
    }

    slice.ForEachConcurrent(nums, addOne, 4)

    fmt.Println(result)

    // Output:
    // [2 3 4 5 6 7 8 9]
}
```


### <span id="ForEachWithBreak">ForEachWithBreak</span>

<p>遍历切片的元素并为每个元素调用iteratee函数，当iteratee函数返回false时，终止遍历。</p>

<b>函数签名:</b>

```go
func ForEachWithBreak[T any](slice []T, iteratee func(index int, item T) bool)
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/qScs39f3D9W)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    numbers := []int{1, 2, 3, 4, 5}

    var sum int

    slice.ForEachWithBreak(numbers, func(_, n int) bool {
        if n > 3 {
            return false
        }
        sum += n
        return true
    })

    fmt.Println(sum)

    // Output:
    // 6
}
```

### <span id="GroupBy">GroupBy</span>

<p>迭代切片的元素，每个元素将按条件分组，返回两个切片</p>

<b>函数签名:</b>

```go
func GroupBy[T any](slice []T, groupFn func(index int, item T) bool) ([]T, []T)
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/QVkPxzPR0iA)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    nums := []int{1, 2, 3, 4, 5}

    isEven := func(i, num int) bool {
        return num%2 == 0
    }

    even, odd := slice.GroupBy(nums, isEven)

    fmt.Println(even)
    fmt.Println(odd)

    // Output:
    // [2 4]
    // [1 3 5]
}
```

### <span id="GroupWith">GroupWith</span>

<p>创建一个map，key是iteratee遍历slice中的每个元素返回的结果。 分组值的顺序是由他们出现在slice中的顺序确定的。每个键对应的值负责生成key的元素组成的数组。iteratee调用1个参数： (value)</p>

<b>函数签名:</b>

```go
func GroupWith[T any, U comparable](slice []T, iteratee func(T) U) map[U][]T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/ApCvMNTLO8a)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    nums := []float64{6.1, 4.2, 6.3}

    floor := func(num float64) float64 {
        return math.Floor(num)
    }

    result := slice.GroupWith(nums, floor) //map[float64][]float64

    fmt.Println(result)

    // Output:
    // map[4:[4.2] 6:[6.1 6.3]]
}
```

### <span id="IntSlice">IntSlice</span>

<p>将接口切片转换为int切片</p>

> ⚠️ 本函数已弃用，使用go1.18+泛型代替。

<b>函数签名:</b>

```go
func IntSlice(slice any) []int
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/UQDj-on9TGN)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    nums := []interface{}{1, 2, 3}

    result := slice.IntSlice(nums) //[]int{1, 2, 3}
    fmt.Println(result)

    // Output:
    // [1 2 3]
}
```

### <span id="InterfaceSlice">InterfaceSlice</span>

<p>将值转换为接口切片</p>

> ⚠️ 本函数已弃用，使用go1.18+泛型代替。

<b>函数签名:</b>

```go
func InterfaceSlice(slice any) []any
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/FdQXF0Vvqs-)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    strs := []string{"a", "b", "c"}

    result := slice.InterfaceSlice(strs) //[]interface{}{"a", "b", "c"}
    fmt.Println(result)

    // Output:
    // [a b c]
}
```

### <span id="Intersection">Intersection</span>

<p>多个切片的交集</p>

<b>函数签名:</b>

```go
func Intersection[T comparable](slices ...[]T) []T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/anJXfB5wq_t)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    nums1 := []int{1, 2, 3}
    nums2 := []int{2, 3, 4}

    result := slice.Intersection(nums1, nums2)

    fmt.Println(result)

    // Output:
    // [2 3]
}
```

### <span id="InsertAt">InsertAt</span>

<p>将元素插入到索引处的切片中</p>

<b>函数签名:</b>

```go
func InsertAt[T any](slice []T, index int, value any) []T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/hMLNxPEGJVE)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    result1 := slice.InsertAt([]string{"a", "b", "c"}, 0, "1")
    result2 := slice.InsertAt([]string{"a", "b", "c"}, 1, "1")
    result3 := slice.InsertAt([]string{"a", "b", "c"}, 2, "1")
    result4 := slice.InsertAt([]string{"a", "b", "c"}, 3, "1")
    result5 := slice.InsertAt([]string{"a", "b", "c"}, 0, []string{"1", "2", "3"})

    fmt.Println(result1)
    fmt.Println(result2)
    fmt.Println(result3)
    fmt.Println(result4)
    fmt.Println(result5)

    // Output:
    // [1 a b c]
    // [a 1 b c]
    // [a b 1 c]
    // [a b c 1]
    // [1 2 3 a b c]
}
```

### <span id="IndexOf">IndexOf</span>

<p>返回在切片中找到值的第一个匹配项的索引，如果找不到值，则返回-1</p>

<b>函数签名:</b>

```go
func IndexOf[T comparable](slice []T, item T) int
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/MRN1f0FpABb)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    strs := []string{"a", "a", "b", "c"}

    result1 := slice.IndexOf(strs, "a")
    result2 := slice.IndexOf(strs, "d")

    fmt.Println(result1)
    fmt.Println(result2)

    // Output:
    // 0
    // -1
}
```

### <span id="LastIndexOf">LastIndexOf</span>

<p>返回在切片中找到最后一个值的索引，如果找不到该值，则返回-1</p>

<b>函数签名:</b>

```go
func LastIndexOf[T comparable](slice []T, item T) int
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/DokM4cf1IKH)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    strs := []string{"a", "a", "b", "c"}

    result1 := slice.LastIndexOf(strs, "a")
    result2 := slice.LastIndexOf(strs, "d")

    fmt.Println(result1)
    fmt.Println(result2)

    // Output:
    // 1
    // -1
}
```

### <span id="Map">Map</span>

<p>对slice中的每个元素执行map函数以创建一个新切片。</p>

<b>函数签名:</b>

```go
func Map[T any, U any](slice []T, iteratee func(index int, item T) U) []U
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/biaTefqPquw)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    nums := []int{1, 2, 3}

    addOne := func(_ int, v int) int {
        return v + 1
    }

    result := slice.Map(nums, addOne)

    fmt.Println(result)

    // Output:
    // [2 3 4]
}
```

### <span id="MapConcurrent">MapConcurrent</span>

<p>对slice并发执行map操作。</p>

<b>函数签名:</b>

```go
func MapConcurrent[T any, U any](slice []T, iteratee func(index int, item T) U, numThreads int) []U
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/H1ehfPkPen0)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    nums := []int{1, 2, 3, 4, 5, 6}
    
    result := slice.MapConcurrent(nums, func(_, n int) int {     
        return n * n 
    }, 4)

    fmt.Println(result)

    // Output:
    // [1 4 9 16 25 36]
}
```

### <span id="FilterMap">FilterMap</span>

<p>返回一个将filter和map操作应用于给定切片的切片。 iteratee回调函数应该返回两个值：1，结果值。2，结果值是否应该被包含在返回的切片中。</p>

<b>函数签名:</b>

```go
func FilterMap[T any, U any](slice []T, iteratee func(index int, item T) (U, bool)) []U
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/J94SZ_9MiIe)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    nums := []int{1, 2, 3, 4, 5}

    getEvenNumStr := func(i, num int) (string, bool) {
        if num%2 == 0 {
            return strconv.FormatInt(int64(num), 10), true
        }
        return "", false
    }

    result := slice.FilterMap(nums, getEvenNumStr)

    fmt.Printf("%#v", result)

    // Output:
    // []string{"2", "4"}
}
```

### <span id="FlatMap">FlatMap</span>

<p>将切片转换为其它类型切片。</p>

<b>函数签名:</b>

```go
func FlatMap[T any, U any](slice []T, iteratee func(index int, item T) []U) []U
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/_QARWlWs1N_F)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    nums := []int{1, 2, 3, 4}

    result := slice.FlatMap(nums, func(i int, num int) []string {
        s := "hi-" + strconv.FormatInt(int64(num), 10)
        return []string{s}
    })

    fmt.Printf("%#v", result)

    // Output:
    // []string{"hi-1", "hi-2", "hi-3", "hi-4"}
}
```

### <span id="Merge">Merge</span>

<p>合并多个切片（不会消除重复元素).</p>

> ⚠️ 本函数已弃用，使用`Concat`代替。

<b>函数签名:</b>

```go
func Merge[T any](slices ...[]T) []T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/lbjFp784r9N)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    nums1 := []int{1, 2, 3}
    nums2 := []int{3, 4}

    result := slice.Merge(nums1, nums2)

    fmt.Println(result)

    // Output:
    // [1 2 3 3 4]
}
```

### <span id="Reverse">Reverse</span>

<p>反转切片中的元素顺序</p>

<b>函数签名:</b>

```go
func Reverse[T any](slice []T)
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/8uI8f1lwNrQ)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    strs := []string{"a", "b", "c", "d"}

    slice.Reverse(strs)

    fmt.Println(strs)

    // Output:
    // [d c b a]
}
```

### <span id="ReverseCopy">ReverseCopy</span>

<p>反转切片中的元素顺序, 不改变原slice。</p>

<b>函数签名:</b>

```go
func ReverseCopy[T any](slice []T) []T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/c9arEaP7Cg-)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    strs := []string{"a", "b", "c", "d"}

    reversedStrs := slice.ReverseCopy(strs)

    fmt.Println(reversedStrs)
    fmt.Println(strs)

    // Output:
    // [d c b a]
    // [a b c d]
}
```

### <span id="Reduce">Reduce</span>

<p>将切片中的元素依次运行iteratee函数，返回运行结果。</p>

> ⚠️ 本函数已弃用，使用`ReduceBy`代替。

<b>函数签名:</b>

```go
func Reduce[T any](slice []T, iteratee func(index int, item1, item2 T) T, initial T) T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/_RfXJJWIsIm)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    nums := []int{1, 2, 3}

    sum := func(_ int, v1, v2 int) int {
        return v1 + v2
    }

    result := slice.Reduce(nums, sum, 0)

    fmt.Println(result)

    // Output:
    // 6
}
```

### <span id="ReduceConcurrent">ReduceConcurrent</span>

<p>对切片元素执行并发reduce操作。</p>

<b>函数签名:</b>

```go
func ReduceConcurrent[T any](slice []T, initial T, reducer func(index int, item T, agg T) T, numThreads int) T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/Tjwe6OtaG07)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    nums := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
    
    result := slice.ReduceConcurrent(nums, 0, func(_ int, item, agg int) int {
        return agg + item
    }, 1)

    fmt.Println(result)

    // Output:
    // 55
}
```

### <span id="ReduceBy">ReduceBy</span>

<p>对切片元素执行reduce操作。</p>

<b>函数签名:</b>

```go
func ReduceBy[T any, U any](slice []T, initial U, reducer func(index int, item T, agg U) U) U
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/YKDpLi7gtee)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    result1 := slice.ReduceBy([]int{1, 2, 3, 4}, 0, func(_ int, item int, agg int) int {
        return agg + item
    })

    result2 := slice.ReduceBy([]int{1, 2, 3, 4}, "", func(_ int, item int, agg string) string {
        return agg + fmt.Sprintf("%v", item)
    })

    fmt.Println(result1)
    fmt.Println(result2)

    // Output:
    // 10
    // 1234
}
```

### <span id="ReduceRight">ReduceRight</span>

<p>类似ReduceBy操作，迭代切片元素顺序从右至左。</p>

<b>函数签名:</b>

```go
func ReduceRight[T any, U any](slice []T, initial U, reducer func(index int, item T, agg U) U) U
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/qT9dZC03A1K)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    result := slice.ReduceRight([]int{1, 2, 3, 4}, "", func(_ int, item int, agg string) string {
        return agg + fmt.Sprintf("%v", item)
    })

    fmt.Println(result)

    // Output:
    // 4321
}
```

### <span id="Replace">Replace</span>

<p>返回切片的副本，其中前n个不重叠的old替换为new</p>

<b>函数签名:</b>

```go
func Replace[T comparable](slice []T, old T, new T, n int) []T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/P5mZp7IhOFo)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    strs := []string{"a", "b", "c", "a"}

    result1 := slice.Replace(strs, "a", "x", 0)
    result2 := slice.Replace(strs, "a", "x", 1)
    result3 := slice.Replace(strs, "a", "x", 2)
    result4 := slice.Replace(strs, "a", "x", 3)
    result5 := slice.Replace(strs, "a", "x", -1)

    fmt.Println(result1)
    fmt.Println(result2)
    fmt.Println(result3)
    fmt.Println(result4)
    fmt.Println(result5)

    // Output:
    // [a b c a]
    // [x b c a]
    // [x b c x]
    // [x b c x]
    // [x b c x]
}
```

### <span id="ReplaceAll">ReplaceAll</span>

<p>返回切片的副本，将其中old全部替换为new</p>

<b>函数签名:</b>

```go
func ReplaceAll[T comparable](slice []T, old T, new T) []T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/CzqXMsuYUrx)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    result := slice.ReplaceAll([]string{"a", "b", "c", "a"}, "a", "x")

    fmt.Println(result)

    // Output:
    // [x b c x]
}
```

### <span id="Repeat">Repeat</span>

<p>创建一个切片，包含n个传入的item</p>

<b>函数签名:</b>

```go
func Repeat[T any](item T, n int) []T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/1CbOmtgILUU)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    result := slice.Repeat("a", 3)

    fmt.Println(result)

    // Output:
    // [a a a]
}
```

### <span id="Shuffle">Shuffle</span>

<p>随机打乱切片中的元素顺序。</p>

<b>函数签名:</b>

```go
func Shuffle[T any](slice []T) []T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/YHvhnWGU3Ge)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    nums := []int{1, 2, 3, 4, 5}
    result := slice.Shuffle(nums)

    fmt.Println(res)

    // Output:
    // [3 1 5 4 2] (random order)
}
```

### <span id="ShuffleCopy">ShuffleCopy</span>

<p>随机打乱切片中的元素顺序, 不改变原切片。</p>

<b>函数签名:</b>

```go
func ShuffleCopy[T any](slice []T) []T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/vqDa-Gs1vT0)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    nums := []int{1, 2, 3, 4, 5}
    result := slice.ShuffleCopy(nums)

    fmt.Println(result)
    fmt.Println(nums)

    // Output:
    // [3 1 5 4 2] (random order)
    // [1 2 3 4 5]
}
```

### <span id="IsAscending">IsAscending</span>

<p>检查切片元素是否按升序排列。</p>

<b>函数签名:</b>

```go
func IsAscending[T constraints.Ordered](slice []T) bool
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/9CtsFjet4SH)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    result1 := slice.IsAscending([]int{1, 2, 3, 4, 5})
    result2 := slice.IsAscending([]int{5, 4, 3, 2, 1})
    result3 := slice.IsAscending([]int{2, 1, 3, 4, 5})

    fmt.Println(result1)
    fmt.Println(result2)
    fmt.Println(result3)

    // Output:
    // true
    // false
    // false
}
```

### <span id="IsDescending">IsDescending</span>

<p>检查切片元素是否按降序排列。</p>

<b>函数签名:</b>

```go
func IsDescending[T constraints.Ordered](slice []T) bool
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/U_LljFXma14)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    result1 := slice.IsDescending([]int{5, 4, 3, 2, 1})
    result2 := slice.IsDescending([]int{1, 2, 3, 4, 5})
    result3 := slice.IsDescending([]int{2, 1, 3, 4, 5})

    fmt.Println(result1)
    fmt.Println(result2)
    fmt.Println(result3)

    // Output:
    // true
    // false
    // false
}
```

### <span id="IsSorted">IsSorted</span>

<p>检查切片元素是否是有序的（升序或降序）。</p>

<b>函数签名:</b>

```go
func IsSorted[T constraints.Ordered](slice []T) bool
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/nCE8wPLwSA-)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    result1 := slice.IsSorted([]int{5, 4, 3, 2, 1})
    result2 := slice.IsSorted([]int{1, 2, 3, 4, 5})
    result3 := slice.IsSorted([]int{2, 1, 3, 4, 5})

    fmt.Println(result1)
    fmt.Println(result2)
    fmt.Println(result3)

    // Output:
    // true
    // true
    // false
}
```

### <span id="IsSortedByKey">IsSortedByKey</span>

<p>通过iteratee函数，检查切片元素是否是有序的。</p>

<b>函数签名:</b>

```go
func IsSortedByKey[T any, K constraints.Ordered](slice []T, iteratee func(item T) K) bool
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/tUoGB7DOHI4)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    result1 := slice.IsSortedByKey([]string{"a", "ab", "abc"}, func(s string) int {
        return len(s)
    })
    result2 := slice.IsSortedByKey([]string{"abc", "ab", "a"}, func(s string) int {
        return len(s)
    })
    result3 := slice.IsSortedByKey([]string{"abc", "a", "ab"}, func(s string) int {
        return len(s)
    })

    fmt.Println(result1)
    fmt.Println(result2)
    fmt.Println(result3)

    // Output:
    // true
    // true
    // false
}
```

### <span id="Sort">Sort</span>

<p>对任何有序类型（数字或字符串）的切片进行排序，使用快速排序算法。 默认排序顺序为升序 (asc)，如果需要降序，请将参数 `sortOrder` 设置为 `desc`。 Ordered类型：数字（所有整数浮点数）或字符串。</p>

<b>函数签名:</b>

```go
func Sort[T constraints.Ordered](slice []T, sortOrder ...string)
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/V9AVjzf_4Fk)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    numbers := []int{1, 4, 3, 2, 5}

    slice.Sort(numbers)
    fmt.Println(numbers) // [1 2 3 4 5]

    slice.Sort(numbers, "desc")
    fmt.Println(numbers) // [5 4 3 2 1]

    strings := []string{"a", "d", "c", "b", "e"}

    slice.Sort(strings)
    fmt.Println(strings) //[a b c d e}

    slice.Sort(strings, "desc")
    fmt.Println(strings) //[e d c b a]
}
```

### <span id="SortBy">SortBy</span>

<p>按照less函数确定的升序规则对切片进行排序。排序不保证稳定性</p>

<b>函数签名:</b>

```go
func SortBy[T any](slice []T, less func(a, b T) bool)
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/DAhLQSZEumm)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    numbers := []int{1, 4, 3, 2, 5}

    slice.SortBy(numbers, func(a, b int) bool {
        return a < b
    })
    fmt.Println(numbers) // [1 2 3 4 5]

    type User struct {
        Name string
        Age  uint
    }

    users := []User{
        {Name: "a", Age: 21},
        {Name: "b", Age: 15},
        {Name: "c", Age: 100}}

    slice.SortBy(users, func(a, b User) bool {
        return a.Age < b.Age
    })

    fmt.Printf("sort users by age: %v", users)

    // output
    // [{b 15} {a 21} {c 100}]
}
```

### <span id="SortByField">SortByField</span>

<p>按字段对结构体切片进行排序。slice元素应为struct，排序字段field类型应为int、uint、string或bool。 默认排序类型是升序（asc），如果是降序，设置 sortType 为 desc</p>

<b>函数签名:</b>

```go
func SortByField(slice any, field string, sortType ...string) error
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/fU1prOBP9p1)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    type User struct {
        Name string
        Age  uint
    }

    users := []User{
        {Name: "a", Age: 21},
        {Name: "b", Age: 15},
        {Name: "c", Age: 100}}

    err := slice.SortByField(users, "Age", "desc")
    if err != nil {
        return
    }

    fmt.Println(users)

    // Output:
    // [{c 100} {a 21} {b 15}]
}
```

### <span id="Some">Some</span>

<p>如果列表中的任何值通过谓词函数，则返回true</p>

<b>函数签名:</b>

```go
func Some[T any](slice []T, predicate func(index int, item T) bool) bool
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/4pO9Xf9NDGS)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    nums := []int{1, 2, 3, 5}

    isEven := func(i, num int) bool {
        return num%2 == 0
    }

    result := slice.Some(nums, isEven)

    fmt.Println(result)

    // Output:
    // true
}
```

### <span id="StringSlice">StringSlice</span>

<p>将接口切片转换为字符串切片</p>

> ⚠️ 本函数已弃用，使用go1.18+泛型代替。

<b>函数签名:</b>

```go
func StringSlice(slice any) []string
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/W0TZDWCPFcI)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    strs := []interface{}{"a", "b", "c"}

    result := slice.StringSlice(strs) //[]string{"a", "b", "c"}
    fmt.Println(result)

    // Output:
    // [a b c]
}
```

### <span id="SymmetricDifference">SymmetricDifference</span>

<p>返回一个切片，其中的元素存在于参数切片中，但不同时存储在于参数切片中（交集取反）</p>

<b>函数签名:</b>

```go
func SymmetricDifference[T comparable](slices ...[]T) []T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/1CbOmtgILUU)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    nums1 := []int{1, 2, 3}
    nums2 := []int{1, 2, 4}

    result := slice.SymmetricDifference(nums1, nums2)

    fmt.Println(result)

    // Output:
    // [3 4]
}
```

### <span id="ToSlice">ToSlice</span>

<p>将可变参数转为切片</p>

<b>函数签名:</b>

```go
func ToSlice[T any](items ...T) []T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/YzbzVq5kscN)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    result := slice.ToSlice("a", "b", "c")

    fmt.Println(result)

    // Output:
    // [a b c]
}
```

### <span id="ToSlicePointer">ToSlicePointer</span>

<p>将可变参数转为指针切片</p>

<b>函数签名:</b>

```go
func ToSlicePointer[T any](items ...T) []*T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/gx4tr6_VXSF)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    str1 := "a"
    str2 := "b"

    result := slice.ToSlicePointer(str1, str2)

    expect := []*string{&str1, &str2}

    isEqual := reflect.DeepEqual(result, expect)

    fmt.Println(isEqual)

    // Output:
    // true
}
```

### <span id="Unique">Unique</span>

<p>删除切片中的重复元素</p>

<b>函数签名:</b>

```go
func Unique[T comparable](slice []T) []T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/AXw0R3ZTE6a)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    result := slice.Unique([]string{"a", "a", "b"})
    fmt.Println(result)

    // Output:
    // [a b]
}
```

### <span id="UniqueBy">UniqueBy</span>

<p>根据迭代函数返回的值，从输入切片中移除重复元素。此函数保持元素的顺序。</p>

<b>函数签名:</b>

```go
func UniqueBy[T any, U comparable](slice []T, iteratee func(item T) U) []T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/GY7JE4yikrl)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/slice"
)

func main() {
    nums := []int{1, 2, 3, 4, 5, 6}
    result := slice.UniqueBy(nums, func(val int) int {
        return val % 3
    })

    fmt.Println(result)

    // Output:
    // [1 2 3]
}
```

### <span id="UniqueByComparator">UniqueByComparator</span>

<p>使用提供的比较器函数从输入切片中移除重复元素。此函数保持元素的顺序。</p>

<b>函数签名:</b>

```go
func UniqueByComparator[T comparable](slice []T, comparator func(item T, other T) bool) []T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/rwSacr-ZHsR)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/slice"
)

func main() {
    uniqueNums := slice.UniqueByComparator([]int{1, 2, 3, 1, 2, 4, 5, 6, 4}, func(item int, other int) bool {
        return item == other
    })

    caseInsensitiveStrings := slice.UniqueByComparator([]string{"apple", "banana", "Apple", "cherry", "Banana", "date"}, func(item string, other string) bool {
        return strings.ToLower(item) == strings.ToLower(other)
    })

    fmt.Println(uniqueNums)
    fmt.Println(caseInsensitiveStrings)

    // Output:
    // [1 2 3 4 5 6]
    // [apple banana cherry date]
}
```

### <span id="UniqueByConcurrent">UniqueByConcurrent</span>

<p>并发的从输入切片中移除重复元素，结果保持元素的顺序。</p>

<b>函数签名:</b>

```go
func UniqueByConcurrent[T comparable](slice []T, comparator func(item T, other T) bool, numThreads int) []T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/wXZ7LcYRMGL)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/slice"
)

func main() {
    nums := []int{1, 2, 3, 1, 2, 4, 5, 6, 4, 7}
    comparator := func(item int, other int) bool { return item == other }

    result := slice.UniqueByConcurrent(nums, comparator, 4)

    fmt.Println(result)
    // Output:
    // [1 2 3 4 5 6 7]
}
```

### <span id="UniqueByField">UniqueByField</span>

<p>根据struct字段对struct切片去重复。</p>

<b>函数签名:</b>

```go
func UniqueByField[T any](slice []T, field string) ([]T, error)
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/6cifcZSPIGu)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/slice"
)

func main() {
    type User struct {
        ID   int    `json:"id"`
        Name string `json:"name"`
    }

    users := []User{
        {ID: 1, Name: "a"},
        {ID: 2, Name: "b"},
        {ID: 1, Name: "c"},
    }

    result, err := slice.UniqueByField(users, "ID")
    if err != nil {
    }

    fmt.Println(result)

    // Output:
    // [{1 a} {2 b}]
}
```

### <span id="Union">Union</span>

<p>合并多个切片</p>

<b>函数签名:</b>

```go
func Union[T comparable](slices ...[]T) []T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/hfXV1iRIZOf)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    nums1 := []int{1, 3, 4, 6}
    nums2 := []int{1, 2, 5, 6}

    result := slice.Union(nums1, nums2)

    fmt.Println(result)

    // Output:
    // [1 3 4 6 2 5]
}
```

### <span id="UnionBy">UnionBy</span>

<p>对切片的每个元素调用函数后，合并多个切片</p>

<b>函数签名:</b>

```go
func UnionBy[T any, V comparable](predicate func(item T) V, slices ...[]T) []T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/HGKHfxKQsFi)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    nums := []int{1, 2, 3, 4}

    divideTwo := func(n int) int {
        return n / 2
    }
    result := slice.UnionBy(divideTwo, nums)

    fmt.Println(result)

    // Output:
    // [1 2 4]
}
```

### <span id="UpdateAt">UpdateAt</span>

<p>更新索引处的切片元素。 如果index &lt 0或 index &lt= len(slice)，将返回错误</p>

<b>函数签名:</b>

```go
func UpdateAt[T any](slice []T, index int, value T) []T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/f3mh2KloWVm)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    result1 := slice.UpdateAt([]string{"a", "b", "c"}, -1, "1")
    result2 := slice.UpdateAt([]string{"a", "b", "c"}, 0, "1")
    result3 := slice.UpdateAt([]string{"a", "b", "c"}, 1, "1")
    result4 := slice.UpdateAt([]string{"a", "b", "c"}, 2, "1")
    result5 := slice.UpdateAt([]string{"a", "b", "c"}, 3, "1")

    fmt.Println(result1)
    fmt.Println(result2)
    fmt.Println(result3)
    fmt.Println(result4)
    fmt.Println(result5)

    // Output:
    // [a b c]
    // [1 b c]
    // [a 1 c]
    // [a b 1]
    // [a b c]
}
```

### <span id="Without">Without</span>

<p>创建一个不包括所有给定值的切片</p>

<b>函数签名:</b>

```go
func Without[T comparable](slice []T, items ...T) []T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/bwhEXEypThg)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    result := slice.Without([]int{1, 2, 3, 4}, 1, 2)

    fmt.Println(result)

    // Output:
    // [3 4]
}
```

### <span id="KeyBy">KeyBy</span>

<p>将切片每个元素调用函数后转为map。</p>

<b>函数签名:</b>

```go
func KeyBy[T any, U comparable](slice []T, iteratee func(item T) U) map[U]T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/uXod2LWD1Kg)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    result := slice.KeyBy([]string{"a", "ab", "abc"}, func(str string) int {
        return len(str)
    })

    fmt.Println(result)

    // Output:
    // map[1:a 2:ab 3:abc]
}
```

### <span id="Join">Join</span>

<p>用指定的分隔符链接切片元素。</p>

<b>函数签名:</b>

```go
func Join[T any](s []T, separator string) string
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/huKzqwNDD7V)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    nums := []int{1, 2, 3, 4, 5}

    result1 := slice.Join(nums, ",")
    result2 := slice.Join(nums, "-")

    fmt.Println(result1)
    fmt.Println(result2)

    // Output:
    // 1,2,3,4,5
    // 1-2-3-4-5
}
```

### <span id="Partition">Partition</span>

<p>根据给定的predicate判断函数分组切片元素。</p>

<b>函数签名:</b>

```go
func Partition[T any](slice []T, predicates ...func(item T) bool) [][]T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/lkQ3Ri2NQhV)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    nums := []int{1, 2, 3, 4, 5}

    result1 := slice.Partition(nums)
    result2 := slice.Partition(nums, func(n int) bool { return n%2 == 0 })
    result3 := slice.Partition(nums, func(n int) bool { return n == 1 || n == 2 }, func(n int) bool { return n == 2 || n == 3 || n == 4 })

    fmt.Println(result1)
    fmt.Println(result2)
    fmt.Println(result3)

    // Output:
    // [[1 2 3 4 5]]
    // [[2 4] [1 3 5]]
    // [[1 2] [3 4] [5]]
}
```


### <span id="Random">Random</span>

<p>随机返回切片中元素以及下标, 当切片长度为0时返回下标-1</p>

<b>函数签名:</b>

```go
func Random[T any](slice []T) (val T, idx int)
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/UzpGQptWppw)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    nums := []int{1, 2, 3, 4, 5}

    val, idx := slice.Random(nums)
    if idx >= 0 && idx < len(nums) && slice.Contain(nums, val) {
        fmt.Println("okk")
    }
    // Output:
    // okk
}
```

### <span id="SetToDefaultIf">SetToDefaultIf</span>

<p>根据给定给定的predicate判定函数来修改切片中的元素。对于满足的元素，将其替换为指定的默认值，同时保持元素在切片中的位置不变。函数返回修改后的切片以及被修改的元素个数。</p>

<b>函数签名:</b>

```go
func SetToDefaultIf[T any](slice []T, predicate func(T) bool) ([]T, int)
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/9AXGlPRC0-A)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    strs := []string{"a", "b", "a", "c", "d", "a"}
    modifiedStrs, count := slice.SetToDefaultIf(strs, func(s string) bool { return "a" == s })
    
    fmt.Println(modifiedStrs)
    fmt.Println(count)
    
    // Output:
    // [ b  c d ]
    // 3
}
```

### <span id="Break">Break</span>

<p>根据判断函数将切片分成两部分。它开始附加到与函数匹配的第一个元素之后的第二个切片。第一个匹配之后的所有元素都包含在第二个切片中，无论它们是否与函数匹配。</p>

<b>函数签名:</b>

```go
func Break[T any](values []T, predicate func(T) bool) ([]T, []T)
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/yLYcBTyeQIz)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    nums := []int{1, 2, 3, 4, 5}
    even := func(n int) bool { return n%2 == 0 }

    resultEven, resultAfterFirstEven := slice.Break(nums, even)
    
    fmt.Println(resultEven)
    fmt.Println(resultAfterFirstEven)
    
    // Output:
    // [1]
    // [2 3 4 5]
}
```

### <span id="RightPadding">RightPadding</span>

<p>在切片的右部添加元素。</p>

<b>函数签名:</b>

```go
func RightPadding[T any](slice []T, paddingValue T, paddingLength int) []T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/0_2rlLEMBXL)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    nums := []int{1, 2, 3, 4, 5}
    padded := slice.RightPadding(nums, 0, 3)
    fmt.Println(padded)
    // Output:
    // [1 2 3 4 5 0 0 0]
}
```

### <span id="LeftPadding">LeftPadding</span>

<p>在切片的左部添加元素。</p>

<b>函数签名:</b>

```go
func LeftPadding[T any](slice []T, paddingValue T, paddingLength int) []T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/jlQVoelLl2k)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    nums := []int{1, 2, 3, 4, 5}
    padded := slice.LeftPadding(nums, 0, 3)
    fmt.Println(padded)
    // Output:
    // [0 0 0 1 2 3 4 5]
}
```

### <span id="Frequency">Frequency</span>

<p>计算切片中每个元素出现的频率。</p>

<b>函数签名:</b>

```go
func Frequency[T comparable](slice []T) map[T]int
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/CW3UVNdUZOq)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    strs := []string{"a", "b", "b", "c", "c", "c"}
    result := slice.Frequency(strs)

    fmt.Println(result)

    // Output:
    // map[a:1 b:2 c:3]
}
```

### <span id="JoinFunc">JoinFunc</span>

<p>将切片元素用给定的分隔符连接成一个单一的字符串。</p>

<b>函数签名:</b>

```go
func JoinFunc[T any](slice []T, sep string, transform func(T) T) string
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/55ib3SB5fM2)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    result := slice.JoinFunc([]string{"a", "b", "c"}, ", ", func(s string) string {
        return strings.ToUpper(s)
    })

    fmt.Println(result)

    // Output:
    // A, B, C
}
```

### <span id="ConcatBy">ConcatBy</span>

<p>将切片中的元素连接成一个值，使用指定的分隔符和连接器函数。</p>

<b>函数签名:</b>

```go
func ConcatBy[T any](slice []T, sep T, connector func(T, T) T) T
```

<b>示例:<span style="float:right;display:inline-block;">[运行](https://go.dev/play/p/6QcUpcY4UMW)</span></b>

```go
import (
    "fmt"
    "github.com/duke-git/lancet/v2/slice"
)

func main() {
    type Person struct {
        Name string
        Age  int
    }

    people := []Person{
        {Name: "Alice", Age: 30},
        {Name: "Bob", Age: 25},
        {Name: "Charlie", Age: 35},
    }

    sep := Person{Name: " | ", Age: 0}

    personConnector := func(a, b Person) Person {
        return Person{Name: a.Name + b.Name, Age: a.Age + b.Age}
    }

    result := slice.ConcatBy(people, sep, personConnector)

    fmt.Println(result.Name)
    fmt.Println(result.Age)

    // Output:
    // Alice | Bob | Charlie
    // 90
}
```