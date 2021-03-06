#lec9 虚存置换算法spoc练习

## 个人思考题
1. 置换算法的功能？

2. 全局和局部置换算法的不同？

3. 最优算法、先进先出算法和LRU算法的思路？

4. 时钟置换算法的思路？

5. LFU算法的思路？

6. 什么是Belady现象？

7. 几种局部置换算法的相关性：什么地方是相似的？什么地方是不同的？为什么有这种相似或不同？

8. 什么是工作集？

9. 什么是常驻集？

10. 工作集算法的思路？

11. 缺页率算法的思路？

12. 什么是虚拟内存管理的抖动现象？

13. 操作系统负载控制的最佳状态是什么状态？

## 小组思考题目

----
(1)（spoc）请证明为何LRU算法不会出现belady现象
```
    由于缺页的考察对象在LRU算法维护的栈，即要证明：对于页面数量为n与n+k (k>0) 分别对应的栈S(n)与S(n+k) 始终满足前者元素是后者子集即可。
为了后续的证明，这里加强条件，即下面将证明：对于页面数量为n与n+k (k>0) 分别对应的栈S(n)与S(n+k) 始终满足前者元素是后者子集，且任意元素在两栈中保持同样的相对顺序。（即不存在元素x1,x2 在S(n+k)与S(n)栈中位置分别是x1，x2 和 x2，x1）
 
采用归纳法证明 对LRU算法进行步骤数T做归纳：
 
1、当T=0时，S(n)、S(n+k)始终为空栈，显然包含于后者 元素停留时间都是零
 
2、设对于任意T<n-1 均有S(n)包含于S(n+k)，则对于第n步 访问元素x，可能出现三种情况：
     a. x在S(n)与S(n+k)中均出现
     b. x在S(n)中出现 未在S(n+k)中出现
     c. x在S(n)与S(n+k) 均未出现
 
     对于a. 由于x均在栈中，则这第n步操作仅为将x移至栈顶，不发生元素变化，包含关系不变；S(n)与S(n+k)中所有
                元素间顺序关系保持不变。
 
     对于b. x不在S(n)中,则按要求要去掉栈底部元素并将x放到栈顶，即S(n)中元素减少，而S(n+k)元素不发生变化。
                故包含关系依然满足；同时S(n+k)中所有非x元素位置加1，S(n)中非栈底元素位置加1。这部分元素保持两栈
                中相同的位置关系；x元素都在栈顶，故所有元素在两栈中排序方式一致。
 
     对于c. 此时S(n)与S(n+k)都需要去掉栈底元素，只要考虑S(n+k)的栈底是否可能继续留在S(n)中。
                如果说S(n+k)的栈顶top_k 依旧留在S(n)中，则表示S(n)中原栈顶top_n（in S(n)） > top_k（in S(n)。
                由于S(n)元素包含于S(n+k)，故top_n也在S(n+k)中，而top_k是S(n+k)的栈底，故：
                top_n（in S(n+k)） < top_k（in S(n+k)）
                得到结论top_n、top_k在T-1步完成后两栈中的顺序关系不同，与假设前提矛盾。
               
                所以S(n+k)的栈底不可能继续留在S(n)，元素包含关系依旧成立。故两栈各自去掉栈底元素，这里也有两个情况：
                i. 若栈底元素相同，则分别去掉，其他元素位置加1，在两栈间保持同样的相对顺序；x放入栈顶，同样不影响
               ii. 若栈顶元素不同，则除两栈顶之外元素情况同i，唯一不同的是此时S(n)被去掉的栈头由于在自己的栈中位置
                   最大故S(n)将是S(n+k)的上半部分。
 
综合以上，可得到原假设：“对于页面数量为n与n+k (k>0) 分别对应的栈S(n)与S(n+k) 始终满足前者元素是后者子集，且任意元素在两栈中保持同样的相对顺序。” 成立。
即证明了LRU不会出现Belady现象。
```

