---
title: JS常用小功能
date: 2019-02-15 14:02:59
tags: js
---
##  <left>目录</left>
 1. ###### [数组去重](#20190215001)
 2. ###### [字符串反转](#20190215002)
 3. ###### [数组去扁平化并去重排序](#20190215003)
##  <left>正文</left>
- <h6 id="20190215001">数组去重(es6)</h2>

	```javascript
	const arr = ['🙂', 7, '🙂', '😡'];
	// 1.Set
	[...new Set(arr)];
	// 或者
	Array.from(new Set(arr));
	
	// 2.Filter
	arr.filter((item, index) => arr.indexOf(item) === index);
	
	// 3.Reduce
	/*  reducer 函数接收4个参数:
		1.Accumulator (acc) (累计器)
		2.Current Value (cur) (当前值)
		3.Current Index (idx) (当前索引)
		4.Source Array (src) (源数组)
	*/
	arr.reduce((acc, cur) => 
		return acc.includes(cur) ? acc : [...acc, cur], []);
		
	// 4.Object 键值对
	array.filter(function(item, index, array){
        return obj.hasOwnProperty(typeof item + item) ? false : (obj[typeof item + item] = true)
    })
    
  // 5.Map
  const seen = new Map()
  arr.filter((a) => !seen.has(a) && seen.set(a, 1))
    ```
 - <h6 id="20190215002">字符串反转</h2>
 
	```javascript
	const str = 'abcdef123';
	console.log(str.split('').reverse().join(''));
	```
 - <h6 id="20190215003">数组去扁平化并去重排序</h2>
 
	```javascript
	var arr = [ [1, 2, 2], [3, 4, 5, 5], [6, 7, 8, 9, [11, 12, [12, 13, [14] ] ] ], 10];
	Array.from(new Set(arr.flat(Infinity))).sort((a,b)=>{ return a-b})
	```


	

