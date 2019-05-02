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
        * 单链表：(带头节点的)head->next == null;(不带头节点的)head == null;
        * 循环单链表：(带头节点的)head->next == head;(不带头节点的)head == null;
        * 双向链表：(带头节点的)head->next == null;(不带头节点的)head == null;
        * 循环双向链表：(带头节点的)head->next == head;或head->prior == head;或head->next==head&&head->prior==head;或head->next==head||head->prior==head(不带头节点的)head == null;

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

1. 将一个顺序表中的元素进行逆置。

```c

void reserve(Sqlist &L){
    int i , j;
    int temp;
    for(i = 0 , j = L.length-1; i<j ; i++ , j--){
        temp = L.data[i];
        L.data[i] = L.data[j];
        L.data[j] = temp;
    }
}

```

2. 将两个表A和B，合并到A中。思路：扩大线性表A的元素长度，将B中不存在A表中的元素加入到A中，需要将B中的元素逐个取出，然后遍历A表，查看是否有相同的元素。

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

3. 两个线性表LA和LB各自均递增排列，先将LA和LB合并成LC，依旧要LC中的元素递增。

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

4. 顺序表用数组A[]表示，表中元素存储在数组下标0-m+n-1的范围内，前m个元素递增有序，后n个元素也递增有序，设计一个算法，使得整个顺序表有序。

```c
    /*算法思想：A[]看成是两个顺序表L和R，L表中有m个元素，下标范围是0-m-1，而R表中有n个元素，下标范围为m-m+n-1。可以将R表中的元素取出，插入到L中合适的位置。
    插入过程：从R中取出第一个元素A[m]存入到temp中，然后依次与L表中的元素从后往前比较，即逐个与A[m-1],A[m-2]...A[0]进行比较，当temp<A[j]时，将A[j]后移一位，否则将temp存入到A[j+1]，重复上述过程继续插入A[m+1]，A[m+2]...A[m+n]，最终A[]中元素整体有序。
    */
    void insertElem(int A[] , int m , int n){
        int i , j , temp;
        for(i = m ; i<m+n-1 ; m++){
            temp = A[i];
            for(j = m-1 ; j>=0 && temp<A[j] ; j--){
                A[j+1] = A[j] ; //元素后移，腾出位置插入temp
            }
            A[j+1] = temp; //插入temp,由于for循环后j多前移了一位，因此在j+1处插入
        }
    }
```

5. 已知递增有序的单链表A、B（A，B中元素个数分别是m、n，且A、B都带有头结点）分别存储了一个集合，请设计算法，求出两个集合A和B的差集A-B(由仅在A中出现，而不在B中出现的元素)

```c
    /*算法思想：只需要从A中删除A与B中共有的元素，定一两个指针，分别指向A和B的起始节点，然后比较p、q所指向节点元素的大小，如果p指向的元素大小小于q，则让p向后移动一位，否则，让q向后移动一位。当p、q指针所指向的数据相等时，删除A中此时对的节点。当两个p、q中任意一个为NULL时，算法结束。
    */
    void difference(LinkList &A ,LinkList &B){
        LNode *p = A->next;
        LNode *q = B->next;
        LNode *pre = A ; //per为A链表中p的前驱节点
        LNode *r;
        while(p!=NULL&&q!=NULL){
            if(p->data < q->data){
                pre = p;
                p = p->next;
            }else if(p->data > q->data){
                q = q->next;
            }else{
                pre ->next = p ->next;
                r = p;
                p = p ->next;
                free(r);
            }
        }
    }
```

6. 已知递增有序的单链表A、B（A，B中元素个数分别是m、n，且A、B都带有头结点）分别存储了一个集合，请设计算法，求出两个集合A和B的差集A-B(由仅在A中出现，而不在B中出现的元素)

```c
    /*算法思想：只需要从A中删除A与B中共有的元素，定一两个指针，分别指向A和B的起始节点，然后比较p、q所指向节点元素的大小，如果p指向的元素大小小于q，则让p向后移动一位，否则，让q向后移动一位。当p、q指针所指向的数据相等时，删除A中此时对的节点。当两个p、q中任意一个为NULL时，算法结束。
    */
    void difference(LinkList &A ,LinkList &B){
        LNode *p = A->next;
        LNode *q = B->next;
        LNode *pre = A ; //per为A链表中p的前驱节点
        LNode *r;
        while(p!=NULL&&q!=NULL){
            if(p->data < q->data){
                pre = p;
                p = p->next;
            }else if(p->data > q->data){
                q = q->next;
            }else{
                pre ->next = p ->next;
                r = p;
                p = p ->next;
                free(r);
            }
        }
    }
```

