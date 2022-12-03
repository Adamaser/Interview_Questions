# Interview_Questions

***********************************************************
`C++ 板块`
------- 
**题目(1):`C++用户空间内存分区有哪些?作用是什么?`**

- **代码区**:存放程序代码,不允许修改,编译后的二进制文件存放在这里。
- **静态数据区**: 在编译器进行编译的时候就为该变量分配的内存,即全局变量和静态变量(用static声明的变量),存放在这个区的数据程序全部执行结束后系统自动释放,声明周期贯穿于整个程序执行过程。全局变量和静态变量的存储是放在一块的,初始化的全局变量和静态变量在一块区域(.data),未初始化的全局变量和未初始化的静态变量在相邻的另一块区域(.bss)。
- **堆区**:这部分存储空间完全由程序员自己负责管理,它的分配和释放都由程序员自己负责。这个区是唯一一个可以由程序员自己决定变量生存 期的区间。可以用malloc,new申请对内存,并通过free和delete释放空间。如果程序员自己在堆区申请了空间,又忘记将这片内存释放掉,就会造成内存泄露的问题,导致后面一直无法访问这片存储区域。但程序退出后,系统自动回收资源。
- **栈区**:存放函数的形式参数和局部变量,由编译器分配和自动释放,函数执行完后,局部变量和形参占用的空间会自动被释放。效率比较高,但是分配的容量很有限。
- **常量区**:存放常量的区间,如字符串常量等,注意在常量区存放的数据一旦经初始化后就不能被修改。 程序结束后由系统释放。 

**题目(2):`C++ static关键字有哪些作用?`**

- **全局静态变量**:位于静态存储区,程序运行期间一直存在,对外部文件不可见。(表明一个全局变量只对定义在同一文件中的函数可见。)

- **局部静态变量**:位于静态存储区,在局部作用域可以访问,离开局部作用域之后static变量仍存在,但无法访问。(表明该变量的值不会因为函数终止而丢失。)

- **静态函数**:即在函数定义前加static,函数默认情况下为extern,即可导出的。加了static就不能为外部类访问。注意不要在头文件声明static函数,因为static只对本文件有效。(表明该函数只在同一文件中调用。)

- **类的静态成员**:可以实现多个不同的类实例之间的数据共享,且不破坏隐藏规则,不需要类名就可以访问。类的静态存储变量是可以修改的。可以通过<对象名>::<静态成员>进行访问。(表明对该类所有对象这个数据成员都只有一个实例。即该实例归所有对象共有。)

- **类的静态函数**:不能调用非静态成员,只可以通过对象名调用<对象名>::<静态成员函数>(意味着一个静态成员函数只能访问它的参数、类的静态数据成员和全局变量)

**题目(3):`指针常量和常量指针的区别?`**

指针常量:指针本身是常量。该指针只能指向某个常量,不可再指向其他常量。(指针地址不可变,值可变)

(注:指针常量在定义时要赋初值)

```c++
int a = 0,b = 0;
int* const p = &a;
*p = 1; //正确,可以修改值
p = &b; //错误,不可改变地址
```

常量指针:指向常量的指针。该指针指向一个常量,常量的值不可变,不可以通过该指针修改其值,但是该指针可以指向其他常量。(指针地址可以变,值不能变)

```c++
int a = 0,b = 0;
const int *p = &a;
*p = 1; //错误,不可修改常量值
p = &b; //正确,可以指向另一个常量
```

区分记忆:如果关键字 const 出现在 * 号左边,表示被指物是常量,侧重点是常量,值不可变;如果出现在 * 号右边,表示指针自身是常量,侧重点是指针,地址不可变;如果出现在 * 号两边,表示被指物和指针都是常量,地址和值都不可变。(简化记忆:const 与 * 号,谁在左边就以谁为侧重点)

**题目(4):`动态库和静态库的区别?`**

            静态库和动态库最本质的区别是:该库是否被编译进目标程序内部。
