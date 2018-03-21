#### 冒 泡 排 序

```
  def maopao():
    List=[4,9,10,8,11,3,2]
    for i in range(0,len(List)-1):
        j=0
        while j<len(List)-1:
            if List[j] > List[j+1]:
                List[j+1],List[j]=List[j],List[j+1]
		print List
            j=j+1
    print List
```