7. 设计一个算法，在给定的顺序表中删除i-j范围的元素。

```c

void DeleteElement(Sqlist &L , int i , int j){
    int k , delta;
    delta = j-i+1;
    for(k = j+1 ; k<L.length ; k++){
        L.data[k-delta] = L.data[k];
    }
    L.length -= delta;
}

```

8. 有一个顺序表L，去元素为整型数据，设计一个算法，将L中所有小于表头元素的整数放在前半部分，大于表头的部分放在后半部分

```c

void move(Sqlist &L){
    int temp ;
    int i = 0 , j = L.length-1;
    temp = L.data[i];
    while(i < j){
        while(i < j && L.data[j]>temp){  
        //j从右往左扫描，当来到第一个比temp小的元素停止，并且每走一步都要判断i是否小于j
            j--;
        }
        //检测是否i小于j
        if(i < j){
            L.data[i] = L.data[j]; //移动元素
            i++;   //i向右移动一位
        }
        while(i < j && L.data[i] < temp){
            i++;
        }
        if(i < j){
            L.data[j] = L.data[i];
            j--;
        }
    }
    L.data[i] = temp;  //表首元素放在最终位置
}

```

### 其他重要知识点
1. 为什么在链表设置尾指针要比设置头指针更好？
答： 因为，设置尾指针方便找到尾结点和头结点，且时间复杂度为O(1),而设置头指针，不利于尾结点的查找，头结点查找的时间复杂度为O(1),而尾结点的查找时间复杂度为O(n)。


### 单链表的操作：

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
        LNode *p = L;
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
        LNode *p = L;
        LNode *q;
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

    //将两个有序链表LA,LB合并成一个有序链表LC，使用尾插法归并成非递减
    void MergeLinkList(LinkList &LA,LinkList &LB,LinkList &Lc){
        //已知单链表LA和LB的元素按值非递减排列，归并LA和LB得到新的单链表LC，LC的元素也按照非递减排列
        LNode *pa = LA->next;
        LNode *pb = LB->next;
        LC = LNode *pc = LA;  //用LA的头结点充当LC的头结点
        pc->next = NULL;
        while(pa && pb){ //LA和LB不为空的时候
            if(pa->data <= pb->data){
                pc->next = pa;  //将pa当前指向的结点添加到LC
                pc = pa;        //将pc指针移动到新添加的结点
                pa = pa->next;  //将指针pa向后挪一位
            }else{
                pc->next = pb;
                pc = pb;
                pb = pb->next;
            }
        }
        pc->next = pa ? pa : pb; //三目运算来将剩余的元素添加
        free(LB);       //释放LB的头结点
    }//MergeLinkList

    //将两个有序链表LA,LB合并成一个有序链表LC，使用头插法归并成递减
    void MergeLinkList(LinkList &LA,LinkList &LB,LinkList &Lc){
        //已知单链表LA和LB的元素按值非递减排列，归并LA和LB得到新的单链表LC，LC的元素按照递减排列
        LNode *pa = LA->next;
        LNode *pb = LB->next;
        LNode *pc;
        LC = LA;
        LC->next = NULL;
        while(pa && pb){ //LA和LB不为空的时候
            if(pa->data <= pb->data){
                pc = pa;
                pa = pa->next;
                pc->next = LC->next;  
                LC->next = pc;
            }else{
                pc = pb;
                pb = pb->next;
                pc->next = LC->next;  
                LC->next = pc;
            }
        }
        //下面的两个循环是和求递增归并序列不同的地方，必须将剩余元素逐个插入到C的头部才能得到最终的递减序列
        while(pa){
            pc = pa;
            pa = pa->next;
            pc->next = LC->next;  
            LC->next = pc;
        }
        while(pb){
            pc = pb;
            pb = pb->next;
            pc->next = LC->next;  
            LC->next = pc;
        }
        free(LB);       //释放LB的头结点
    }//MergeLinkList

    //查找链表C中是否存在一个值为x的结点，若存在，则删除该节点并返回1，否则返回0
    int findx(LinkList &C,int x){
        LNode *p , *q;
        p = C;
        //开始查找
        while(p->next != NULL){
            if(p->next->data == x){
                break;  //跳出整个while循环
            }
            p = p->next;
        }
        if(p->next == NULL){
            return 0;
        }
        else{
            q = p->next;
            p->next = p->next->next;
            free(q);
            return 1;
        }
    }