**静态库**:一般扩展名为(.a或.lib),这类库在编译的时候会直接整合到目标程序中,所以利用静态函数库编译的文件会比较大,这类函数库最大的优点就是编译成功的可执行文件可以独立运行,不再需要向外部函数要求读取函数库的内容,但是从升级难易程度来看,明显没有优势,如果函数库更新,需要重新编译。
**动态库**:一般扩展名为(.so或.dll),与静态函数库被整个捕捉到程序中不同,动态函数库在编译的时候,在程序里只有一个指向的位置而已,也就是当可执行文件需要使用到函数库机制的时候,程序才会读取函数库使用,也就是说可执行文件无法单独运行。这样从产品升级角度方便升级,只要替换对应动态库即可,不必重新编译整个可执行文件。
**总结**:从产品化的角度,发布的算法库或功能库尽量使用动态库,这样方便更新和升级,不必重新编译整个可执行文件,只需要新版本动态库替换掉旧的动态库即可。从函数库集成的角度,若要将发布的所有子库集成为一个动态库向外提供接口,那么就需要将所有子库编译成一个静态库,这样所有子库就可以全部编译进动态库中,由最终的一个集成库向外提供功能。

**题目(5) :`Struct 和 Class 的区别?`**

- struct 是值类型,class 是对象类型。
- struct 不能被继承,class 可以被继承。
- struct 默认的访问权限是public,而class 默认的访问权限是private。
- struct的new和class的new是不同的。struct的new就是执行一下构造函数创建一个新实例再对所有的字段进行Copy。而class则是在堆上分配一块内存然后再执行构造函数,struct的内存并不是在new的时候分配的,而是在定义的时候分配

**题目(6) :`vertor 和 deque 的区别?`**

- deque 两端都能快速安插和删除元素。
- 元素的存取和迭代器的动作比vector稍慢。
- 迭代器需要在不同区块间跳转,所以它非一般指针。
- 因为deque使用不止一块内存(而vector必须使用一块连续内存),所以deque的max_size()可能更大。
- deque的内存区块不再被使用时,会自动被释放。deque的内存大小是可自动缩减的。

**题目(7):`queue 和 stack 的异同?`**

- 都是线性结构。
- 插入操作都是限定在表尾进行。
- 都可以通过顺序结构和链式结构实现。
- 删除数据元素的位置不同,栈的删除操作在表尾进行,队列的删除操作在表头进行。

**题目(8):`list 的优缺点`**

***优点***:

- 采用动态存储分配,不会造成内存浪费和溢出。
- 链表执行插入和删除操作十分方便,修改指针即可,不需要移动大量元素。

***缺点***:

- 链表灵活,但是空间(指针域) 和 时间(遍历)额外耗费较大。


**题目(9):`指针与引用有什么区别?`**

- 指针会占用内存,引用不占用内存。

- 引用在定义时必须初始化。

- 没有空的引用,但是有空的指针。

**题目(10):`数组与链表的区别是什么?`**

- 数组在内存中是连续的,链表在内存中是不连续的,靠指针连接。

- 数组占用的总内存相对链表要小,因为链表除了要保存数据外,还要保存用于连接的指针。

- 数组方便排序和查找,但不方便删除和插入;而链表则方便删除和插入,不方便排序和查找。

**题目(11):`头文件中的ifndef/define/endif有什么作用?`**

这是C++预编译头文件保护符,保证即使文件被多次包含,头文件也只定义一次。

**题目(12):`请描述进程和线程的区别`**

(1)进程是程序的一次执行,线程是进程中的执行单元;

(2)进程间是独立的,这表现在内存空间、上下文环境上,线程运行在进程中;

(3)一般来讲,进程无法突破进程边界存取其他进程内的存储空间;而同一进程所产生的线程共享内存空间;

(4)同一进程中的两段代码不能同时执行,除非引入多线程。

**题目(13):`头文件中的ifndef/define/endif和#pragma once有什么区别?`**

