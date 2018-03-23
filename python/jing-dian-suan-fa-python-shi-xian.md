#### 冒 泡 排 序

说明:

![](/assets/849589-20171015223238449-2146169197.gif)

##### 代码实现

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

---

#### 选 择 排 序

动态图说明:

![](/assets/xz.gif)

###### 代码实现

```
  def f():
     List=[4,10,2,11,3,10,1,19,3]
     for i in range(0,len(List)-1):
        min_index=in=n+1
        for j in range(i+1,len(List)):
           if List[j]<List[min_index]:
           min_index=j
        List[i],List[min_index]=List[min_index],List[i]
     print List
```

---------------------------------------