```

### 双链表的操作：

```c
    //采用尾插法建立双链表La，将数组a中的元素插入到双链表中
    void CreateDLinkList(LinkList &La ,int a[] ,int n){
        DLNode *p , *q;
        int i ;
        La = (LNode*)malloc(sizeof(DLNode));
        La->prior = NULL;
        La->next = NULL;
        p = La;
        while(i = 0 ; i<n ; i++){
            q = (DLNode)malloc(sizeof(DLNode));
            q->data = a[i];
            p->next =q;
            q->prior = p;
            p = q;
        }
        p->next = NULL;
    }

    //在双链表中查找第一个值为x的结点，从第一个结点开始，边扫描，边比较，若找到这样的结点，返回结点指针，否则返回NULL
    DLNode* findNode(LinkList C,int x){
        DLNode *p = C->next;
        while(p){
            if(p->data == x){
                break;
            }
            p = p->next;
        }
        return p;
    }
```


### 链表的习题：

1. 有一个递增非空单链表，设计一个算法删除值域重复的结点。例如：{1,1,2,3,3,3,4,4,7,7,7,9,9,9}经过删除后变成{1,2,3,4,7,9}
```c

//算法思想：依次将原序列中每个连续相等子序列的第一个元素移动到表的前端，将剩余的元素删除即可。令p指向起始节点。q从p的后继结点开始扫描，q每来到一个新结点的时候进行检测：当q->data等于p->data时，什么也不做，q继续往后走；当两者不相等时，p往后走一个位置，然后用q->data取代p->data。之后，q继续往后扫描，重复以上过程。当q为空时，释放从p之后的所有结点空间。
void dels(LinkList &L){
    LNode *p = L->next;
    LNode *q = L->next->next;
    LNode *r;
    while(q!=NULL){
        while(q!=NULL && q->data==p->data){
            q = q->next;
        }
        if(q!=NULL){
            p = p->next;
            p->data = q->data;
        }
    }
    q = p->next;
    p->next = NULL;
    while(q!=NULL){
        r = q;
        q = q->next;
        free(r);
    }
}

```

2. 将一个带头结点的单链表逆置，且不能建立新表，只能在原来的基础上改动

```c

//算法思想：将L->next设置为空，然后将头结点后的元素使用头插法插入L中，这样L中的元素正好逆序。

void reversel(LinkList &L){
    LNode *p = L->next , *q;
    L-next = NULL;
    while(p != NULL){
        q = p ->next;
        p->next = L->next;
        L->next = p ;
        p = q;
    }
}


```

3. 设计一个算法，将单链表中最小的元素删除掉
```c

void delmin(LinkList &L){
    LNode *pre = L ,*p = pre->next,*minp = p , *minpre = pre;
    while(p!= NULL){
        if(p->data<minp->data){
            minp = p ;
            minpre = pre;
        }
        pre = p;
        p = p ->next;
    }
    minpre->next = minp->next;
    free(minp);
}

```

4. 设计一个算法，将单链表A拆分成两个链表分别是A和B，A中存放原表数据域为奇数的结点，B中存放原表中数据域为偶数的结点，并保持原来链表的相对顺序。
```c
//算法思想：用指针p从头至尾扫描A链表，当发现结点data域为偶数结点则取下，插入链表B。
void split(LinkList &L){
    LNode *p,*q,*r;
    B = (LNode*)malloc(sizeof(LNode));
    B->next = NULL;

    r = B;
    p = A;
    while(p->next !=NULL){
        if((p->next->data & 1) == 0){
            q = p->next;
            p->next = q->next;
            q->next = NULL;
            r->next = q;
            r = q;
        }else{
            p = p->next;
        }
    }
}


```
