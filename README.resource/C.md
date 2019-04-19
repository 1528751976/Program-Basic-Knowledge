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

    void insertElem(Sqlist &L , int e){
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

void Union(Sqlist &LA , Sqlist LB){
    for(int i = 0 ; i < LB.length ; i++){
        GetElem(LB,i,e);  //取LB中的第i个元素
        if(!LocateElem(LA,e,equal)){   //查看LA中是否存在LB中的e元素
            ListInsert(LA,++LA.length,e);   // 不存在则将LA的表长+1，然后将e插入到表尾
        }
    }
}

```

2. 两个线性表LA和LB各自均递增排列，先将LA和LB合并成LC，依旧要LC中的元素递增。

```c

void MergeList(Sqlist LA ,Sqlist LB , Sqlist &LC){
    InitList(LC);  //初始化链表C
    int i = 0 , j = 0 , k = 0 ;  //定义两个指标i和j，i用来遍历LA，j用来遍历LB，k指示LC的长度
    while((i <= LA.length) && (j <= LB.length)){ //LA和LB均非空的时候
        GetElem(LA,i,e1); //取出A中第i个元素
        GetElem(LB,j,e2); //取出B中第j个元素
        if(e1 <= e2){  //若e1小于e2
            ListInsert(LC,++k,e1);//将LC的表长加1，然后将e1插入到表尾
            ++i;  // 将i值加1，进行下次比较
        }else{  //若e1大于e2
            ListInsert(LC,++k,e2);  //将LC的表长加1，然后将e2插入到表尾
            ++j; // 将i值加1，进行下次比较
        }
    }

    //如果只有LA表时。将A表直接加入到C表中
    while(i<= LA.Length){
        GetElem(LA,i++,e1); //i++代表先进行运算，再自增
        ListInsert(LC,++k,e1);//将e1添加到表C中
    }

    //如果只有LB表时，将B表直接加入到C表中
    while(i<= LB.length){
        GetElem(LB,i++,e2); //i++代表先进行运算，再自增
        ListInsert(LC,++k,e2);//将e2添加到表C中
    }

}
```

### 链表的操作：

```c
    //单链表的定义
    typedef struct LNode{
        int data; //数据域,int类型可按照实际更改 
        struct LNode *next; //定义指针域
    }LNode,*LinkList;  //把LNode看成Java的类名，LNode中有两个属性，一个是data，一个是指针型的next，而LinkList看成是一个对象

    //双链表的定义
    typedef struct DLNode{
        int data;//数据域,int类型可按照实际更改 
        struct DLNode *prior;   //定义前驱指针域
        struct DLNode *next;    //定义后继指针域
    }DLNode,*DLinkList; //把DLNode看成Java的类名，DLNode中有三个属性，一个data，一个是前驱指针，一个是后继指针而DLinkList看成是一个对象

    //单链表的插入算法
    void LinkListInsert(LinkList &L , int i , int e){ //在带头节点的第i个位置插入e
        p = L;
        int j = 0;
        if(!p || j>i ){
            return Erro;
        }
        while(p&&j<i-1){ //寻找i-1个结点
            p = p-> next;
            ++j;
        }
        s = (LinkList)malloc(sizeof(LNode));//生成新的结点
        s->data = e;
        s->next = p->next;
        p->next = s;
    }   //LinkListInsert

    //单链表删除元素
    void LinkListDelete(LinkList &L , int i,int &e){ //删除第i个位置上的元素，并将元素值赋给e
        p = L;
        int j = 0;
        if(!p || j>i){
            return Erro;
        }
        while(p->next && j<i-1){  //寻找第i个位置
            p = p->next;
            ++j;
        }
        q = p->next;
        p->next = q->next;
        e = q->data;
        free(q);
    } // LinkListDelete

    //逆向建立单链表，即头插法
    void CreateLinkList(LinkList &L , int n){//输入n个值，将其逆序放入单链表
        L = (LinkList)malloc(sizeof(LNode)); //创建结点
        L->next = NULL; //先令带头结点的单链表为空
        for(int i = n ; i >=0 ; i++){
            p = (LinkList)malloc(sizeof(LNode)); //生成新结点
            scanf(&p->data);//输入值
            p->next = L->next;//插入到表头
            L->next = p;
        }
    } //CreateLinkList
```
