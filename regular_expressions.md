### greedy vs non-greedy(lazy)
[參考:https://stackoverflow.com/questions/3075130/what-is-the-difference-between-and-regular-expressions]  
'.','+'在regex中預設都是greedy的，他們會先取到最多，再一個個返回找出符合規則最長的段落。  
加上'?'則會變為non-greedy，他們會先取最少，再一個個往後找到符合的最短段落。  
ex1.
```re
eeeAiiZuuuuAoooZeeee
```
```A.*Z``` 會找到 ```AiiZuuuuAoooZ```  
```A.*?Z``` 會找到 ```AiiZ```和```AoooZ```
##### 替代方案
negated character(否定字元)  
```A.*?Z```可以用```A[^Z]*Z```替換  
ex2.
```re
eeAiiZooAuuZZeeeZZfff
```
```A[^Z]*ZZ```找出```AuuZZ```
```A.*?ZZ```找出```AiiZooAuuZZ```
```A.*ZZ```找出```AiiZooAuuZZeeeZZ```