- #ifndef的方式受C/C++语言标准支持。它不光可以保证同一个文件不会被包含多次,也能保证内容完全相同的两个文件不会被不小心同时包含。缺点就是如果不同头文件中的宏名不小心“撞车”,可能就会导致你看到头文件明明存在,编译器却硬说找不到声明的状况——这种情况有时非常让人抓狂。由于编译器每次都需要打开头文件才能判定是否有重复定义,因此在编译大型项目时,ifndef会使得编译时间相对较长,因此一些编译器逐渐开始支持#pragma once的方式。
- #pragma once一般由编译器提供保证:同一个文件不会被包含多次。注意这里所说的“同一个文件”是指物理上的一个文件,而不是指内容相同的两个文件。你无法对一个头文件中的一段代码作pragma once声明,而只能针对文件。其好处是,你不必再费劲想个宏名了,当然也就不会出现宏名碰撞引发的奇怪问题。大型项目的编译速度也因此提高了一些。对应的缺点就是如果某个头文件有多份拷贝,本方法不能保证他们不被重复包含。当然,相比宏名碰撞引发的“找不到声明”的问题,这种重复包含很容易被发现并修正。在C/C++中,#pragma once是一个非标准但是被广泛支持的方式。

**题目(14):`面向对象的三大特性是什么?`**

继承、封装、多态。

**题目(15):`什么是多态?`**

多态是指相同的操作或函数、过程可作用于多种类型的对象上并获得不同的结果。不同的对象,收到同一消息可以产生不同的结果,这种现象称为多态。

**题目(16):`什么是虚函数?`**

虚函数是多态的实现方式,多态意指相同的消息给予不同的对象会引发不同的动作(一个接口,多种方法)。其实更简单地来说,就是“在用父类指针调用函数时,实际调用的是指针指向的实际类型(子类)的成员函数”。多态性使得程序调用的函数是在运行时动态确定的,而不是在编译时静态确定的。当使用类的指针调用成员函数时,普通函数由指针类型决定,而虚函数由指针指向的实际类型决定。
参考链接:https://blog.csdn.net/daaikuaichuan/article/details/88364336

**题目(17):`vector中的size和capacity的区别`**

- size表示当前vector中有多少个元素。
- 而capacity函数则表示它已经分配的内存中可以容纳多少元素。

**题目(18):`vector 中的 resize 和 reserve 的区别`**

- resize()函数和容器的size息息相关。调用resize(n)后,容器的size即为n。至于是否影响capacity,取决于调整后的容器的size是否大于capacity。
- reserve()函数和容器的capacity息息相关。调用reserve(n)后,若容器的capacity<n,则重新分配内存空间,从而使得capacity等于n。如果capacity>=n,则capacity无变化。


**题目(19):` C++中静态成员初始化要注意什么?`**

```c++
public:
	MyClass();
	~MyClass();
 
private:
	int a ; 变量(非静态变量)
	const int b; 常量(非静态常量) //❌非静态常量需要进行初始化
	static int c; 静态变量
	static const int d; 静态常量
```

在C\++11当中,对C++98进行了改进,常量、变量、静态常量可以在类内初始化,而静态变量不可以在类内初始化。
如下:
```c++
int a =1;
	const int b=2;
	static int c=3; //❌带有类内初始化表达式的静态数据成员必须具有不可变的常量整型类型
	static const int d=4;
```
要在类内对静态成员进行初始化,必须是静态常量(d)才可以,而静态变量(c)不可以,因为静态成员属于整个类,而不属于某个对象,如果在类内初始化,会导致每个对象都包含该静态成员,这是矛盾的静态常量成员是可以在类内声明的。静态常量成员可以直接赋值,因为静态常量成员是常量,不允许修改,这种情况下是否所有的对象共享同一份数据已经不重要了,因为都是同一常量数据,而且如果不允许直接赋值,那么这个常量就没有意义了,直接就是系统默认的值了。

**题目(20):` C++中for循环与while循环的区别是什么?`**

```c++
int number = 10;
for(int i = 0;i <= number;i++)
{
    system.out.print(i + "\t");
}
```

