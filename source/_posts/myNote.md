---
title: myNote
date: 2018-05-01 22:45:21
tags:
---

笔记而已
<!-- more -->

## 冒泡排序：
>&#160; &#160; &#160; &#160;比较相邻的两个数，如果前一个数大于后一个数，就将这两个数换位置。每一次遍历都会将本次遍历最大的数冒泡到最后。
>&#160; &#160; &#160; &#160;为了将n个数排好序，需要n-1次遍历。 如果某次遍历中，没有调整任何两个相邻的数的位置关系，说明此时数组已排好序，可以结束程序。
```js
Array.prototype.bubbleSort = function () {
    let i, j;
    for (i = 1; i < this.length; i++) { //表示本次是第i次遍历
        let changed = false;
        for (j = 0; j < this.length - i; j++) { //访问序列为arr[0:length-i]
            if (this[j] > this[j + 1]) { //发现前一个数大于后一个时，互换位置
                [this[j], this[j + 1]] = [this[j + 1], this[j]];
                changed = true;
            }
        }
        if (!changed) { //如果本轮遍历没有发现位置调整，结束排序函数
            break;
        }
    }
};
let arr = [43, 21, 10, 5, 9, 15, 32, 57, 35];
arr.bubbleSort();
console.log(arr);
```

## 选择排序:
>&#160; &#160; &#160; &#160;第i轮便利, 选出arr[0:n-i]中最大的数, 与arr[n-i]互换
```js
Array.prototype.selectSort = function () {
    let i, j;
    for (i = 1; i < this.length; i++) { //表示本次是第i次遍历
        let maxIndex = 0;
        for (j = 0; j <= this.length - i; j++) { //访问子序列为arr[0:this.length-i]
            if (this[j] > this[maxIndex]) { //当前值大于当前最大值时，记录索引
                maxIndex = j;
            }
        }
        //将子数组最大值索引的值，与子数组末尾的值互换
        [this[this.length - i], this[maxIndex]] = [this[maxIndex], this[this.length - i]]
    }
};
let arr = [43, 21, 10, 5, 9, 15, 32, 57, 35];
arr.selectSort();
console.log(arr);
```

## 插入排序:
>&#160; &#160; &#160; &#160;数组的前面部分已经排好序，要把当前数字插入到前面已排好序的数组的相应位置。
>&#160; &#160; &#160; &#160;可能有人会有疑问为什么默认数组前面部分已排好序？是怎么排好序的？
是因为当排序开始时，从第2个数字开始进行向前插入，此时当前数字索引为1，当前数字前面仅有一个数字，因此可以认为前面部分已经排好序，将这个数字插入到相应位置之后数组仍然是有序的。每次都将当前数字插入到对应的位置，因此每次插入之后前面的数组仍是排好序的。
```js
Array.prototype.insertSort = function () {
    let i, j;
    for (i = 1; i < this.length; i++) { //i表示当前要向前插入的数字的索引，从1(即第2个数)开始前插
        let val = this[i]; //记录当前要前插的数的大小
        /*
         * 用指针j来遍历第i个数字前面的，已经排好序的子数组。当j没有指到头，并且j的数字大于要插入的数字时，说明
         * j还要向前遍历，直到发现一个比要插入数字小的位置pos，然后将这个数字插到pos+1处。如果j已经指到头了，
         * 到了-1了还没有找到比当前数字小的位置，就把当前数字放在索引0处。
         *
        for (j = i - 1; j >= 0 && this[j] > val; j--) {
            this[j + 1] = this[j];
        }
        this[j + 1] = val;
    }
};

let arr = [43, 21, 10, 5, 9, 15, 32, 57, 35];
arr.insertSort();
console.log(arr);
```

