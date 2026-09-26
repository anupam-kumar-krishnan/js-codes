
### _Reverse a String_

```js
const str="hello";
const revstr=str.split("").reverse().join("");
console.log(revstr);
```

### _Palindrome_

```js
const str="hello";
const revstr=str.split("").reverse().join("");

if(str===revstr){
  console.log("Palindrome");
}
else{
  console.log("Not Palindrome")
}
```

### _Reverse words in a Sentence_

```js
const str1="I love JavaScript";
const revLine=str1.split(" ").reverse().join(" ");
console.log(revLine);
```

### _Find the first non-repeating character in a string_

```js
const str="anupam";

let freq={};

for(let i=0;i<str.length;i++){
  if(freq[str[i]]){
    freq[str[i]]++;
  } else {
    freq[str[i]] = 1;
  }
}

for(let i=0;i<str.length;i++){
  if(freq[str[i]] === 1){
    console.log(str[i]);
    break;
  }
}
```

### _Count Vowel_

```js
const str="anupam";
const vw="aeiou";
let frequency=0;

for(let i=0;i<str.length;i++){
  if(vw.includes(str[i])){
    frequency++;
  }
}

console.log(frequency);
```

### _Character Frequency_

```js
const str="anupam";

const frequency={};

for (let i=0;i<str.length;i++){
  if(frequency[str[i]]===undefined){
    frequency[str[i]]=1;
  } else {
    frequency[str[i]]++;
  }
}

console.log(frequency);
```

### _Remove the duplicate_

```js
const str="anupam";
const result=[];

for(let i=0;i<str.length;i++){
  if(result.includes(str[i])){
    
  } else{
    result.push(str[i]);
  }
}

console.log(result);
```

### _Find Duplicate Elements_

```js
const arr=[2,5,3,2,8,5,9];
var temp=[];

for(let i=0;i<arr.length;i++){
  for(let j=0;j<i;j++){
    if(arr[i]==arr[j]){
      temp.push(arr[i]);
    }
  } 
}

console.log([...new Set(temp)]);
```

### _Missing Number_

```js
const arr=[1,2,3,4];
let count=1;
let found = false;

for(let i=0;i<arr.length;i++){
  if(arr[i] !== count){
    console.log(count);
    found = true;
    break;
  } 
  count++;
}


if(!found ){
  console.log(count);
}
```

###  _Intersection of two arrays_

```js
const arr1 = [1,2,3,4,5];
const arr2 = [3,4,5,6,7];
let result=[];

for(let i=0;i<arr1.length;i++){
  if(arr2.includes(arr1[i])){
    result.push(arr1[i]);
  }
}

console.log(result);
```


### _Move zeros to the end_

```js
const arr=[1,8,3,6,0,2,5,6,0,0,1];
let limit=arr.length;

for(let i=0;i<limit;i++){
  if(arr[i] === 0){
    arr.push(arr.splice(i, 1)[0]);
    i--;
    limit--;
  }
}

console.log(arr);
```

### _Two Sum_

```js
const arr=[5,4,11,15], target=9;

for(let i=0;i<arr.length;i++){
  for(let j=i+1;j<arr.length;j++){
    if((arr[i]+arr[j]) === target){
      console.log(arr[i],arr[j]);
    }
  }
}
```

### _Flatten an Array_

```js
const arr=[1, [2, 3], [4, [5, 6]]];

const result=[];

for(let i=0;i<arr.length;i++){
 if(Array.isArray(arr[i])){
   for(let j=0;j<arr[i].length;j++){
     if(Array.isArray(arr[i][j])){
      for(let k=0;k<arr[i][j].length;k++){
        result.push(arr[i][j][k]);
      }
     } else {
       result.push(arr[i][j]);
     }
   }
} else {
   result.push(arr[i]);
  }
}

console.log(result);
```
