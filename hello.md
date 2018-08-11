<<<<<<< HEAD
# 与剑指offer相遇的八月31天

day 1  
- 二进制中1的个数  
常规解法  
1、将1 每次 <<1 与 x 进行&操作 记录结果为1的次数   
2、怎么确定 <<1 结束条件 <<1的数值大于x  
科学解法  
1、原数减去1之后最右边的1都会变成0 ， 用减1后的数和原数进行&操作  
```
    class Solution {
    public:
        int  NumberOf1(int n) {
            //常规解法
            int count = 0;
            int unsigned flag = 1;
            while(flag){
                
                if(n & flag)
                    count ++;
                flag = flag << 1;
            }
            
            //科学解法
            int count = 0;
            while(n){
                
                count ++;
                n = n & (n-1);
            }

            return count;
        }
    };
```
    相关题目：  
    题目1  
    用一条语句判断一个整数是不是 `2` 的整数次方。  
    思路：  
    一个整数如果是 `2` 的整数次方的话，它的二进制表示只有一位是 `1`  
    把这个整数减 `1` 后和原数 `&`  

    题目2  
    输入两个整数 `m` 和 `n`，计算需要改变 `m` 的二进制表示中的多少位才能得到 `n`  
    思路：  
    两数进行异或运算 `^` 不同位被置 `1`  
    把 *此数* 和 *此数减 `1`*  后的值进行与运算



- 试题14  
剪绳子  

动态规划  
1、求一个问题的最优解  
2、整体问题的最优解是依赖各个子问题的最优解  
3、小问题之间还有相互重叠的更小的子问题  
4、从上往下分析问题、从下往上求解问题  

动态规划思路  
1、从
```  
    f(1)=0,  
    f(2)=1,  
    f(3)=2 * 1 = 2,  
    f(4)=2 * 2 = 4,  
    f(5)=2 * 3 = 6,  
    f(6)=3 * 3 = 9,  
    f(7)=3 * 2 * 2 = 12,  
    f(8)=3 * 3 * 2 = 18,   
```  
2、  
```
int maxProductAfterCutting_solution1(int length){

    if(length < 2)
    return 0;
    if(length == 2)
    return 1;
    if(length == 3)
    return 2;

    int* len = new int[length+1];
    len[0]=0;
    len[1]=1;
    len[2]=2;
    len[3]=3;

    int max = 0;
    for(int i = 4; i <= length; i++){
        
        for(int j = 1; j <= i/2; j++){

            len[i] = len[j] * len[i-j];
            if(max < len[i])
                max = len[i];
        }
        len[i] = max;
    }

    max = len[length];
    delete[] len;

    return max;
}
```

贪心算法  
应得到尽可能多的3  
当余数为1时，退3，进4

```
int maxProductAfterCutting_solution1(int length){

    if(length < 2)
    return 0;
    if(length == 2)
    return 1;
    if(length == 3)
    return 2;

    int timesOf3 = length / 3;
    if(length - 3 * timesOf3 == 1)
        timesOf3 --;

    int timesOf2 = (length - 3 * timesOf3) / 2;

    return pow(2,timesOf3) * pow(2,timesOf2);
}
```
- 12 
    
    **回溯法**  
    回溯法非常适合由多个步骤组成的问题，并且每个步骤都有多个选项  
    用回溯法解决的问题的所有选项可以形象地用 *树状结构* 表示  
    在某一步有 *n* 个可能的选项，那么该步骤就可以看成是树状结构中的一个节点，每个选项看成树中节点的连接线，经过这些连接线到达该节点的 *n* 个子节点。树的叶节点对应着终结状态。如果在叶节点的状态满足题目的约束条件，那么我们找到了一个可行的解决方案。  
    如果在叶节点的状态不满足约束条件，那么只好回溯到它的上一个节点再尝试其他的选项。如果上一个节点所有可能的选项都已经试过，并不能达到满足约束条件的终结状态，则再次回溯到上一个节点。如果所有节点的所有选项都已经试过仍然不能达到满足约束条件的终结状态，则该问题无解。  

    适合 递归实现