## shell排序
>&#160; &#160; &#160; &#160;shell排序 加了step的插入排序。分别以索引数为0,1,......step-1的元素为起点，将其看做不同的组，0、0+step、0+2step、......、0+nstep为一组，1、1+step、1+2step、.....、1+nstep为一组依次分组，按照组为单位进行插入排序。各组都已经插入排序一轮过后，将step除以2向下取整，再进行分组并将各组分别进行插入排序，直到step为0。 step的取值与性能直接相关，需要思考后取值。 并且这里的分组仅仅是逻辑上分组，并没有开辟新的地址空间将其进行物理上的分组。
```js
const {
    floor
} = Math;

//这个和插入排序相同，只不过加了step
Array.prototype.shellInsertSort = function (startIndex, step) {
    let i, j;
    for (i = startIndex + step; i < this.length; i += step) {
        let val = this[i];
        for (j = i - step; j >= 0 && this[j] > val; j -= step) {
            this[j + step] = this[j];
        }
        this[j + step] = val;
    }
};

Array.prototype.shellSort = function () {
    let i, step;
    for (step = floor(this.length / 2); step > 0; step = floor(step / 2)) {
        for (i = 0; i < step; i++) {
            this.shellInsertSort(i, step);
        }
    }
};

let arr = [43, 21, 10, 5, 9, 15, 32, 57, 35];
arr.shellSort(true);
console.log(arr);
```

## 合并排序
>&#160; &#160; &#160; &#160;举个例子： 有 43 12 32 29 66 78 31这个数组要用合并排序。 先将相邻两数分为一组进行合并 43|12 32|29 66|78 31 结果为12 43 29 32 66 78 31再将组的大小乘以二 (12 43|29 32) (66 78|31) 本次合并后结果为 12 29 32 43 31 66 78，再将组的大小乘以二 12 43 29 32 | 66 78 31 合并结果：12 29 31 32 43 66 78，合并的过程中要开辟新的数组arr，建立两个指针i,j分别指向arr1与arr2，此时arr1与arr2都是排好序的，然后每次都将arr1[i]与arr2[j]较小的数加到arr中并将指针后移。最后哪个数组有剩余的数在追加到arr后面。
```js
const {
    min
} = Math;

function merge(arr1, arr2, ) {
    let arr = [];
    let i = 0,
        j = 0;
    while (i < arr1.length && j < arr2.length) {
        arr1[i] < arr2[j] ? arr.push(arr1[i++]) : arr.push(arr2[j++]);
    }
    return i < arr1.length ? arr.concat(arr1.slice(i)) : arr.concat(arr2.slice(j))
}

Array.prototype.mergeSort = function () {
    let groupSize, i, secondPartSize, firstPart, secondPart, totalSize;
    //最初合并时，每组的大小仅为1，然后将组的大小乘以2。
    for (groupSize = 1; groupSize < this.length; groupSize *= 2) {
        for (i = 0; i < this.length; i += 2 * groupSize) {
            //前半段大小一定是groupSize，后半段则不一定
            secondPartSize = min(groupSize, this.length - i - groupSize);
            totalSize = secondPartSize + groupSize;
            //截取前后部分数组，将其排序
            firstPart = this.slice(i, i + groupSize);
            secondPart = this.slice(i + groupSize, i + groupSize + secondPartSize);
            this.splice(i, totalSize, ...merge(firstPart, secondPart));
        }
    }
};

let arr = [43, 21, 10, 5, 9, 15, 32, 57, 35];
arr.mergeSort();
console.log(arr);
```

