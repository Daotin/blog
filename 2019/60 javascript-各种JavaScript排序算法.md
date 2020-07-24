```js
// 冒泡排序
let arr = [1, 11, 8, 4, 65, 50];

let bubbleSort = function(list) {
    if (list.length <= 1) {
        return list;
    }
    for (let i = 0; i < list.length; i++) {
        for (let j = 1; j < list.length; j++) {
            let temp;
            if (list[j] > list[j + 1]) {
                temp = list[j];
                list[j] = list[j + 1];
                list[j + 1] = temp;
            }
        }
    }
    return list;
};
console.log('bubbleSort',bubbleSort(arr));

// 快速排序
function quickSort(list) {
    if (list.length <= 1) {
        return list;
    }
    let middleIndex = Math.floor(list.length / 2); //取中间下标
    let middle = list.splice(middleIndex, 1)[0]; //取该中间下标的数
    let leftList = [];
    let rightList = [];
    for (let i = 0; i < list.length; i++) {
        if (list[i] < middle) {
            leftList.push(list[i]);
        } else {
            rightList.push(list[i]);
        }
    }
    return quickSort(leftList).concat([middle], quickSort(rightList));
}
console.log('quickSort', quickSort(arr));

// 选择排序
function selectSort(arr){
    for(var i = 0; i < arr.length - 1; i++){
        var min = arr[i];
        for(var j = i + 1; j < arr.length - 1; j++){
            if(min > arr[j]){
                var temp = min;
                min = arr[j];
                arr[j] = temp;
            }
        }
        arr[i] = min;
    }
    return arr;
}

// 插入排序
function insertSort(arr) {
    for(var i = 1;i< arr.length;i++) {
        var key = arr[i];
        var j = i - 1;
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key;
    }
    return arr;
}

// 二分插入排序
function binaryInsertSort(arr){
    for (var i = 1; i < arr.length; i++) {
        var key = arr[i], left = 0, right = i - 1;
        while (left <= right) {
            var middle = parseInt((left + right) / 2);
            if (key < arr[middle]) {
                right = middle - 1;
            } else {
                left = middle + 1;
            }
        }
        for (var j = i - 1; j >= left; j--) {
            arr[j + 1] = arr[j];
        }
        arr[left] = key;
    }
    return arr;
}

// 希尔排序
function shallSort(array) {
    var increment = array.length;
    var i
    var temp; //暂存
    var count = 0;
    do {
        //设置增量
        increment = Math.floor(increment / 3) + 1;
        for (i = increment ; i < array.length; i++) {
            console.log(increment);
            if (array[i] < array[i - increment]) {
                temp = array[i];
                for (var j = i - increment; j >= 0 && temp < array[j]; j -= increment) {
                    array[j + increment] = array[j];
                }
                array[j + increment] = temp;
            }
        }
    }
    while (increment > 1)
    return array;
}
```