矩阵中的路径  
1、实现上下左右移动  
2、路径标识   

```
class Solution {
public:
    bool hasPath(char* matrix, int rows, int cols, char* str)
    {
        if(matrix == NULL || rows < 1 || cols < 1 || str == NULL)
            return false;
        
        bool* visited = new bool[rows * cols];
        memset(visited,0,rows*cols);
        
        int pathlength = 0;
        
        for(int row = 0; row < rows; row ++){
            for(int col = 0; col < cols; col ++){
                if(hasPathCore(matrix,rows,cols,str,row,col,pathlength,visited))
                    return true;
            }
        }
        
        delete[] visited;
        return false;
    }

    bool hasPathCore(const char* matrix, int rows, int cols, const char* str, int row, int col, int& pathlength, bool* visited){
        
        if(str[pathlength] == '\0')
            return true;
        
        bool hasPath = false;
        //if(matrix == null || row < 0 || col < 0)
            //return false;
        if(row >= 0 && row < rows && col >= 0 && col < cols
           && matrix[cols * row + col] == str[pathlength]
           && !visited[cols * row + col]){
            
            pathlength ++;
            visited[cols * row + col] = true;
            
            hasPath = hasPathCore(matrix, rows, cols, str, row-1, col, pathlength, visited)
                    ||hasPathCore(matrix, rows, cols, str, row+1, col, pathlength, visited)
                    ||hasPathCore(matrix, rows, cols, str, row, col-1, pathlength, visited)
                    ||hasPathCore(matrix, rows, cols, str, row, col+1, pathlength, visited);
            
            if(!hasPath){
                pathlength --;
                visited[cols * row + col] = false;
            }
        }
        
        return hasPath;
    }
};
```
13 机器人的运动范围  
不能进入行坐标和列坐标的位数之和大于k的格子  
```
class Solution {
public:
    int movingCount(int threshold, int rows, int cols)
    {
        if(threshold < 1 || rows < 0 || cols < 0)
            return 0;
        
        bool* visited = new bool[rows * cols];
        memset(visited,0,rows*cols);
        
        int count = movingCountCore(threshold, rows, cols, 0, 0, visited);

        delete[] visited;
        return count;
    }
    
    int movingCountCore(int threshold, int rows, int cols, int row, int col, bool* visited){
        
        int count = 0;
        if(row >= 0 && row < rows && col >= 0 && col <= cols
           && checkSumOfBit(threshold, row, col)
           && !visited[row*cols+col]){
            
            visited[row*cols+col] = true;

            count = 1 +   movingCountCore(threshold, rows, cols, row - 1, col, visited)
                        + movingCountCore(threshold, rows, cols, row + 1, col, visited)
                        + movingCountCore(threshold, rows, cols, row, col - 1, visited)
                        + movingCountCore(threshold, rows, cols, row, col + 1, visited);
        }
          return count;
    }
    
    bool checkSumOfBit(int threshold, int row, int col){
        int sum = sumOfRowsAndCols(row) + sumOfRowsAndCols(col);
        return sum <= threshold;
    }
    
    int sumOfRowsAndCols(int row){
        /*
        if(row < 10)
            return row;
        
        int sum = 0;
        int ten = 10;
        while(row/ten || row%ten){

            sum += row%ten;
            row /= ten;
        }*/
        int sum = 0;
        while(row){ 
            
            sum += row%10;
            row /=10;
        }
        return sum;
    }
};
```