```c++
int number = 0;
while(number < 10)
{
    system.out.print(number + "\t");
    number++;
}
```
- **循环结构不同**:for语句事先指导循环次数,先判断后执行;而while不知道循环次数,先判断后循环。
- **使用目的不同**:for循环的目的是为了限制循环体的执行次数,使结果更精确;while循环的目的是为了反复执行语句或代码块。
- **语法结构不同**for循环的语法为:for (变量 = 开始值;变量 <= 结束值;变量 = 变量 + 步进值) {需执行的代码 };while循环的语法为:while (<条件>) {需执行的代码 }。

**题目(21):` 二叉树的几种遍历方式`**
前序遍历：根结点 —> 左子树 —> 右子树

中序遍历：左子树—> 根结点 —> 右子树

后序遍历：左子树 —> 右子树 —> 根结点

层次遍历：仅仅需按层次遍历就可以

![image](https://user-images.githubusercontent.com/114053234/204471602-4b3b6606-04b5-4ae2-997b-370147a55641.png)

前序遍历：1 2 4 5 7 8 3 6

中序遍历：4 2 7 5 8 1 3 6

后序遍历：4 7 8 5 2 6 3 1

层次遍历：1 2 3 4 5 6 7 8

**题目（22）：`静态绑定`**

**静态绑定：绑定的是静态对象类型，发生在编译时。**

```c++
class A{
    public:
    virtual void test(){
        cout << "A test" << endl;
    }
    void cnn(){
        test();
        cout << "CNN" << endl;
    }
};

class B : public A{
    public:
    void test(){
        cout << "B test" << endl;
    }
};

int main(){
   B b;
   A a = (A) b;
   a.cnn();   //A test    CNN
}
```

**题目（23）：`动态绑定`**

**动态绑定：绑定的是动态对象类型，发生在运行时。**

满足3个条件：**有指针或引用**、**指针向上转型**、**虚函数**

```c++
   A* a = new B;
   a->cnn();  //B test    CNN
```

**题目（24）：`什么是内存抖动（Thrashing）`**

内存页面的频繁更换，导致整个系统的效率急剧下降这个现象称为内存抖动。
抖动一般是内存分配算法不好，内存太小或者程序的算法不佳引起的页面频繁从内存调入调出。

**题目（25）：`文件操作的唯一依据是什么？`**

文件句柄。

文件描述符：open 函数的返回值，返回当前操作文件的句柄，后续通过文件描述符（文件句柄）来读文件或者写文件。
```
int open(const char *pathname , int flags , mode_t mode);
ssize_t read(int fd , void *buf , size_t count);
ssize_t write (int fd,const void *buf ,size_count);
```

**********************************************************
`SLAM板块`
------- 
**题目(1):`视觉和IMU融合有哪些优势?`**

IMU可以对短时间内的快速运动可以做出较好的估计,但是存在零偏问题,导致长时间的积分会产生飘逸甚至发散。视觉slam直接测量旋转与平移,不会产生飘逸,但是在相机快速运动时特征点容易丢失,无法进行匹配,同时容易受图像遮挡、动态障碍物无法判断等问题。IMU和视觉两者的优劣势互补,可以有效解决各自存在的问题。

**题目(2):`slam中为什么要引入李群李代数?`**

在进行最小二乘的优化问题时,对于复杂的函数,通常采用迭代的方法,需要对函数进行求导进而得到一个增量,不断更新当前的优化变量。 问题在于,旋转矩阵更新的方式是不断左乘新的旋转变量而不是加,即SO(3)上没有加法,它的导数无法按照导数的定义进行计算。因此,我们可以将其映射为李代数,化乘为加,通过李代数求得增量,再通过指数映射将其映射回SO(3)进行当前估计值的更新。                                                                                            
**题目(3):`关键帧是什么?有什么作用?`**

关键帧是几帧普通帧中较具有代表性的一帧。SLAM方案中一般会将普通帧的深度投影到关键帧上,故一定程度上,关键帧是普通帧滤波和优化的结果,防止无效和错误信息进入优化过程;可以降低局部相邻关键帧中的信息冗余度,提高计算资源的效率。

**题目(4):`关键帧如何选取?`**

不同算法对关键帧选取的策略有所不同,以orbslam和vins-mono为例:

orbslam:
1. 距离上一次全局重定位后需要超过20帧图像。
2. 局部地图构建处于空闲状态,或距上一个关键帧插入后,已经有超过20帧图像。
3. 当前帧跟踪少于50个地图云点。
4. 当前帧跟踪少于参考关键帧K_ref云点的90%。

vins-mono:
1. 两帧间跟踪的特征点的平均视差大于某个阈值。
2. 当前跟踪特征点少于特定阈值。

**题目(5):`描述一下PnP问题`**

Perspective-n-Points, PnP(P3P)提供了一种解决方案,它是一种由3D-2D的位姿求解方式,即需要已知匹配的3D点和图像2D点。目前遇到的场景主要有两个,其一是求解相机相对于某2维图像/3维物体的位姿;其二就是SLAM算法中估计相机位姿时通常需要PnP给出相机初始位姿。

在场景1中,我们通常输入的是物体在世界坐标系下的3D点以及这些3D点在图像上投影的2D点,因此求得的是相机坐标系相对于世界坐标系(Twc)的位姿;
在场景2中,通常输入的是上一帧中的3D点(在上一帧的相机坐标系下表示的点)和这些3D点在当前帧中的投影得到的2D点,所以它求得的是当前帧相对于上一帧的位姿变换。

**题目(6):`单目视觉slam的尺度不确定性是什么?`**

单目视觉中,丢失了地图点的深度信息,通常经过对极几何(三角化)的方法恢复深度,地图点深度(d)与偏移(t)相关,地图点不同深度,则对应一个t值。同样的观测可以计算出无穷组d和t。导致了单目视觉的尺度不确定性。因此需要进行初始化来固定一个尺度,后面的计算都以该尺度为单位进行。



**********************************************************
`路径规划板块`
------- 

**题目(1):`路径规划与轨迹生成常用的方法有哪些?`**

基于搜索的路径规划有两项,分别是Dijkstra和A*算法;
基于采样的路径规划有三项,分别是RRT、RRT*和Informed RRT*算法;
基于智能算法的路径规划两项,分别是遗传算法和蚁群算法。

轨迹生成常用的有两种方法,分别是多项式曲线和贝塞尔曲线;
轨迹优化目标有两种方法,分别是最小化snap和轨迹长度;
轨迹约束有两种方法,分别是软约束和硬约束。

**题目(2):`路径规划中常用的地图种类有哪些?`**

Occupancy grid map(栅格地图),包括了二维栅格地图与三维珊格地图,会消耗大量的时间去寻找路径,不适合作为全局的地图格式来存储;
Octo-map(八叉数地图),可以看作是栅格地图的改进后的地图,以正方体举例子,先分为8份,对于有障碍物的再分,没有障碍物的大栅格就不再区分;
Point cloud map(点云地图),可以尽可能地保留原始环境信息,有益于三维重建,但需要在环境模型中存储大量无序的三维数据点;
ESDF(可以看作珊格地图优化版本),显示障碍物的位置,并带有距离、方向信息,有利于后期做局部路径规划算法。

**题目(3):`软约束与硬约束分别是什么?`**

通常轨迹优化问题通常会有目标函数,然后我们会为决策变量设置一些约束条件,这些约束条件缩小了可行域的范围,约束条件会存在以下几种可能性:约束合适,简化了目标函数的求解,去掉了鞍点留下来最值点(正常约束);约束过多,发现不可能满足所有的约束条件,可行域为空集(过约束);约束不足,发现可行域太大,搜索算法很费时(欠约束)。

硬约束即是给地图划分出绝对安全的空间,并在空间交界处放置必经点,将路径约束在安全空间内。

软约束则是通过构建惩罚函数,利用类似于场的原理使得障碍物原理,在任何一点处的位置都由周围环境的惩罚函数决定,生成局部路径比较快。

**题目(4):`已经有了后端的轨迹优化为什么还要有符合动力学的路径规划算法?`**

传统的路径规划算法被明显地分为前端路径点生成和后端轨迹优化两部分,传统的路径点生成方法生成的路径在具有比较多动力学模型约束的时候可能无法生成符合动力学的轨迹,导致花费大量的资源重新进行搜索。前期考虑了动力学模型后就只需要在原有的轨迹上进行优化(可以通过OPVB或者minimum-snap/minimum-jerk)就可以得到一条可以运行的轨迹。

符合动力学模型的路径生成方法可分为两类,离散状态量与离散控制量,离散控制量是从根部出发衍生,而离散状态量则是每一个点画出轨迹后连接。

**题目(5):`凸优化问题的优势?`**

凸优化问题的局部最优解就是全局最优解

很多非凸问题都可以被等价转化为凸优化问题或者被近似为凸优化问题

凸优化问题的研究较为成熟,当一个具体被归为一个凸优化问题,基本可以确定该问题是可被求解的

**题目(6):`轨迹优化中为什么是minimum snap, minimum jerk或其它??`**

nap是位置的四阶导数。由于四旋翼的微分平坦特性,snap对应的是角加速度,也就是力矩层面。力矩层面又和电机转速是直接相关的,因此最小化这个指标可以获得理论上最节省能量的轨迹。
(对一些存在平坦输出的非线性系统,如果可以找到一组系统输出,使得所有状态变量和输入变量都可以由这组输出及其有限阶微分进行表示,那么该系统即为微分平坦系统)

当然也可以优化不同阶数的导数,minimum jerk就对应角速度,也可以优化加速度,甚至是速度,只是在不同意义下得到的最优。
 Minimum jerk:最小化角速度,有利于视觉跟踪
 Minimum snap:最小化差速推力,节省能源
上面说的对应可以理解为,假如我们完全开环控制四旋翼无人机。也就说根据设计的的轨迹,直接设计力矩(或者是电机转速指令,这个是通过常数矩阵映射的)发送给无人机,让他跟踪上轨迹。那么就对应需要轨迹四阶导数的信息(也可以说这是微分平坦理论告诉我们的)。


**题目(7)`RRT算法优缺点`**

***优点:***

- 搜索效率比较高,搜索速度比较快
- 适用于高维空间,不会产生维度灾难的问题
- 只需对状态空间采样点进行碰撞检测,避免了对空间的建模

***缺点:***

- 规划出的路径质量一般,可能存在棱角、不够光滑
- RRT算法不太适用于存在狭长空间的环境
- 规划出的路径可能不是最优路径
- 不适用于动态环境的路径规划

**题目(8):`PRM算法优缺点`**

***优点:***

- 适用于高维空间和复杂约束的路径规划问题
- 搜索效率高,搜索速度快

***缺点:***

- 概率完备但不是最优

**题目(9):`无人机路径生成时的飞行走廊指的是什么?`**

在获取到路径规划出的原始路径后,提取环境中的自由空间构建用于后端优化的飞行走廊。充分利用好自由空间来找到解空间并获得最优解对于生成轨迹来说是同样重要的。因此,我们的方法是初始化飞行走廊并进行膨胀。对于路径中的每个节点,可以通过查询ESDF轻易获得从节点位置到最近障碍物的距离。在每个节点处,我们获得了一个安全空间的球体以抵御最近的障碍物,并将飞行走廊初始化为该球体的内接立方体。接着通过查询沿轴方向x,y,z的相邻网格来扩大每个多维数据集,直到它到达最大的自由体积为止(如图3所示)。由于我们将飞行走廊初始化为一系列密集节点,因此某些节点可能包含相同的放大立方体,这是不必要的,但会增加后端优化的复杂性。因此,需要修剪掉走廊中所有重复的立方体。

**题目(10):`为什么贝塞尔曲线也能用于路径生成?`**

路径生成要求在安全空间内生成光滑轨迹,且要符合无人机动力学要求、waitpoint连续性的要求。之所以采用贝塞尔曲线是因为贝塞尔曲线的性质符合要求,其性质如下所示:

- **系数和为1**:由于其系数可以化为二项式展开,故其各项系数之和为1.
- **对称性**:其系数二项式展开后具有对称性,使得其具有了对称性的特征,即第i项系数和倒数第i项系数相同.
- **递归性**:n阶贝塞尔曲线可递归地化为两项n-1阶贝塞尔曲线表示.
- **凸包性**:贝塞尔曲线始终会在包含了所有控制点的最小凸多边形中, 不是按照控制点的顺序围成的最小多边形.
- **端点性**:第一个控制点和最后一个控制点,恰好是曲线的起始点和终点.这一点可以套用二项式展开来理解,t=1或者0的时候,相乘二项式的系数,出了初始点或者末尾点,其余的都是0.
- **一阶导性质**:贝塞尔曲线在t=0时的导数是和P0P1的斜率(导数)是相同,t=1时的导数是和P3P4的斜率(导数)是相同,这一点的性质可以用在贝塞尔曲线的拼接,只要保证三点一线中的中间点是两段贝塞尔曲线的连接点,就可以保证两端贝塞尔曲线的导数连续.

- **综上所述**:凸包性保证了由贝塞尔曲线生成的路径在安全走廊中,满足了不等式约束。而一阶导数性质保证了贝塞尔曲线生成的路径在路径点处光滑,满足了等式约束。故贝塞尔曲线完美适用于路径生成。

**题目(11):`RRT*算法与RRT算法的区别`**

RRT*算法与RRT算法的区别主要在于两个针对新节点 x_{new} 的重计算过程，分别为：

* 重新为 x_{new} 选择父节点的过程， 比起RRT多了一个rewire的过程。

* 重布线随机树的过程。

**题目(10):`与贝塞尔曲线相比，B样条曲线的优缺点`**

相比于bezier曲线，b样条有两个优势：
（1）b样条基函数多项式次数独立于控制点个数。
（2）允许局部控制曲线。
缺点：比bezier更加复杂。

**********************************************************
`控制板块`
------- 
**题目(1):`MPC与LQR的区别有哪些?`**

- 1) 控制对象:mpc可对线性和非线性进行控制        LQR只对线性系统