## 合并排序:
>&#160; &#160; &#160; &#160;举个例子： 有 43 12 32 29 66 78 31这个数组要用合并排序。 先将相邻两数分为一组进行合并 43|12 32|29 66|78 31 结果为12 43 29 32 66 78 31，再将组的大小乘以二 (12 43|29 32) (66 78|31) 本次合并后结果为 12 29 32 43 31 66 78，再将组的大小乘以二 12 43 29 32 | 66 78 31 合并结果：12 29 31 32 43 66 78，合并的过程中要开辟新的数组arr，建立两个指针i,j分别指向arr1与arr2，此时arr1与arr2都是排好序的，然后每次都将arr1[i]与arr2[j]较小的数加到arr中并将指针后移。最后哪个数组有剩余的数在追加到arr后面。
```js
const {
    min
} = Math;

function merge(arr1, arr2, ) {
    let arr = [];
    let i = 0,
        j = 0;
    while (i < arr1.length && j < arr2.length) {
        arr1[i] < arr2[j] ? arr.push(arr1[i++]) : arr.push(arr2[j++]);
    }
    return i < arr1.length ? arr.concat(arr1.slice(i)) : arr.concat(arr2.slice(j))
}

Array.prototype.mergeSort = function () {
    let groupSize, i, secondPartSize, firstPart, secondPart, totalSize;
    //最初合并时，每组的大小仅为1，然后将组的大小乘以2。
    for (groupSize = 1; groupSize < this.length; groupSize *= 2) {
        for (i = 0; i < this.length; i += 2 * groupSize) {
            //前半段大小一定是groupSize，后半段则不一定
            secondPartSize = min(groupSize, this.length - i - groupSize);
            totalSize = secondPartSize + groupSize;
            //截取前后部分数组，将其排序
            firstPart = this.slice(i, i + groupSize);
            secondPart = this.slice(i + groupSize, i + groupSize + secondPartSize);
            this.splice(i, totalSize, ...merge(firstPart, secondPart));
        }
    }
};

let arr = [43, 21, 10, 5, 9, 15, 32, 57, 35];
arr.mergeSort();
console.log(arr);
```

## 自然合并排序
>&#160; &#160; &#160; &#160;合并排序的分组是死板的没有利用到数组中原本就是顺序的子序列。如果数组为 43 56 79 12 33 90 66 将其分组为 43 56 79 | 12 33 90 | 66 再将相邻的，原本就是从小到大的顺序的数组进行合并，效果会更好。
```js
function merge(arr1, arr2) {
    let arr = [],
        i = 0,
        j = 0;
    while (i < arr1.length && j < arr2.length) {
        arr.push(arr1[i] < arr2[j] ? arr1[i++] : arr2[j++])
    }
    return arr.concat(i < arr1.length ? arr1.slice(i) : arr2.slice(j));
}

function getSortedArrList(arr) {
    //记录下已经原本就是从小到大顺序的子数组
    let sortedArrList = [];
    let childArr = [arr[0]];
    for (let i = 1; i < arr.length; i++) {
        //当前值小于上一个值时，将childArr加入sortedArrList中，创建新的childArr，并加入当前值。
        if (arr[i] < arr[i - 1]) {
            sortedArrList.push(childArr);
            childArr = [arr[i]];
        }
        //否则，将当前值加入到childArr中
        else {
            childArr.push(arr[i]);
        }
    }
    sortedArrList.push(childArr);
    return sortedArrList;
}

Array.prototype.naturalMergeSort = function () {
    let sortedArrList = getSortedArrList(this); //获取原本从小到大顺序的子数组
    while (sortedArrList.length > 1) { //当还有两个及以上的数组没合并完成时
        let newSortedArrList = [];
        for (let i = 0; i < sortedArrList.length; i += 2) {
            if (i !== sortedArrList.length - 1) {
                newSortedArrList.push(merge(sortedArrList[i], sortedArrList[i + 1]));
            } else {
                newSortedArrList.push(sortedArrList[i]);
            }
        }
        sortedArrList = newSortedArrList;
    }
    this.splice(0, this.length, ...sortedArrList[0]);
};

let arr = [43, 21, 10, 5, 9, 15, 32, 57, 35];
arr.naturalMergeSort();
console.log(arr);
```