day3  
查找和排序  
查找：  
- 顺序查找  
```
int lineSearch(int arr[], int v, int n){
	for(int i = 0; i < n; i ++)
		if(arr[i] = v)
		return i;
	return -1;
}
```
- 二分查找  
```
//循环
int binarySearch(int arr[], int v, int n){
	int l = 0;
	int r = n-1;
	
	while(l < r){
		int mid = (l + r) / 2;
		if(arr[mid] == v)
			return mid;
		else if(arr[mid] < v)
			r = mid - 1;
		else
			l = mid + 1;
	}
	return -1;
}
```

```
//递归
int binarySearch(int arr[], int v, int l, int r){
	if(l > r)
	return -1;
	
	int mid = (l + r) / 2;
	if(arr[mid] == v)
	return mid;
	
	if(arr[mid] < v)
	binarySearch(arr,v,mid+1,r);
	else if(arr[mid] > v)
	binarySearch(arr,v,l,mid-1);
}
```
在排序数组中查找一个数字  
或者统计某个数字出现的次数  
11、旋转数组的最小数字  
```
int Min(int arr[], int n){
	if(arr == NULL || n <= 0)
		return -1;
	
	int l = 0;
	int r = n-1;
	int mid = l; //防止{1，2，3，4，5} 旋转0个数字的数组
	while(arr[l] >= arr[r]){
		if(r - l == 1){
			return r;
			break;
		}

		int mid = (l + r) / 2;
		if(arr[l] >= arr[mid])
			l = mid; //而不是l = mid + 1;
		else if(arr[mid] <= arr[r])
			r = mid; //r = mid - 1;
		
	}
}
```
53、在排序数组中查找数字 
```

```

- 哈希表查找  
优点：能在 *O*(1)时间内查找某一元素，是效率最高的查找方法  
缺点：需要额外空间来实现哈希表  
50、第一个只出现一次的字符  
- 二叉树查找  
 33、二叉搜索树的后序遍历序列  
 36、二叉搜索树与双向链表  

排序：  (额外空间消耗、平均时间复杂度、最差时间复杂度)
插入排序  
```
void insertionSort(int arr[], int n){
    for(int i = 1; i < n; i ++)
        for(int j = i; j > 0; j --)
            if(arr[j]<arr[j-1])
                swap(arr[j],arr[j-1]);
            else
                break;
        
}

void insertionSort(int arr[], int n){
    for(int i = 1; i < n; i ++){
        int e = arr[i];
        int j;
        for(j = i; j > 0; j --){
            if(e < arr[j-1])
                arr[j] = arr[j-1];
            else
                arr[j] = e;
        }
    }
}
```
冒泡排序  
```
void buttleSort(int arr[], int n){
    for(int i = 1; i < n; i ++){
        for(int j = i; j < n - i + 1; j ++){

            if(arr[j] < arr[j-1])
                swap(arr[j],arr[j-1]);          
        }
        
    }
}
```
优化:  
```
template<typename T>
void bubbleSort( T arr[] , int n){

    bool swapped;
    do{
        swapped = false;

        for( int i = 1 ; i < n ; i ++ )
            if( arr[i-1] > arr[i] ){
                swap( arr[i-1] , arr[i] );
                swapped = true;

                // 可以记录最后一次的交换位置,在此之后的元素在下一轮扫描中均不考虑
            }
        // 优化,每一趟Bubble Sort都将最大的元素放在了最后的位置
        // 所以下一次排序,最后的元素可以不再考虑
        // 实测,使用这种简单优化,时间性能更好
        n --;

    }while(swapped);
}
```

