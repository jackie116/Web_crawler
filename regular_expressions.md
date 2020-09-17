### greedy vs non-greedy(lazy)
'.','+'在regex中預設都是greedy的，他們會先取到最多，再一個個返回找出符合規則最長的段落。  
加上'?'則會變為non-greedy，他們會先取最少，再一個個往後找到符合的最短段落。  
ex.  
```re
eeeAiiZuuuuAoooZeeee
```
```re A.*Z``` 會找到 ```re AiiZuuuuAoooZ```  
```re A.*?Z``` 會找到 ```re AiiZ```和```re AoooZ```