- 2) 求解目标函数方法。MPC:一般转化为二次规划问题,利用求解器进行求解,生成控制序列 。LQR:一般采用变分法,通过求解黎卡提方程进行逼近,最终获取控制序列。

- 3) 工作时域。两者的工作时域不同,mpc在有限控制时域求解,LQR在无限时域里求解。MPC:是求解预测时间段Np内的控制序列 ,并在下个周期后进行滚动优化,每次只需控制序列的第一个值作为控制的输入。为了提高速度,可只计算控制时间段Nc 内的控制序列,时间段(Np-Nc)内的控制量均取Nc-1处的值即可。LQR:求解预测时间段内的控制序列,只求解一次,每个周期下取对应的控制量即可,而不考虑之前周期下实际与规划的误差。

- 4) LQR不滚动优化,预测时间段内的控制序列只求解一次,没有考虑实际与规划的误差。

- 5) 有无约束。mpc可以很好的处理约束问题;LQR要求无约束,假设对控制量无约束。

**题目(2):`MPC与PID的区别有?`**

- mpc是基于模型进行预测控制的,依赖模型;而pid是基于反馈的,不需要模型。

- pid不适用于非线性系统;mpc适用与非线性系统。

**题目(3):`在mpc算法中,获得的最优控制序列一定可以使系统稳定吗?`**

- 不一定。直观理解:该算法是在有限时域内作优化求解控制序列,在有限时域里的优化只反映局部信息,而不是整个时域里的最优解,没有反映系统的真正性能。要使系统稳定,则需在目标函数里对终端误差控制矩阵进行设计。换句话说,也就是将作优化的时域设置成无限时域。

**题目(4):`带约束线性mpc保证稳定性的方式主要有哪几种?`**

- 主要有四种,终端等式约束,无穷时域,终端不等式约束,终端代价。其中终端等式约束可以选取,x(N|k)=0。