希尔排序   
```
void shellSort(int arr[], int n){
	int h = 0;
	while(h < n/3)
		h = 3 * h + 1;
	
	while(h >= 1){
		for(int i = h; i < n, i++){
			int e = arr[i];
			int j;
			for(int j = i; j >= h && arr[j] < arr[h-j]; j -= h)
				arr[j] = arr[j-h];
			
			arr[j] = e;
		}
		
		h /= 3;
	}
	
}
```
归并排序  
递归:  
(自底向上)  
```
void mergebottomup(int arr[],int n){
    for(int sz = 1; sz < n; sz += sz){
        for(int i = 0; i < n; i = sz + sz){
            __merge(arr,i,i+sz-1,min(i+sz+sz-1,n-1));
        }
    }
}
```
(自顶向下)  
```
void mergeSort(int arr[], int n){
    __mergeSort(arr, 0, n-1);
}

void __mergeSort(int arr[], int l, int r){
    if(l > r)
        return;

    int mid = (l + r) / 2;
    __mergeSort(arr,l, i);
    __mergeSort(arr,i+1,r);
    __merge(arr,l,mid,r);
}

void __merge(int arr[], int l, int mid, int r){
    int aux[r-l+1];
    for(int i = 0; i < r-l+1; i ++){
        aux[i] = arr[l + i];
    }
    int i = 0;
    int j = mid-l+1;
    for(;l<=r;l++){
        if(l > mid){
            arr[l] = aux[j];
            j++;
        }else if(j > r){
            arr[l] = aux[i];
            i++;
        }else if(aux[i] < aux[j]){
            arr[l] = aux[i];
            i++;
        }else{
            arr[l] = aux[j];
            j++;
        }
    }
}
```
循环:  
```

```
快速排序   
在长度为n的数组中查找第k大的数字  
```
int partition(int arr[], int l, int r){
	swap(arr[l], arr[random(r-l+1)]);
	int e = arr[l];
	
	int lt = l;
	int gt = r+1;
	//[l+1,i] [i+1,r]
	for(int i = l+1; i < gt; i++){
		if(arr[i] >= e)
			lt ++;
		else if(arr[i] < e){
			swap(arr[i],arr[gt-1]);
			gt --;
		}
	}
	swap(arr[l], arr[lt]);
	return lt;
}
```
39、数组中出现的次数超过一半的数字  
```

```
40、最小的k个数  
```

```
单路快排
```
//单路快排
void quickSort(int arr[], int n){
    __quickSort(arr, 0, n-1);
}

void __quickSort(int arr[], int l, int r){

    if( l >= r)
        return;

    int p = partition(arr,l,r);
    __quickSort(arr,l,p);
    __quickSort(arr,p+1,r);
}

void partition(int arr[],int l, int r){

    int v = arr[l];
    int i = l+1;
    int lt = l;//[l,lt] [lt+1,i-1]

    while(i <= r){
        if(arr[i] < v){
            swap(arr[lt+1],arr[i]);
            i++;
            lt++;
        }else
            i++;
    }
    swap(arr[lt-1],arr[l]);
    return lt;
}
```
双路快排  
```
//双路快排
 void quicksort(int arr[], int l, int r){
     if(l >= r){
         return;
     }

     int p = partition(arr, l, r);
     quicksort(arr,l,p-1);
     quicksort(arr,p+1,r);
 }

 void partition(int arr[],int l, int r){
     int v = swap(arr[l],arr[rand(r-l+1)]);
     //[l+1,j-1]  [j+1,r]
     int j = l+1;
     int j = r;
     while(i < j){
         if(i < j && arr[i] <= v)
             i ++;
         if(j > i && arr[j] >= v)
             j --;
         swap(arr[i],arr[j]);
     }
    swap(arr[l],arr[j]);
     return j;
 }
```
三路快排  
```
//三路快排 
void quicksort(int arr[], int l, int r){
    if(l <= r)
        return;

    int v = swap(arr[l], arr[r-l+1]);
    int lt = l;
    int gt = r;
    //[l+1,i] [i+1,r]
    for(int i = l+1; i < gt; i++){
        if(arr[i] < v) {
            swap(arr[lt + 1], arr[i]);
            lt++;
        }else if(arr[i] > v){
            swap(arr[i], arr[gt]);
            gt--;
                }
    }
    swap(arr[l],arr[lt]);

    quicksort(arr,l,lt-1);
    quicksort(arr,gt,r);

    }
}
```
选择排序   
```
void selectionSort(int arr[], int n){
    
    for(int i = 0; i < n; i ++){
        int minIndex = i;
        for(int j = i+1; j < n; j++){
            if(arr[minIndex] > arr[j])
            minIndex = j;
        }
        swap(arr[i],arr[minIndex]);
    }

    return;
}

```
day6  
- 递归和循环  
递归  
10 斐波那契数列  
```
int Fibonacci(int n){
	if(n <= 0)
	return 0;
	if(n == 1)
	return 1;
	
	int fib1 = 0;
	int fib2 = 1;
	int ret = 0;
	
	for(int i = 2; i <= n; i ++){
		ret = fib1 + fib2;
		fib1 = fib2;
		fib2 = ret;
	}
	
	return ret;
}
```
青蛙跳台阶  
```
void Fibonacci(int n){
	
	if( n <= 0)
		return 0;
		
	if( n == 1)
		return 1;
		
	return Fibonacci(n-1) + Fibonacci(n-2);
}
```
60  n个骰子的点数  
```

```
循环  
14、剪绳子  
47、礼物的最大价值  
48、最长不含重复字符的子字符串  