## 基数排序(LSD least significant digit first)
>&#160; &#160; &#160; &#160;LSD中没有数值之间的比较。建立一个[10][]的二维数组arr。挑选出要排序数组中最大的数字，计算该数字的位数记为digitNum。将数组中的所有数字填充到digitNum位，位数不够的高位补0。然后遍历digitNum次，从低位开始。第i次遍历按照将数组中元素的第i位的数值，将元素num放到二维数组相应位置处，如果num第i位数值为n，则执行arr[n].push(num)的操作。每次遍历之后，将arr[0:9]各数组的元素依次取出，并且重新初始化二维数组。直到遍历到最高位为止，再取出的就是已经排好序的。
```js
const {
    max
} = Math;

function initBarrel() {
    let barrel = [];
    for (let i = 0; i < 10; i++) {
        barrel[i] = [];
    }
    return barrel;
}

function radixSort(arr) {
    let barrel = initBarrel();
    let figureNum = max(...arr).toString().length; //计算最大的数字的位数
    arr = arr.map(num => num.toString().padStart(figureNum, '0')); //将数字填充到figureNum位
    for (let i = 0; i < figureNum; i++) {
        let index = figureNum - i - 1; //本次根据第index位来选择放入哪个桶
        arr.forEach(numStr => { //将填充过的数组放入桶中
            let num = Number(numStr[index]);
            barrel[num].push(numStr);
        });
        arr = barrel.reduce((prevArr, curArr) => prevArr.concat(curArr), []); //汇总barrel中的数
        barrel = initBarrel(); //初始化barrel
    }
    return arr.map(num => Number(num)); //最终转为数字形式
}

Array.prototype.radixSort = function () {
    let arr = radixSort(this);
    this.splice(0, this.length, ...arr);
};

let arr = [1234342, 52165, 75, 1, 356, 575, 765433212, 57994, 3535];
arr.radixSort();
console.log(arr);
```

## 基数排序(MSD most significant digit first) 从高位开始，依然没有数值之间的比较。
>&#160; &#160; &#160; &#160;将最初的元素序列按照各元素最高位的数值进行分组，将分组后，组中只有一个元素或者多个相等元素的组拼接到result数组中，而有多个不同元素的组再递归地向下分，取的位次依次减少。
```js
const {
    max
} = Math;

function initBarrel() {
    let barrel = [];
    for (let i = 0; i < 10; i++) {
        barrel[i] = [];
    }
    return barrel;
}
//判断当前桶中是否只有唯一值 有的桶中可能只有一种值，但是有多个重复项
function unique(barrel) {
    return new Set(barrel).size <= 1;
}

Array.prototype.radixSort = function () {
    let result = [];
    let figureNum = max(...this).toString().length;
    this.splice(0, this.length, ...this.map(num => num.toString().padStart(figureNum, '0')));
    radixGroup(this, 0, figureNum, result);
    this.splice(0, this.length, ...result.map(numStr => Number(numStr)));
};

function radixGroup(group, index, figureNum, result) { //输入的group是一组numStr，index是当前分桶依据第几位数
    if (index < figureNum) {
        let barrel = initBarrel();
        group.forEach(numStr => {
            let idx = Number(numStr[index]);
            barrel[idx].push(numStr);
        });
        barrel.forEach(subBarrel => {
            if (unique(subBarrel)) {
                subBarrel.forEach(num => {
                    result.push(num);
                })
            } else {
                radixGroup(subBarrel, index + 1, figureNum, result);
            }
        })
    }
}

let arr = [1234342, 52165, 75, 1, 356, 575, 765433212, 57994, 3535];
arr.radixSort();
console.log(arr);
```

## 快速排序
>&#160; &#160; &#160; &#160;将数组头部的元素pivotNum作为一个基准，通过两个指针指向数组的头部和尾部，经过一次partition以后将pivotNum放在一个位置pivot，pivot前面的数小于pivotNum，后面的数大于pivotNum。 为了防止最坏情况的发生，可以在数组中随机选出一个数来与数组头部元素换位置，来降低具体实例与最坏情况的关联性。
```js
const {
    floor,
    random
} = Math;

function randomIndex(start, end) {
    return floor(random() * (end - start + 1)) + start;
}

function partition(arr, start, end) {
    let index = randomIndex(start, end);
    [arr[start], arr[index]] = [arr[index], arr[start]];
    let value = arr[start];
    while (start < end) {
        while (start < end && arr[end] > value) end--;
        arr[start] = arr[end];
        while (start < end && arr[start] < value) start++;
        arr[end] = arr[start];
    }
    arr[start] = value;
    return start;
}

function quickSort(arr, start, end) {
    if (start < end) {
        let pivot = partition(arr, start, end);
        quickSort(arr, start, pivot - 1);
        quickSort(arr, pivot + 1, end);
    }
}

Array.prototype.quickSort = function (asc = true) {
    quickSort(this, 0, this.length - 1, asc)
};

let arr = [43, 21, 10, 5, 9, 15, 32, 57, 35];
arr.quickSort();
console.log(arr);
```

