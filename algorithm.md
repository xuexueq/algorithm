1. 排序算法
2. 剑指offer 66道题目 [在线编程专题](https://www.nowcoder.com/ta/coding-interviews)
3. leetcode easy版

## 时间复杂度和空间复杂度

算法的效率主要由以下两个复杂度来评估：

> 时间复杂度：评估执行程序所需的时间。可以估算出程序对处理器的使用程度。
> 空间复杂度：评估执行程序所需的存储空间。可以估算出程序对计算机内存的使用程度。

设计算法时，时间复杂度要比空间复杂度更容易出问题，所以一般情况一下我们只对时间复杂度进行研究。

一般来说，只要算法中不存在循环语句，其时间复杂度就是Ο(1)

> O(1)<O(logn)<O(n)<O(nlogn)<O(n²)<O(n³)<O(2ⁿ)<O(n!)

[算法的时间复杂度和空间复杂度](https://juejin.im/entry/5a49f7d36fb9a0450a67b269)

[算法复杂度分析](http://www.cnblogs.com/gaochundong/p/complexity_of_algorithms.html)


=================我是分割线=====================

[常见排序算法](https://www.kancloud.cn/kancloud/sort-algorithm/46561)

[JS中可能用得到的全部的排序算法](http://louiszhai.github.io/2016/12/23/sort/#%E6%8A%98%E5%8D%8A%E6%8F%92%E5%85%A5%E6%8E%92%E5%BA%8F)

[The algorithm of sort](https://github.com/damonare/Sorts)

[js系列教程5-数据结构和算法全解](https://blog.csdn.net/luanpeng825485697/article/details/77010306)


## 关于插入排序

插入排序的设计初衷是往有序的数组中快速插入一个新的元素。

插入排序由于操作不尽相同, 可分为 直接插入排序 , 折半插入排序(又称二分插入排序), 链表插入排序 , 希尔排序。


1. 直接插入排序

它的工作原理是通过构建有序序列，对于未排序数据，在已排序序列中从后向前扫描，找到相应位置并插入。

在从后向前扫描过程中，需要反复把已排序元素逐步向后挪位，为最新元素提供插入空间。

* 其中，在每次比较操作发现待插入元素小于等于已排序的元素时，*将已排序的元素移到下一位置，随后将待插入元素插入该位置*，接着再与前面的已排序的元素进行比较，这样做交换操作代价比较大。

```
function directInsertSort(arr) {
	function swap (arr, i, j) {
		let temp = arr[i]
		arr[i] = arr[j]
		arr[j] = temp
	}
	let len = arr.length

	for(let i = 1; i<len; i++) {
		for(let j = i; j>0; j--) {
			if(arr[j-1] > arr[j]) {
				swap(arr, j-1, j)
			}else {
				break
			}
		}
	}
	
	return arr
}

```

* 还有一个做法是，将待插入元素取出，从左到右依次与已排序的元素比较，如果已排序的元素大于待插入元素，那么将该元素移动到下一个位置，接着再与前面的已排序的元素比较，*直到找到已排序的元素小于等于新元素的位置，这时再将新元素插入进去*。

步骤：

> * 从第一个元素开始，该元素可以认为已经被排序；
> * 取出下一个元素，在已经排序的元素序列中从后向前扫描；
> * 如果该元素（已排序）大于新元素，将该元素移到下一位置；
> * 重复步骤3，**直到找到已排序的元素小于或者等于新元素的位置**；
> * 将新元素插入到该位置后；(考虑原数组中两个相同值挨在一起，此时要避免同一个元素赋值给自身)
> * 重复步骤2~5。


```
function directInsertSort(arr) {
	let len = arr.length,
		 index,
		 current
	
	for(let i=1; i<len; i++) {
		index = i - 1    //待比较元素的下标
		current = arr[i]
		
		//对于前面的有序数组，从后往前一个一个与带插入元素进行两两比较，找到插入点
		while(index >=0 && arr[index] > current) {      //前置条件之一:待比较元素比当前元素大
			arr[index + 1] = arr[index]      //将待比较元素后移一位
			index--  //游标前移一位
		}
		
		if(index + 1 != i) {      //避免同一个元素赋值给自身
			arr[index + 1] = current    //将当前元素插入预留空位
		}
		//console.log(arr)
	}
	return arr
}
directInsertSort([2,3,1,9,8,1])

```
或者

```
function directInsertSort(arr) {
	let len = arr.length
	
	for(let i = 1; i<len; i++) {
		let index = arr[i]  //将待插入元素取出
		for(let j = i; j>=0; j--) {
			if(arr[j-1]> index) {
				arr[j] = arr[j-1]
			} else if (index != arr[j]){
				arr[j] = index
				break
			}
		}
	}
	
	return arr
}

```


2. 折半插入排序（二分插入排序）

基本思路：折半插入排序（binary insertion sort）是对插入排序算法的一种改进，由于排序算法过程中，就是不断的依次将元素插入前面已排好序的序列中。由于前半部分为已排好序的数列，这样我们不用按顺序依次寻找插入点，可以*采用折半查找的方法来加快寻找插入点的速度*。

步骤：

> * 将一新的元素插入到有序数组中，寻找插入点时，将待插区域的首元素设置为a[low],末元素设置为a[high],比较时则将待插元素与参考元素a[m]（m=(low+high)/2）相比较；	
> * 如果待插元素比参考元素小，则选择a[low]到a[m-1]为新的插入区域（即high=m-1）,否则选择a[m+1]到a[high]为插入区域：
> * **如此直到low<=high不成立**，即将此位置(low)之后所有元素后移一位，并将新元素插入a[high+1]中。

【注】: x>>1 是位运算中的右移运算, 表示右移一位, 等同于x除以2再取整, 即 x>>1 == Math.floor(x/2) .


```
function binaryInsertSort(arr) {
	let low,
		 high,
		 m,
		 current
		 
	for(let i=1,len=arr.length; i<len; i++) {
		low = 0,
		high = i-1
		current = arr[i]
		
		//找到插入点
		while(high >= low) {
			m = Math.floor((low+high) / 2)  //折半查找
			if(current >= arr[m]) { // 使用 >= 表示当两两相同时，切换到高半区，保证稳定性（移动的次数少）
				low = m + 1
			} else {
				high = m -1
			}
		}
		
		//将插入点到待插入元素之间的所有元素依次往后移一位，此时low也即high+1，使用arr[high+1]也行
		for(let j = i; j>low; j--) {
			arr[j] =arr[j-1]
		}
		
		arr[low] = current // 要把arr[i]提前保存下来，这里写arr[low] = arr[i]会出现问题，原因是此时arr[i]已经被替换成他值了
		console.log(arr)
	}
	
	return arr
}
binaryInsertSort(5,4,3,2,1)
binaryInsertSort(2,3,1,9,8,1)
```
算法分析：

> 最佳情况：T(n) = O(nlogn)
> 最差情况：T(n) = O(n^2)
> 平均情况：T(n) = O(n^2)


3. 希尔排序（又叫做缩小增量排序）（**不稳定**）

是插入排序的一种更高效的改进版本。希尔排序是基于插入排序的以下两点性质而提出改进方法的：

* 插入排序在对几乎已经排好序的数据操作时， 效率高， 即可以达到线性排序的效率。

* 但插入排序一般来说是低效的， 因为插入排序每次只能将数据移动一位。


**思路：**它的作法不是每次一个元素挨一个元素的比较。而是初期选用大跨步（增量较大）间隔比较，使数组项跳跃式接近它的排序位置；然后增量缩小；最后增量为 1 ，这样数组项移动次数大大减少，提高了排序效率。

希尔排序对增量序列的选择没有严格规定。

希尔排序是按照不同步长对元素进行插入排序，当刚开始元素很无序的时候，步长最大，所以插入排序的元素个数很少，速度很快；当元素基本有序了，步长很小，插入排序对于有序的序列效率很高。

由于多次插入排序，我们知道一次插入排序是稳定的，不会改变相同元素的相对顺序，但在不同的插入排序过程中，相同的元素可能在各自的插入排序中移动，最后其稳定性就会被打乱，所以，**Shell排序是不稳定的**。

**步骤**： 

> * 先取一个正整数 d1(初始步长一般为`d1=arr.length/2`), 将步长为 d1 以及 d1 的倍数的数组项看成一组，然后对其进行直接插入排序；
* 然后取 d2 为 d1/2 ；重复上述步骤；
* 直到取 di = 1, 即所有数组项为一组， 进行直接插入排序，此时相当于在对几乎已经排好序的数据进行直接插入排序。


```
function shellSort(arr) {
	function swap(arr, i, j) {
		let temp = arr[i]
		arr[i] = arr[j]
		arr[j] = temp
	}
	let gap = Math.floor(arr.length / 2)
	
	while(gap >0) {
		//直接插入排序
		for(let i = gap,len=arr.length; i<len; i++) {
			for(let j=i; j>0; j-=gap) {  //let j=i
				if(arr[j-gap] > arr[j]) {
					swap(arr, j-gap, j)
				}else { //else
					break;
				}
			}
		}
		gap = Math.floor(gap / 2) //Math.floor
	}
	
	return arr
	
}

```
算法分析：

> 最佳情况：T(n) = O(nlog2 n)
最坏情况：T(n) = O(nlog2 n)
平均情况：T(n) =O(nlog n)

## 选择排序

```

```

## 堆排序（不稳定，相同的两个数字会被交换）（适合大数据量的排序）

1. 关于二叉树

二叉树又分为*完全二叉树*和*满二叉树*

满二叉树: 除叶子结点外的所有结点均有两个子结点。节点数达到最大值。

完全二叉树: 是效率很高的数据结构，堆是一种完全二叉树或者近似完全二叉树，所以效率极高，像十分常用的排序算法、Dijkstra算法、Prim算法等都要用堆才能优化。
若设二叉树的深度为h，除第 h 层外，其它各层 (1～h-1) 的结点数都达到最大个数，第 h 层所有的结点都连续集中在最左边，这就是完全二叉树。

满二叉树一定是完全二叉树，完全二叉树不一定是满二叉树。

满指的是出了叶子节点外每个节点都有两个孩子，而完全的含义则是最后一层没有满。

2. 什么是堆

堆（二叉堆）可以视为一个完全的二叉树，即除了最底层之外，每一层都是满的。使得数组可以用堆来表示，每一个结点对应数组的一个元素。

* **堆和数组的相互关系**

> 对于给定的某个结点的下标i, 可以很容易地计算出该结点的父节点、孩子结点的下标。
> * Parent(i) = Math.floor(i / 2)  // i的父节点的下标
> * Left(i) = 2i    // i的左子节点的下标
> * Right(i) = 2i + 1  // i的右子节点的下标

* 二叉堆一般分为两种：最大堆和最小堆。

最大堆: 
* 最大堆中的最大元素值出现在根结点（堆顶）;
* 堆中每个父节点的元素值都大于等于其孩子结点（如果存在）

最小堆:
* 最小堆中的最小元素值出现在根结点（堆顶）;
* 堆中每个父节点的元素值都小于等于其孩子结点（如果存在）

3. 堆排序原理 

由于数组下标都是从0开始的, 意味着我们的堆数据结构要发生改变。相应地，几个计算公式也要做出调整：
> Parent(i) = Math.floor((i-1) / 2)
Left(i) = 2i + 1
Right(i) = 2(i + 1)

**思路**: 堆排序就是把最大堆堆顶的最大数取出，将剩余的堆继续调整为最大堆，再次将堆顶的最大数取出，这个过程持续到剩余数只有一个时结束。

**步骤**

* 最大堆调整函数
* 创建最大堆数组函数
* 堆排序（一步步将堆缩小）

```
function heapSort(arr) {
	function swap(arr, i, j) {
		let temp = arr[i]
		arr[i] = arr[j]
		arr[j] = temp
	}

	//最大堆调整函数
	function maxHeapify(arr, index, heapSize) {
		let iMax,
			 iLeft,
			 iRight
			 
		//这里可以使用递归。递归调用需要压栈/清栈，和迭代相比，性能上有略微的劣势	 
		while(true) {
			iMax = index   // index为当前结点
			iLeft = 2 * iMax + 1
			iRight = 2 * (iMax + 1)
			
			if(iLeft < heapSize && arr[iLeft] > arr[index]) {//index
				//swap(arr, iMax, iLeft)
				//index = iLeft
				iMax = iLeft
			}
			
			if(iRight < heapSize && arr[iRight] > arr[iMax]) {//iMax
				//swap(arr, iMax, iRight)
				//index = iRight
				iMax = iRight
			}
			
			if(iMax != index) {
				swap(arr, iMax, index)
				index = iMax
			}else {  //记得终止，不然会无限循环，导致内存溢出
				break
			}
		}
	}
	
	//从最后一个非叶子节点开始，自下而上进行最大堆调整（调用最大堆调整函数）
	function buildMaxHeap(arr) {
		let iParent = Math.floor((arr.length - 1) / 2)
		
		for(let i = iParent; i>=0; i--) {  // i>=0
			maxHeapify(arr, i, arr.length)
		}
	}
	
	//对于调整好的最大堆数组，将堆顶元素与堆底元素交换，然后将堆底最后一位元素砍掉（隔离）,产生的新的堆会一点点缩小，此时新的堆只需要对下标为0的元素进行最大堆调整即可。调整完了之后，重复上述步骤，直到堆数组只剩下一位根元素，就完成了对数组从小到大的排序。
	function sort(arr) {
		buildMaxHeap(arr)
		
		let len = arr.length
		for(let i = len - 1; i>0; i--) {
			swap(arr, 0, i)
			maxHeapify(arr, 0, i) // i
		}
		
		return arr
	}
	
	return sort(arr)
	
}

```

算法分析：

> 最佳情况：T(n) = O(nlogn)
最差情况：T(n) = O(nlogn)
平均情况：T(n) = O(nlogn)

## 快速排序

快排*基于冒泡、递归分治*。它是处理大数据最快的排序算法之一。平均时间复杂度很低，在大量随机输入的情况下最坏情况出现的概率是极小的。

思想：通过一趟排序将待排记录分隔成独立的两部分，其中一部分记录的关键字均比另一部分的关键字小（*分区partition操作*），则可分别对这两部分记录继续进行排序，以达到整个序列有序。


1. 但是内存占用比较少。**真正的快排**。

> 时间复杂度: O(nlogn)

* 以要排序的序列中第一个元素为基准pivot

```

```

* 以要排序的序列中最后一个元素为基准pivot

```

```

[常见排序算法 - 快速排序 (Quick Sort)](http://bubkoo.com/2014/01/12/sort-algorithm/quick-sort/)


2. 阮一峰版 比较容易理解，但是内存占用较多。

选取数组中间元素为基准`pivot = Math.floor(arr.length / 2)`

据说这种其实不是真正的快速排序，而是根据分治法加上冒泡以及递归综合起来的一种排序，其时间复杂度依然跟冒泡一样为`O(n^2)`

> 时间复杂度: O(n^2)

```


```

3. 能不能用一行代码写快排 (提示：使用filter)

选取数组第一个元素为基准pivot

```
function quickSort(arr) {
	return arr.length <= 1 ? arr : quickSort(arr.slice(1).filter(item => item < arr[0])).concat(arr[0], quickSort(arr.slice(1).filter(item => item >= arr[0])))
}
// 记得return 
//还有slice对数组截取后递归，否则会无限循环,
//与arr[0]相等的值放在右边保证稳定性

```
选取数组最后一个元素为基准

```
function quickSort(arr) {
  return arr.length <= 1 ? arr : quickSort(arr.slice(0,-1).filter(item => item <= arr[arr.length-1])).concat(arr[arr.length-1], quickSort(arr.slice(0,-1).filter(item => item > arr[arr.length-1])))
}
//与arr[arr.length-1]相等的值放在右边保证稳定性

```

算法分析

> 最佳情况：T(n) = O(nlogn)
最差情况：T(n) = O(n2)
平均情况：T(n) = O(nlogn)

## 应用

1. 输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4。

**算法分析**：

> 基于堆排序算法，构建最大堆。时间复杂度为O(nlogk) = O（k+（n-k）* logk）=O（n * logk）
> 如果用快速排序，时间复杂度为O(nlogn)；
> 如果用冒泡排序，时间复杂度为O(n*k)

**思路一：O（nlogk）的算法，精剧适合处理海量数据**: 利用堆

我们可以这样想，如果有一个容器，可以存放 k 个元素。每次得到一个新元素的时候，如果它比容器内元素的最大值要小，那么就替换容器中的最大值。把数组中的元素循环一次后，容器中的元素就是要求的最小的 k 个值。

那么我们需要一个能快速获取到最大值的容器。我们知道，*如果要获取一个数组中最大（小）的元素，堆的速度是最快的*。那么，为什么不用堆来求取最小的 k 个数呢？

这个方法有两个好处：

* 不会改变原数组的值。
* 适合大文件，如果数组特别大，或者是一个大文件，我们可以按顺序取出一部分，更新容器即可

> 这种思想*适合处理海量数据*，特别是n大k小的情况。这个时候数据不能全部装入内存，所以要求尽可能少地遍历所有数据。*避免了对n-k个数的排序*。

首先创建一个堆大小为k的最大堆，其余数组元素依次与堆顶进行比较，如果比堆顶小，将该元素置换到堆顶（丢弃堆顶元素）。最后对最大堆进行升序处理，输出数组项中前k个元素即可。

**步骤**： 

>* 从k-1的父节点自下向上建立堆大小为k的最大堆(**建立堆大小为k的最大堆**）；，
>* 剩余的元素依次与堆顶进行两两比较，若该元素小于堆顶元素，则将堆顶元素替换为该元素并调整堆大小。（**即剩下的元素要是比堆中最大值还小，说明该最大值肯定不是前k个最小的数**）。
>* 比较结束之后，数组项中最小的k个数都在堆中，要顺序输出这k个数，需要将堆顶与堆底最后一个元素两两交换，并对堆顶进行最大堆调整，随后砍掉堆底最后一个元素（堆底上升），直到最终堆中只剩一个根元素，则表示完成了对堆大小为k的最大堆从小到大升序的排列；(此题没有要求k个数有序，所有此时可以不用排序)
>* 输出前k个数即为该数组的最小的k个数。


```
function GetLeastNumbers_Solution(arr,k) {
  let len = arr.length

  function swap(arr, i ,j) {
    let temp = arr[i]
    arr[i] = arr[j]
    arr[j] = temp
  }

  //最大堆调整
  function maxHeapify(arr, index, HeapSize) {
    let iMax,
        iLeft,
        iRight

    while(true) {
      iMax = index
      iLeft = 2 * index + 1
      iRight = 2 * (index + 1)

      if(iLeft < HeapSize && arr[iLeft] > arr[index]) {
        iMax = iLeft
      }

      if(iRight < HeapSize && arr[iRight] > arr[iMax]) {
        iMax = iRight
      }

      if(iMax != index) {
        swap(arr, iMax, index)
        index = iMax
      }else {
        break
      }
    }
  }

  //建立堆大小为k的最大堆，这里最好将前k个元素取出放入一个新的容器中，就不会改变原数组
  function buildHeap(arr, num) {
    let iParent = Math.floor((num - 1) / 2)

    for(let i = iParent; i>=0; i--) {
      maxHeapify(arr, i, num)
    }
  }
  
  //剩余元素依次与堆顶比较
  function compare(arr, k) {
    buildHeap(arr)

    for(let i = k; i<len; i++) {
      if(arr[i] < arr[0]) {
        arr[0] = arr[i]
        maxHeapify(arr, 0, k)
      }else { //可以不用写break for循环写了break可以终端操作吗
        break
      }
    }

	//对最大堆进行升序处理
    for(let i = k - 1; i > 0; i--) {
      swap(arr, 0, i)
      maxHeapify(arr, 0, i)
    }

    return arr.slice(0, k)
  }

  return compare(arr, k)
}
console.log(GetLeastNumbers_Solution([9,8,7,6,5,4,3,2,1], 4))

```

**思路（二）**
类似于思路一的思想，

首先把序列中前k个数看做是该序列中最小的k个数，对这k个数利用选择排序或者交换排序找到最大值kMax,然后继续遍历剩余的n-k个序列，使其和最大值kMax进行两两比较，若该值小于kMax，用该值替换kMax，并重新找出k个数的kMax，直到遍历完剩余的n-k个数。最后输出前k个数，即为该序列中最小的k个数。

算法分析：

> 时间复杂度为 n * O（k）=O（n*k）

**思路二：O(n)时间算法，只有可以修改输入数组时可用**： 利用快排Partition

快排Partition

**思路三**

按照惯有的思路，先对这个序列按照从小到大排序，然后输出前面的k个数即是最小的k个数。

```
function GetLeastNumbers_Solution(arr, k) {
	arr.sort(function(a, b) {
		return a - b
	})
	
	return k>=arr.length ? [] : arr.slice(0, k)
}
```

**思路四：**

红黑树

2. 对于一个无序数组，数组中元素为互不相同的整数，请返回其中最小的k个数，顺序与原数组中元素顺序一致。
给定一个整数数组A及它的大小n，同时给定k，请返回其中最小的k个数。

[寻找最小的k个数](https://wizardforcel.gitbooks.io/the-art-of-programming-by-july/content/02.01.html	)


## 有关数组的方法

1. `Array.prototype.slice()`

返回一个从开始到结束（*不包含结束*）的数组项*浅拷贝*到一个新数组对象。即*原数组不会被修改*。

【参数】：
不写参数，slice从索引0提取到数组尾项，即浅拷贝一个新数组。

`slice(begin)`一个参数，表示从索引处提取到数组末尾；*如果该参数为负值*，表示从数组的倒数第几个元素开始提取到最后一个元素。

`slice(begin, end)`，表示会提取原数组中索引从begin到end的所有元素（包含begin，但不包含end）;如果该end参数为负数，表示从第几个元素结束抽取（但是不包括这个end元素）

【应用场景】：
> slice 方法可以用来将一个类数组（Array-like）对象/集合转换成一个新数组。

`Array.prototype.slice.call(arguments) //[].slice.call(arguments)`


> 判断一个对象是否是数组类型
`Object.prototype.toString.call(array).slice(8, -1)==="Array"`

`Object.prototype.toString.call(array)`返回字符串`"[object Array]"`

## 对于循环遍历的几种方式（哪些可以break中断跳出循环）




function charSort(str){
 let arr = str.split("");
 let obj=new Map();
 let result=[];
 arr.forEach(item => {
  obj.has(item)? obj.set(item, obj.get(item)+1) : obj.set(item,1);
 })

 
 Array.from(obj.keys()).forEach(item=>{
  let tempArr = new Array(obj.get(item));
  tempArr.fill(item)
  result.push(...tempArr)
 })
 return result.join("")
}

