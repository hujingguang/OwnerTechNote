#### 冒 泡 排 序

```
  def bulle_sort():
    List=[4,9,10,8,11,3,2]
    for i in range(1,len(List)):
        while j<len(List)-1:
            if List[j] > List[j+1]:
                List[j+1],List[j]=List[j],List[j+1]
            print List
            j=j+1
        j=0
    print List
```