(2)（spoc）根据你的`学号 mod 4`的结果值，确定选择四种替换算法（0：LRU置换算法，1:改进的clock 页置换算法，2：工作集页置换算法，3：缺页率置换算法）中的一种来设计一个应用程序（可基于python, ruby, C, C++，LISP等）模拟实现，并给出测试。请参考如python代码或独自实现。
```
#include<iostream>
#include<stdlib.h>

using namespace std;
#define M 2
int const A = 4;//Pages
int count = 0;
int Inside[A];
int const PageCount   =10;//Total page
int Page[PageCount];
int state[A];//clock state
int state2[A][M];// A:pos M:revise
double lost = 0.0;

bool isInside(int num){
	for(int i = 0; i < A; i++){
		if(Inside[i] == Page[num]){
			state[i] = 1;
			return true;
		}
	}
	return false;
}

bool change(){
	if((rand()%2+1) == 1 ){
		cout<<"Changed"<<endl;
		return true;
	}
	else
		return false;
}

bool isInside2(int num){
	for(int i = 0; i < A; i++){
		if(Inside[i] == Page[num]){
			if(change()){
				state2[i][0] = 1;
				state2[i][1] = 1;
			}
			else{
				state2[i][0] = 1;
			}
			return true;
		}
	}
	return false;
}

int whichpage(){
	int j;

	for(j=0; j < A;j++){
        if(state2[j][0] == 0&&state2[j][1] == 0){
			return j;
		}
	}
	for(j=0; j < A;j++ ){
        if(state2[j][0] == 0&&state2[j][1] == 1){
			return j;
		}
		state2[j][0] = 0 ;
	}
	for(j=0; j < A;j++ ){
		state2[j][0] = 0 ;
	}
	return whichpage();
}

void LCLOCK(int num){
	int j;

	if(isInside2(num)){
		cout<<"Hit"<<endl;
		for(int i=0 ; i <A; i++)
			
        cout<<"Block"<<i<<"#content:"<<Inside [i]<<endl;
	}
	else
		if(count == A){
			lost++;
			j =whichpage();
			Inside[j] = Page[num];
			state2[j][0] = 1;
			for(int i=0 ; i <A; i++)
			
           cout<<"Block"<<i<<"#content:"<<Inside [i]<<endl;
			
		}

		else{
			Inside[count] = Page[num];
			count++;
			for(int i=0 ; i <A; i++)
			cout<<"Block"<<i<<"#content:"<<Inside [i]<<endl;
		}
}

int main()
{
  char ch ;
  for(int i = 0; i < PageCount; i++){
	    Page[i] =rand()%9 + 1;
		cout<<Page[i]<<" ";
	}
  cout<<endl;

  lost = 0;
  count = 0;
  for(int m = 0; m < A; m++)
  {
  	for(int n = 0; n < 2;n++)
	state2[m][n] = 0;
  }
  for(int j = 0; j < A; j++)
  	Inside[j] = 0;
	
  for(int i = 0; i < PageCount; i++)
  {
    cout<<"read Page["<<i<<"]="<<Page[i]<<endl;
    LCLOCK(i);
  }
  cout<<"\nTimes"<<PageCount<<"\nBreak times"<<lost<<"\nPercentage"<<lost/(PageCount)<<"\n"<<endl;
  
  return 0;
}
```



 - [页置换算法实现的参考实例](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab3/page-replacement-policy.py)
 
## 扩展思考题
（1）了解LIRS页置换算法的设计思路，尝试用高级语言实现其基本思路。此算法是江松博士（导师：张晓东博士）设计完成的，非常不错！

参考信息：

 - [LIRS conf paper](http://www.ece.eng.wayne.edu/~sjiang/pubs/papers/jiang02_LIRS.pdf)
 - [LIRS journal paper](http://www.ece.eng.wayne.edu/~sjiang/pubs/papers/jiang05_LIRS.pdf)
 - [LIRS-replacement ppt1](http://dragonstar.ict.ac.cn/course_09/XD_Zhang/(6)-LIRS-replacement.pdf)
 - [LIRS-replacement ppt2](http://www.ece.eng.wayne.edu/~sjiang/Projects/LIRS/sig02.ppt)
