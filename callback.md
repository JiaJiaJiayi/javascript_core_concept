# CALLBACK
### Callback-based of synchronous programming
#### Error-first callback
```javascript
function A(callback){
    try{
    const data = await AnotherFunction();
    callback(data)}
    catch{
        callback(new Error("Something wrong"))
     }
}
A(function(error, data){
    if(error){
        // Handle error
    }
    else{
        // Handle data
    }
})
```

### Solve callback hell
Key: split several steps
```javascript
function A(callback){
    try{
    const data = await AnotherFunction();
    callback(data)}
    catch{
        callback(new Error("Something wrong"))
     }
}
step1(function(error, data){
    if(error){
        // Handle error
    }
    else{
        step2()
    }
})
step2(function(error, data){
    if(error){
        // Handle error
    }
    else{
        step3()
    }
})

A(step1);
```
