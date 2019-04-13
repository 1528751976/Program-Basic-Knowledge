线性表
=====

### 顺序表与链表的区别：
    1. 基于空间的比较：
        * 存储分配方式：顺序表需要分配连续的地址空间，空间一旦确定，就不能再改变，一次性分配；链表是散落存储，可进行动态的分配
        * 存储密度：顺序表存储密度大于链表，因为链表可以用来存储前驱和后继指针
    2. 基于时间的比较：
        * 存取方式：顺序表可以实现随机存取；链表只能够顺序存取
        * 插入和删除：顺序表插入和删除平均需要移动一半元素；链表不需要移动元素，只需要移动指针
### 链表的分类：
    单链表、循环单链表、双向链表、循环双向链表、静态链表
    * 各个链表的判空方式：
        * 单链表：(带头节点的单链表)head->next == null;(不带头节点的单链表)head == null;
        * 循环单链表：(带头节点的单链表)head->next == head;(不带头节点的单链表)head == null;
        * 双向链表：(带头节点的单链表)head->next == null;(不带头节点的单链表)head == null;
        * 循环双向链表：(带头节点的单链表)head->next == head;或head->prior == head;或head->next==head&&head->prior==head;或head->next==head||head->prior==head(不带头节点的单链表)head == null;
### 顺序表的操作：(其本质就是一个数组)

```c

    //顺序表结构体的定义
    #define MaxSize 100 //设置地址空间的大小
    typedef struct{ //定义结构体，相当于Java中的class标记符，{}里面有成员变量
        int data[MaxSize]; //用一个数组来分配连续的地址空间，空间为100，地址为0-99
        int length; //用来表示当前空间中的元素个数
    }Sqlist; //这个就相当于是Java中的类名称
    //顺序表初始化：
    void initList(Sqlist &L){ //这里的L就相当于Java中类的对象，因为要将L进行初始化，表示L自身将要发生改变，那么给他加上&符号，凡是会发生改变的就加&
        L.length = 0; //让表的初始长度等于0，表示当前表中的元素个数为0个
    }

    //顺序表插入元素：
    int InsertElement(Sqlist &L , int p , int e){ //这里需要向L中插入元素，因此L会发生改变，p代表要插入的位置，e为要插入的元素。函数返回值类型为int类型，因此成功返回1，失败返回0
        if(p<0 || p>MaxSize || L.length == MaxSize){ //这里先判断p的值是否小于0，是否大于最大值，当前元素个数是否已经达到上限
            return 0;
        } //如果条件成立，则将元素从后往前，依次往后移动一位
        for(int i = L.length-1 ; i>=p ; i++){
            L.data[i+1] = L.data[i];
        }
        L.data[p] = e;  //找到p位置，将e插入进去
        ++(L.length);    //插入元素后，将元素个数加1
        return 1;   //插入成功，返回1
    }

    //顺序表删除元素：
    int deleteElement(Sqlist &L , int p ,int &e){ // 这个e不能少，这不是我规定的，用来接收要删除元素的值
        if(p<0 || p>MaxSize || L.length == MaxSize){ //这里先判断p的值是否小于0，是否大于最大值，当前元素个数是否已经达到上限
            return 0;
        } //如果条件成立，则将元素从p后面一位开始依次往前覆盖一位
        e = L.data[p];
        for(int i = p ; i < L.length; i++){
            L.data[i] = L.data[i+1];
        }
        --(L.length); //删除后减1
        return 1; // 删除成功，返回1
    }

    //查找指定元素位置：
    int findElement(Sqlist L , int e){
        for(int i = 0; i<L.length; i++){
            if(e == L.data[i]){
                return i;
            }
        }
        return -1;  //未找到指定数字
    }

    //查找指定位置上的元素：
    int findEle(Sqlist L,int p){
        for(int i = 0 ; i < L.length ; i++){
            if(i == p){
                return L.data[i];
            }
        }
    }

    //在一个升序序列中，插入一个数，使得整个序列依然有序，需要拆分成两个部分，第一步，需要将这个位置找出来，第二部，将这个数插入到这个位置
    int findEle(Sqlist &L , int x){
        for(int i = 0 ; i < L.length ; i ++ ){
            if(x < L.data[i]){
                return i;
            }
        }
        return i;   //没有这个位置，那么就肯定是在表尾了，插入到表尾
    }

    void insertEle(Sqlist &L , int e){
        int p = findEle(L,x);   //先通过find函数将这个位置找出来
        for(int i = L.length-1 ; i>= p ; i-- ){  //通过遍历，将p位置及之后的元素依次向后移动一位
            L.data[i+1] = L.data[i];  
        }
        L.data[p] = e ;  //将e插入
        ++(L.length);
        return 1;       //成功返回1
    }


```

### 顺序表的方面的练习题：
1. 将两个表A和B，合并到A中。思路：扩大线性表A的元素长度，将B中不存在A表中的元素加入到A中，需要将B中的元素逐个取出，然后遍历A表，查看是否有相同的元素。

```c

void Union(Sqlist &A , Sqlist B){
    

}

```