## 堆排序
>&#160; &#160; &#160; &#160;将数组看做完全二叉树，因此节点i的左右子节点的索引分别为2i+1与2i+2。通过从根节点开始令小的值下沉，或者从最后的叶节点开始令大的值上浮的方法，将一个数组构造成一个大根堆。再将大根堆的头元素与尾元素换位置，这样就将当前最大值置换到了尾部。然后下次构建大根堆的时候，将刚置换过的尾部元素刨除在外不做为节点。
```js
const {
    floor,
    max
} = Math;

function getBiggestNodeIndex(...nodes) {
    return nodes.indexOf(max(...nodes));
}
//将arr从0开始，长度为length的子数组构建为堆
function constructHeap(arr, length) {
    let adjusted = true; //adjusted来标识本次堆是否作出了调整，若未调整说明堆已构建完毕
    while (adjusted) {
        adjusted = false;
        for (let i = 0; i < floor(length / 2); i++) {
            //当只有左节点时
            if (2 * i + 2 === length) {
                //当父节点比左节点小的时候
                if (arr[i] < arr[2 * i + 1]) {
                    //互换
                    [arr[i], arr[2 * i + 1]] = [arr[2 * i + 1], arr[i]];
                    adjusted = true;
                }
            }
            //当同时有左节点和右节点时
            else {
                //判断三个中最大的节点
                let biggestNodeIndex = getBiggestNodeIndex(arr[i], arr[2 * i + 1], arr[2 * i + 2]);
                //若父节点不是最大的，则和最大的交换
                //如果biggestNodeIndex为0，说明自己最大，为1，说明左节点大，为2，说明右节点大
                switch (biggestNodeIndex) {
                    case 0:
                        break;
                    case 1:
                        [arr[i], arr[2 * i + 1]] = [arr[2 * i + 1], arr[i]];
                        adjusted = true;
                        break;
                    case 2:
                        [arr[i], arr[2 * i + 2]] = [arr[2 * i + 2], arr[i]];
                        adjusted = true;
                        break;
                }
            }
        }
    }
}

function heepSort(arr) {
    //只将arr从0开始，长度为length的子数组构建成大根堆
    let length = arr.length;
    while (length > 1) {
        constructHeap(arr, length);
        [arr[0], arr[length-- - 1]] = [arr[length - 1], arr[0]];
    }
}

Array.prototype.heepSort = function () {
    heepSort(this);
};

let arr = [43, 21, 10, 5, 9, 15, 32, 57, 35];
arr.heepSort();
console.log(arr);
```

## 计算字符串长度
```js
function chkstrlen(str) {
    var strlen = 0;
    for (var i = 0; i < str.length; i++) {
        if (str.charCodeAt(i) > 255) {
            strlen += 2;
        } else {
            strlen++;
        }
    }
    return strlen;
}
```

## js中, 类型的相互转换
>1, 对象<-->布尔值

    对象: 先转换为字符串,然后字符串转换为数字,
    布尔值: 转换为数字

>2, 对象<-->字符串

    对象: 转为字符串

>3, 对象<-->数字

    对象: 先转为字符串, 再转为数字

>4, 字符串<-->数字

    字符串: 转为数字

>5, 字符串<-->布尔值

    二者全都转为数字

>6, 布尔值<-->数字

    布尔值: 转为数字

比较规则:
```bash
   对象
     \
      \
       字符串   布尔值
         \       /
          \     /
            数字
```

如果不是同一类型比较:
    对象-->字符串-->数值
    布尔值-->数值

特殊:   undefined == null    // true,  undefined和null,与其他值比较都是false
        undefined === null   // false



