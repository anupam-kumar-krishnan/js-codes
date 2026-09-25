
### Reverse a String

```js
const str="hello";
const revstr=str.split("").reverse().join("");
console.log(revstr);
```

### Palindrome

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

### Reverse words in a Sentence

```js
const str1="I love JavaScript";
const revLine=str1.split(" ").reverse().join(" ");
console.log(revLine);
```

### Find the first non-repeating character in a string

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