- 栈和队列  
9、用两个栈实现队列  
```
template<typename T> class CQueue{
public:
	CQueue();
	~CQueue();
	
	void appendTail(const T& node);
	T deleteHead();
	
private:
	stack<T> stack1;
	stack<T> stack2;
}

template<typename T> void CQueue<T>::appendTail(const T& element){
	stack1.push(element);
}

template<typename T> T CQueue<T>::deleteHead(){

	if(stack2.size() <= 0)
		while(stack1.size() != 0){
			T temp = stack1.top();
			stack2.push(temp);
			stack1.pop();
		}
	
	if(stack2.size() == 0)
		return -1;
		
	T temp = stack2.top();
	stack2.pop();
	
	return temp;
}
```

用两个队列实现栈  
```
template <typename T> class CStack(){
	public:
		CStack();
		~CStack();
		
		void appendTail(const T& node);
		T deleteHead();
		
	private:
		queue<T> queue1;
		queue<T> queue2;
}

template <typename T> void CStack::appendTail(const T& element){
	if(queue1.size() != 0)
		queue1.push(element);
	else
		queue2.push(element);
}

template <typename T> T CStack::deleteHead(){
	if(queue1.size() =< 0 || queue2.size() =< 0)
	return -1;//throw("all queue is empty")
	
	T ret;
	if(queue1.size() > 0){
		while(queue1.size() != 1){
			T temp = queue1.front();
			queue2.push(temp);
			queue1.pop();
		}
		ret = queue1.fornt();
		queue1.pop();
	}
	if(queue2.size() > 0){
		while(queue2.size() != 1){
			T temp = queue2.front();
			queue1.push(temp);
			queue2.pop();
		}
		ret = queue2.fornt();
		queue2.pop();
	}
	
	return ret;
}
```

31、栈的压入、弹出序列  
```

```

30、包含min函数的栈  
```

```

32、从上到下打印二叉树  
```

```

day8  
树  
- 二叉树  
二叉搜索树  
36、二叉搜索树与双向链表  
68、树中两个节点的最低公共祖先  
堆  (最大堆，最小堆)  
红黑树  
基于红黑树：set multiset map multimap  
40、最小的k个数  

- 考查遍历  
前序遍历  
中序遍历  
后序遍历  
层序遍历  
26、树的子结构  
34、二叉树中和为某一值的路径  
55、二叉树的深度  

实现方式:  
递归  
循环 
- 考查对遍历特点的理解  
 7、重建二叉树  
 33、二叉搜索树的后序遍历序列  
```

```
=======  
你最喜欢的花是  
玫瑰  
闻它你需要怎做  

尽量憋气, 越久越好  
然后一口呼出来
>>>>>>> d1dc6b3c89d140bf297dbc0cb97c250eaa3ed8ac