## 比较两个数组是否相同
```js
function equalArrays(a, b) {
    if (a.length != b.length) {
        return false;
    }
    for (var i = 0; i < a.length; i++) {
        if (a[i] !== b[i]) {
            return false;
        }
    }
    return true;
}
```


## 删除数组中指定"变量"的成员
```js
Array.prototype.indexOf = function (val) {
    for (var i = 0; i < this.length; i++) {
        if (this[i] == val) {
            return i;
        }
    }
    return -1;
}
Array.prototype.remove = function (val) {
    var index = this.indexOf(val);
    if (index > -1) {
        This.splice(index, 1);
    }
}

var arr = [1, 2, 333, 4, 5];
arr.remove(333);
```


## 常见浅拷贝
```js
function shallowCopy(obj) {
    if (typeof obj !== 'object') {
        return;
    }
    // var newObj = obj instanceof Array ? [] : {};
    var newObj = Object.prototype.toString.call(arr) === '[Object Array]' ? [] : {};
    for (var key in obj) {
        if (obj.hasOwnProperty(key)) {
            newObj[key] == obj[key];
        }
    }
    return newObj;
}
```

## 为什么需要child-process?
>node是异步非阻塞的，这对高并发非常有效．可是我们还有其它一些常用需求，比如和操作系统shell命令交互，调用可执行文件，创建子进进行阻塞式访问或高CPU计算等，child-process就是为满足这些需求而生的


## fs.watch和fs.watchFile有什么区别，怎么应用?
>二者主要用来监听文件变动．fs.watch利用操作系统原生机制来监听，可能不适用网络文件系统; fs.watchFile则是定期检查文件状态变更，适用于网络文件系统， 但是相比fs.watch有些慢，因为不是实时机制．


## express response有哪些常用方法
>res.end() 结束response
>res.download()弹出文件下载
>res.json()返回json
>res.jsonp()返回jsonp
>res.redirect（）重定向请求
>res.send()返回多种形式的数据
>res.sendFile（）返回文件
>res.sendStatus()返回状态

## apache,nginx有什么区别?
>二者都是代理服务器，功能类似．apache应用简单，相当广泛．nginx在分布式，静态转发方面比较有优势．

## nodejs优缺点：
### nodejs优点
>1，nodejs是基于时间驱动和无阻塞的，非常适合处理并发请求
>2，与nodejs代理服务器交互的客户端代码是由js语言编写， 客户端和服务端采用一种语言编写

### nodejs缺点
>&#160; &#160; &#160; &#160;1，node技术是一个相对新的开源项目， 不太稳定，变化速度快
2，不适合cpu密集应用，如果有长时间运行的计算，将会导致cpu时间片不能释放，使得后续的I/O无法发起

## nodejs适用场景
>高并发，聊天，实时消息推送

## npm是什么
npm是nodejs包管理和分发的工具，用于管理node包，如：安装、卸载、发布、查看等

## npm好处
>npm可以安装、管理项目的依赖，且可以指明依赖项目的具体版本

## 设么是前后端分离
>大家一致认同的前后端分离的例子就是SPA(Single-page application)，所有用到的展现数据都是后端通过异步接口(AJAX/JSONP)的方式提供的，前端只管展现。

## SPA式的前后端分离，是从物理层做区分(认为只要是客户端的就是前端，服务器端的就是后端)，
>这种分法已经无法满足我们前后端分离的需求，我们认为从职责上划分才能满足目前我们的使用场景：前端：负责View和Controller层。后端：只负责Model层，业务处理/数据等。

## 什么是错误优先的回调函数？
>错误优先的回调函数(Error-First Callback)用于同时返回错误和数据。第一个参数返回错误，并且验证它是否出错；其他参数用于返回数据。

## async/await简介
>async/await是写异步代码的新方式，以前的方法有回调函数和Promise。
>async/await是基于Promise实现的，它不能用于普通的回调函数。
>async/await与Promise一样，是非阻塞的。
>async/await使得异步代码看起来像同步代码，这正是它的魔力所在。


