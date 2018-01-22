---
title: 如何使用C++新特性避免错误
date: 2018-01-20 00:00:00
tags: C++
---
原文：http://www.cplusplus.com/articles/y807M4Gy/

C++程序最主要的一个问题是有大量未定义的构造器，它会造成许多意想不到的错误。使用静态分析工具有时可以发现极个别的问题。然而，最佳的检测错误的时期在编译阶段。采用现代C++技术，不仅帮助我们写出简洁的代码，而且更加安全可靠。

<!--more-->

提及现代C++，人们通常思考的是什么？并行，编译时计算(compile-time calculation)，RAII，lambda表达式，范围(ranges)，concepts,modules以及标准库自带的一些重要组件(例如文件系统API)。这些都是很酷的现代特性，它们有望加入下一代C++标准。然而，我更关注那些可以帮我们写出更安全代码的标准。运行静态分析工具，我们就能发现大量的错误，而不是到编译阶段。但是，现代C++可以帮我们做到。因此，我建议我们可以在开源项目中，利用PVS-Studio检查错误。

## Automatic type inference

在C++中，新增关键字auto和decltype。或许，你已经知道如何使用它们。

std::map<int, int> m;
auto it = m.find(42);
//C++98: std::map<int, int>::iterator it = m.find(42);

对于长类型定义，auto是非常方便。然而，这些关键字和模板一起使用，代价是非常昂贵的。不要使用auto和decltype指定返回类型。

string str = ...;
unsigned n = str.find("ABC");
if (n != string::npos)

在64b程序中，string::npos的值比UINT_MAX大，UINT_MAX是一个unsigned类型。它似乎可以用auto代替。n的类型不重要。实际上，如果我们用auto代替，它会出现错误:

stirng str = ...;
auto n = str.find("ABC")；
if (n != string::npos)

auto并不是万能灵药。随意使用auto，会造成许多隐患。例如下列代码：

auto n = 1024 * 1024 * 1024 * 5;
char* buf = new char[n];

auto不能保存这么大的数，会造成内存泄漏。

auto还有造成另一些错误：无效的循环。例如：

std::vector<int> bigVector;
for (unsigned i = 0; i < bigVector.size(); ++i)
{...}

对于一个很大的数组,这个循环是无效的。

我们是否可以用auto代替？

std::vector<int> bigVector;
for (auto i = 0; i < bigVector.size(); ++i)
{...}

采用auto,错误不仅仍然存在，甚至变得更加糟糕。

采用简单类型auto并不好。当然，在一些简单的表达式(auto x = y)，它能正常工作，但是它也有可能带来无法预期的行为。更糟糕的是，它会造成错误更加隐蔽，不易被发现。稍后，我们用提到另一种现代方式来避免这种错误。


## dangerous countof


C++中一种危险的类型是数组。我们经常使用数组作为参数传入函数，程序员忘记它是一个指针，而用sizeof计算它的大小。

#define RTL_NUMBER_OF_V1(A) (sizeof(A)/sizeof((A)[0]))
#define _ARRAYSIZE(A) RTL_NUMBER_OF_V1(A)
int GetAllNeighbors(const CCoreDispInfo *pDisp,
                    int iNeighbors[512])
					{
...
if(nNeighbors < _ARRAYSIZE(iNeighbors))
	iNeighbors[nNeighbors++] = pCorner->m_Neighbors[i];
...
					}

注：这段代码摘自Source Engine SDK.

PVS-Studio警告：V511 The sizeof() operator returns size of the pointer, and not of the array, in 'sizeof (iNeighbors)' expression. Vrad_dll disp_vrad.cpp 60

冲突是由于计算参数中的数组大小:此数据对于编译器无用，只是提示程序员。

麻烦的是这个代码可以编译通过，程序员无法意识到程序有错误。一个有效的解决途径是使用元编程：

template < class T, size_t N><br>constexpr size_t countof( const T (&array)[N] )
{
    return N;
}

countof(iNeighbors); //complite-time error

如果我们使用countof函数，传递的参数不是数组，编译时会发生错误。在C++17中，我们可以使用std::size.

在C++11中,新增加函数std::extent,但是它不等价于countof,因为在无效参数时,它返回0.

使用sizeof和countof都会产生错误。
VisitedLinekMaster::TableBuilder::TableBuilder(VisitedLinkMaster* master, const uint8 salt[LINK_SALT_LENGTH])
:master_(master),
 success_(true){
 fingerprints_.reserve(4096);
 memcpy(salt_, salt, sizeof(salt));
 }
注：这段代码摘自Chromium.

PVS警告：

    V511 The sizeof() operator returns size of the pointer, and not of the array, in 'sizeof (salt)' expression. browser visitedlink_master.cc 968
    V512 A call of the 'memcpy' function will lead to underflow of the buffer 'salt_'. browser visitedlink_master.cc 968

如你所见，标准C++数组有许多问题。这也是为什么提倡使用std::array的原因。现代C++的API跟std::vector等容器的接口类似，使用std::array很难出错。

void Foo(std::array<uint8, 16> array)
{
array.size();
}

## for循环避免错误

许多错误发生在for循环中，这时，你可能回想"我究竟在哪里犯错了？是不是应该增加退出条件或者保存代码行？"不是，错误发生在循环中。看下面的代码：

const int SerialWindow::kBaudrates[] = {50, 75, 110, ...};

SerialWindow::SerialWindow() : ...
{
...
for(int i = sizeof(kBaudrates) / sizeof(char*); --i >= 0;)
{
message->AddInt32("baudrate", kBaudrateConstants[i]);
}
...
}
注：这段代码摘自Haiku Operation System。

PVS-Studio警告: V706 Suspicious division: sizeof (kBaudrates) / sizeof (char *). Size of every element in 'kBaudrates' array does not equal to divisor. SerialWindow.cpp 162

上一章节我们详细地讨论了这类型错误：数组的大小无法正确计算。使用std::size很容易修复它：

const int SerialWindow::kBaudrates[] = {50, 75, 110, ...};

SerialWindow::SerialWindow() : ...
{
...
for(int i = std::size(kBaudrates); --i >= 0;)
{
message->AddInt32("baudrate", kBaudrateConstants[i]);
...
}
}
下面有一种更好的方式(要求传入数组的大小)：
inline void CXmlReader::CXmlInputStream::UnsafePutCharsBack(
const TCHAR* pChars, size_t nNumChars)
{
if(nNumChars > 0)
{
for(size_t nCharPos = nNumChars - 1; nCharPos >=0; --nCharPos)
	UnsafePutCharBack(pChars[nCharPos]);
}
}
注：这段代码摘自Shareaza.
PVS-Studio 警告: V547 Expression 'nCharPos >= 0' is always true. Unsigned type value is always >= 0. BugTrap xmlreader.h 946


在这个循环中发生类型错误:程序员忘记unsigned类型的计算方式致使判断始终为真。你可能会想“这个问题怎么会发生？只有新手和学生才会犯这种错误。有经验的不会这么做。”不幸的是，这确实无法避免。当然，大家都明白unsigned >= 0始终为真。这类错误还会在哪出现？它们也会发生在代码重构阶段。试想一个项目需要从32b平台移植到64b。原先，int/unsigned作为索引类型；现在准备使用size_t/ptrdiff_t替换该类型。这样就会造成无符号型替换有符号型。

在我们的代码中，如何避免此类问题的出现？有人建议在C#或者Qt中，采用有符号类型。或许，它是一种解决办法，但是如果我们想使用更大的数据，无法避免不采用size_t.在C++中，是否有更安全的方式呢？当然有，无成员函数。这是容器数组和初始化列表的标准函数；是C++的一类常用规范。

char buf[4] = {'a', 'b', 'c', 'd'};
for (auto it = rbegin(buf); it != rend(buf); ++it)
{
std::cout << *ite;
}

现在，我们不需要考虑顺序和反序的差异。我们只关注是否使用简单数组或者循环数组。采用iterator是避免此类问题更好的解决方案。但是，是否存在更好的解决方案呢？使用range-based进行循环。

char buf[4] = {'a', 'b', 'c', 'd'};
for(auto it : buf)
{
std:cout << it;
}

当然，在range-based for循环中有些缺陷：它不支持可变数组循环，而且，当要求更复杂的索引时，它也无法使用。但是，这类情形可以单独处理。我们会遇到这种情况：我们必须在一个反序遍历过程中移动某些满足条件的数据，标准库中没有提供额外的类来使用range-based for循环。下列代码展现如何实现：

template <typename T>
struct reversed_wrapper {

	const T& _v;
	
	reversed_wrapper (const T& v) : _v(v) {}
	
	auto begin() -> decltype(rbegin(_v))
	{
		return rbegin(_v);
	}
	
	auto end() -> decltype(rend(_v))
	{
		return rend(_v);
	}
};

template <typename T>
reversed_wrapper<T> reversed(const T& v)
{
	return reversed_wrapper<T>(v);
}

在C++14中，你可以移除decltype来简化代码。你可以看出auto如何帮你写泛型函数-reversed_wrapper，它支持数组和vector。

重构代码：
char buf[4] = {'a', 'b', 'c', 'd'};
for(auto it : reversed(buf)){
	std::cout << it;
}

这样写有什么好处？首先，较好的可读性。我们可以立即明白for循环是在反序遍历数组。第二，不易犯错。第三，适配任意类型。


你可以使用boost::adaptors::reverse(arr)。

让我们回顾下开始的示例。数组通过传入指针及大小，这样造成reversed无法正常工作。我们怎么办？使用span/array_view。在C++17中，可以使用string_view，代码可以写成下列形式：

void Foo(std::string_view s);
std::string str = "abc";
Foo(std::string_view("abc", 3));
Foo("abc");
Foo(str);

string_view仅封装了string对象的char*和长度。这也是为什么上述代码采用形参传值，而不用引用传值。string_view的一个重要特性是兼容多种字符串类型：const char*，std::string以及以非空格结尾的const char*。

代码示例：

inline void CXmlReader::CXmlInputStream::UnsafePutCharsBack(std::wstring_veiw chars)
{
for(wchar_t ch : reversed(chars))
	UnsafePutCharBack(ch);
}

我们必须记住：当使用这个函数时，string_view(const char*)构造器是隐式使用。这也是为什么我们可以写成如下形式：
Foo(pChars);
而不是
Foo(wstring_view(pChars, nNumChars));

string_view指向的string不需要以空字符结尾，string::data指向它，一定要记住这点。当将它的值从cstdlib(C语言风格字符串)传入函数，这是未定义行为。使用std::string或者非空字符结尾的字符串你可以很容易避免它。

## Enum

暂时不考虑C++，思考下C。如何定义安全的枚举值？对于多数的string类型，使用默认构造函数或者类型转换都不会出问题。实践中，错误经常发生在最简单的构造器中：检查和调试不易发现这些问题，程序员经常忘记检查构造器。下面列举一种危险的构造：

enum iscsi_param{
	...
	ISCSI_PARAM_CONN_PORT,
	ISCSI_PARAM_CONN_ADDRESS,
	...
};

enum iscsi_host_param{
	...
	ISCSI_HOST_PARAM_IPADDRESS,
	...
};

int iscsi_conn_get_addr_param(...,
	enum iscsi_param param, ...)
{
	...
	switch(param){
	case ISCSI_PARAM_CONN_ADDRESS:
	case ISCSI_HOST_PARAM_IPADDRESS:
	...
	}
	
	return len;
}

这段代码摘自Linux内核。
PVS-Studio警告：V556 The values of different enum types are compared: switch(ENUM_TYPE_A) { case ENUM_TYPE_B: ... }. libiscsi.c 3501


请注意switch-case中的枚举值，它们来自两个不同的枚举类型。在大段的代码中，这类型错误不易被发现。

在C++11中，应当使用enum类：如此错误将不能工作，编译阶段会出现错误提示。下面的代码无法通过编译：

enum class ISCSI_PARAM {
  ....
  CONN_PORT,
  CONN_ADDRESS,
  ....
};
 
enum class ISCSI_HOST {
  ....
  PARAM_IPADDRESS,
  ....
};
int iscsi_conn_get_addr_param(....,
 ISCSI_PARAM param, ....)
{
  ....
  switch (param) {
  case ISCSI_PARAM::CONN_ADDRESS:
  case ISCSI_HOST::PARAM_IPADDRESS:
  ....
  }
 
  return len;
}

下面的代码虽然未使用enum类型，但是也有同样的问题：

void adns__querysend_tcp(....) {
  ...
  if (!(errno == EAGAIN || EWOULDBLOCK || 
        errno == EINTR || errno == ENOSPC ||
        errno == ENOBUFS || errno == ENOMEM)) {
  ...
}

代码摘自ReactOS。

在C/C++中，使用宏定义是个坏方式，即便使用enum也不安全。

Initialization in the constructor


在C++中，多个构造器需要初始化相同的对象。例如：有一个类，它定义两个构造器，它们互相调用。我们一般这样做：把相同的代码封装成一个函数。这样是否就不会出现问题？

Guess::Guess()
{
	language_str = DEFAULT_LANGUAGE;
	country_str = DEFAULT_COUNTRY;
	encoding_str = DEFAULT_ENCODING;
}

Guess::Guess(const char * guess_str)
{
	Guess();
	...
}

注：这段代码摘自LibreOffice

PVS-Studio警告:V603 The object was created but it is not being used. If you wish to call constructor, 'this->Guess::Guess(....)' should be used. guess.cxx 56

但是，最好的解决方式是使用构造器代理，我们可以使用下面这种方式在一个构造器函数中调用另一个构造器。

Guess::Guess(const char * guess_str) : Guess()
{
...
}

这用方法有几种限制。首先，代理构造器将负责类的初始化。调用者不能初始化其他类型：
Guess:Guess(const char * guess_str)
: Guess(),
  m_member(42)
  {
  ...
  }

另外，我们也必须确保构造器不能循环调用。不幸地是，这样的代码可以通过编译：

Guess::Guess(const char * guess_str)
: Guess(str::string(guess_str))
{
...
}

Guess::Guess(std::string guess_str)
: Guess(guess_str.c_str())
{
...
}

## About virtual functions

虚函数存在一个潜在问题：继承类在重载虚函数时，定义一个新函数。看下面代码示例：

class Base{
	virtual void Foo(int x);
};

class Derived : public class Base{
	void Foo(int x, int a = 1);
};

Derived::Foo不能使用Base的指针或者引用调用。你可能说不会有人犯这类错误。查看如下代码：
class DBClientBase : ...
{
public:
	virtual auto_ptr<DBClientCursor> query(
		const string &ns,
		Query query,
		int nToReturn = 0,
		int nToSkip = 0,
		const BSONObj *fieldsToReturn = 0,
		int queryOptions = 0,
		int batchSize = 0);
};

class DBDirectClinet : public DBClientBase
{
public:
	virtual auto_ptr<DBClientCursor> query(
		const string &ns,
		Query query,
		int nToReturn = 0,
		int nToSkip = 0,
		const BSONObj *fieldsToReturn = 0,
		int queryOptions = 0);
};

PVS-Studio警告：V762 Consider inspecting virtual function arguments. See seventh argument of function 'query' in derived class 'DBDirectClient', and base class 'DBClientBase'. dbdirectclient.cpp 61


query函数有许多参数，程序员在重载虚函数时可能会遗漏某个参数或者默认值。

下列代码更具欺骗性。这个代码可以在32b系统编译通过，但是不能在64b系统编译。原因是，在基类中，参数的类型是DWORD_PTR，但是在继承类中，参数类型是DWORD。
class CWnd : public CCmdTarget
{
	...
	virtual void WinHelp(DWORD_PTR dwData, UINT nCmd = HELP_CONTEXT);
	...
};

class CFrameWnd : public CWnd {...};

class CFrameWndEx : public CFrameWnd {

	...
	virtual void WinHelp(DWORD dwData, UINT nCmd = HELP_CONTEXT);
	...
};

这类错误还可能发生在其他方面。可能忘记const函数或者参数修饰符；函数可能在基类中未添加virtual修饰符；类型可能是signed/unsigend.


在C++中，新引进override关键字帮助我们避免此类错误。 下列代码将不能编译：

class DBDirectClinet : public DBClientBase
{
public:
	virtual auto_ptr<DBClientCursor> query(
		const string &ns,
		Query query,
		int nToReturn = 0,
		int nToSkip = 0,
		const BSONObj *fieldsToReturn = 0,
		int queryOptions = 0) override;
};

## NULL vs nullptr

使用NULL可能使空指针变成一个数字0.因为NULL是一个int类型的0值定义的宏，这就不难理解为何下列代码中的第二个函数被选择：

void Foo(int x, int y, const char *name);
void Foo(int x, int y, int ResourceID);
Foo(1, 2, NULL);

上述代码虽然没有错误，但是不是我们想要实现的逻辑关系。针对此种情况，C++引入nullptr_t关键字。现代C++不建议使用NULL。

另一个例子：NULL可以与int类型的值比较。假设有个WinAPI函数返回HRESULT。HRESULT不是一个指针类型，因此，它与NULL做比较是无意义的。与nullptr比较编译会出现错误，但是与NULL比较却能编译。

if(WinApiFoo(a, b) != NULL) //That's bad
if(WinApiFoo(a, b) != nullptr) //Hooray, a compilation error

va_arg
项目中经常遇到无法准确知道函数参数具体数量。经典例子：标准输入/输出。当然，你可以采取一种方式避免使用可变参数，但是可变参数定义却更加方便易用。以前的C++采用什么方式呢？va_list。它会带来什么问题？它不仅容易传递一个错误类型的参数。而且也无法传递任意参数。参考下面示例：

typedef std::wstring string16;
const base::string16& relaunch_flags() const;

int RelaunchChrome(const DelegateExecuteOperation& operation)
{
	AtlTrace("Relaunching [%1s] with flags [%s] \n",
				operation.mutex().c_str(),
				operation.relaunch_flags());
	...
}

注：这段代码摘自Chromium

PVS-Studio 警告: V510 The 'AtlTrace' function is not expected to receive class-type variable as third actual argument. delegate_execute.cc 96

程序员想打印std::wstring字符串，但是忘记调用c_str()函数。wstring类型无法转换成wchar_t*类型。当然，函数不能正确计算。

cairo_status_t
_cairo_win32_print_gdi_error(const char *context)
{
...
fwprintf(stderr, L"%s:%S", context, (wchar_t *)lpMsgBuf);
...
}

注：这段代码摘自Cairo.

PVS-Studio 警告: V576 Incorrect format. Consider checking the third actual argument of the 'fwprintf' function. The pointer to string of wchar_t type symbols is expected. cairo-win32-surface.c 130

上述代码中，程序员意图格式化字符串。在VC++中，wchar_t*使用%s。char*使用%S.有趣的是，字符串中的这些错误会输出错误信息。所以它们不容易看到。

static void GetNameForFile(
	const char* baseFileName,
	const uint32 fileIdx,
	char outputName[512])
{
	assert(baseFileName != NULL);
	sprintf(outputName, "%s_%d", baseFileName, fileIdx);
}

注：这段代码摘自CryEngine 3 SDK


PVS-Studio 警告: V576 Incorrect format. Consider checking the fourth actual argument of the 'sprintf' function. The SIGNED integer type argument is expected. igame.h 66


整形也容易造成错误。尤其是当它们的大小与平台无关。最常见的是有符号类型和无符号类型。值很大的数可能被打印成负值。

ReadAnddumpLargeSttb(cb, err)
int cb:
int err:
{
...
printf("\n - %d strings were read, "
		"%d were expected (decimal numbers) - \n");
...
}

注：这段代码摘自word for windows 1.1a。


PVS-Studio 警告: V576 Incorrect format. A different number of actual arguments is expected while calling 'printf' function. Expected: 3. Present: 1. dini.c 498

考察下面的示例
BOOL CALLBACK EnumPickIconResourceProc(
	HMODULE hModule, LPCWSTR lpszType,
	LPWSTR lpszName, LONG_PTR lParam)
{
	...
	swprintf(szName, L"%u", lpszName);
	...
}

注：这段代码摘自ReactOS。


PVS-Studio 警告: V576 Incorrect format. Consider checking the third actual argument of the 'swprintf' function. To print the value of pointer the '%p' should be used. dialogs.cpp 66

在64b系统中，这段代码会出错。指针的大小由体系结构决定，使用%u是个不好的主意，我们应该改成什么？PVS工具给我们的建议是%p。如果调试时打印指针地址，这是个好主意。

使用可变参数的函数有什么问题？你不能检查参数类型，参数个数。

这里有更好的方式。首先，采用可变模板。利用它，我们能在编译时获取所有的类型信息。下面的代码更加安全：

void printf(const char* s)
{
	std::cout << s;
}

template<typename T, typename... Args>
void printf(const char* s, T value, Args... args){
	while(s && *s){
		if(*s == '%' && *++s != '%'){
			std::cout << value;
			return printf(++s, args...);
		}
		std::cout << *s++;
	}
}

实践中，它的使用时无指针的。但是在可变模板示例中，只要你能想到都能实现。

一个更结构化的方式是使用可变数量参数类型操作std::initializer_list。它不允许传入不同类型的参数。如果可能，尽量使用std::initializer_list。

void Foo(std::initializer_list<int> a);
Foo({1, 2, 3, 4, 5});

它非常便利，跟begin、end及range for类似。

## Narrowing

在编程过程中，收缩(精度降低,范围变小...)带来许多问题。尤其是64b系统。采用正确的类型是很好的实践原则。但是，程序员经常碰见类型转换。如下代码：
char* ptr = ...;
int n = (int)ptr;
...
ptr = (char*)n;

下述代码有两个整数类型，程序员计算两个数的比率：
virtual int GetMappingWidth() = 0;
virtual int GetMappingHeight() = 0;

void CDetailObjectSystem::LevelInitPreEntity()
{
	...
	float flRatio = pMat->GetMappingWidth() / 
					pMat->GetMappingHeight();
	...
}

注：这段代码摘自Source Engine SDK.

PVS-Studio 警告: V636 The expression was implicitly cast from 'int' type to 'float' type. Consider utilizing an explicit type cast to avoid the loss of a fractional part. An example: double A = (double)(X) / Y;. Client (HL2) detailobjectsystem.cpp 1480


不幸地是，上述代码可能发生错误。有多种方式可以避免类型的默认转换。但是，在C++11中，最好的方式是使用初始化方法，它会阻止默认转换。在下列代码中，在编译阶段会给出错误信息。

float flRatio{pMat->GetMappingWidth() / pMat->GetMappingHeight()};

## No news is good news
在程序中，大量的错误发生在资源和内存管理。便捷易用是对现代程序语言最重要的设计要求。现代C++也遵循这个设计原则，提供了大量的工具来自动控制资源。

void AccessibleContainsAccessible(...)
{
	auto_ptr<VARIANT> child_array(new VARIANT[child_count]);
}

注：这段代码摘自Chromium

PVS-Studio 警告: V554 Incorrect use of auto_ptr. The memory allocated with 'new []' will be cleaned using 'delete'. interactive_ui_tests accessibility_win_browsertest.cc 171

当然，智能指针不是新概念，例如，std::auto_ptr类是一个智能指针。但是，我们并不使用auto_ptr，在C++17中已经被移除。上述代码错误地使用auto_ptr，这个类不能在数组中使用，它采用delete释放资源而不是delete []。unique_ptr用来替代auto_ptr，它提供特定的方式管理数组。通过传递deleter仿函数代替delete。

void text_editor::_m_draw_string(...) const
{
	...
	std::unique_ptr<unsigned> pxbuf_ptr(new unisgned[len]);
	...
}

注：这段代码摘自nana。

PVS-Studio 警告: V554 Incorrect use of unique_ptr. The memory allocated with 'new []' will be cleaned using 'delete'. text_editor.cpp 3137

上述代码存在同样的错误。它应该被改写成unique_ptr<unsigned[]>。
现代C++中使用std::vector或许更好？查看下列代码：
template<class TOpenGLStage>

static FString GetShaderStageSource(TOpenGLStage* Shader)
{
	...
	ANSICHAR* Code = new ANSICHAR[Len +  1];
	glGetShaderSource(Shaders[i], Len + 1, &Len, Code);
	Source += Code;
	delete Code;
	...
}


注：这段代码摘自Unreal Engine 4.

PVS-Studio 警告: V611 The memory was allocated using 'new T[]' operator but was released using the 'delete' operator. Consider inspecting this code. It's probably better to use 'delete [] Code;'. openglshaders.cpp 1790

不使用智能指针，上述错误很容易发生。采用new[]分配内存，却用delete释放内存。

bool CxImage::LayerCreate(int32_t position)
{
	...
	CxImage** ptmp = new CxImage*[info.nNumLayers + 1];
	...
	free(ptmp);
	...
}

注：这段代码摘自CxImage。



PVS-Studio 警告: V611 The memory was allocated using 'new' operator but was released using the 'free' function. Consider inspecting operation logics behind the 'ptmp' variable. ximalyr.cpp 50

上述代码将malloc/free和new/delete混淆。

int settings_proc_language_packs(...)
{
	...
	if(mem_files)
	{
		mem_files = 0;
		sys_mem_free(mem_files);
	}
	...
}

注：这段代码摘自Fennec Media.

PVS-Studio 警告: V575 The null pointer is passed into 'free' function. Inspect the first argument. settings interface.c 3096

这里有个有趣的示例。一个指针被释放并且赋值为0。有时，程序员定义一个特殊的宏。虽然，这是个很好的技术：它能防止在其他地方释放内存。但是，这个表达式容易造成困惑，free函数传入null指针。

ETOOLS_API int __stdcall ogg_enc(...)
{
	format = open_audio_file(in, &enc_opts);
	if(!format)
	{
		fclose(in);
		return 0;
	}
	out = fopen(out_fn, "wb");
	if(out == NULL)
	{
		fclose(out);
		return 0;
	}
}

这个问题不仅与内存管理有关，也与资源管理有关。例如，忘记关闭文件。RAII(‘智能指针’)管理内存帮助我们避免许多内存泄漏问题。

auto deleter = [](FILE* f) {fclose(f);};

std::unique_ptr<FILE, decltype(deleter)> p(fopen("1.txt", "w"), deleter);


## What is the result?
现代C++提供大量工具来帮助我们写出更加安全的代码。许多编译时的分析和检查已经出现。你可以更方便地切换内存和资源管理模型。

但是没有任何技术能保证你不犯错。灵活采用各种技术及工具，可以帮助我们写出高质量代码，节省时间和精力。

推荐使用：PVS-Studio(https://www.viva64.com/en/pvs-studio/)，提供
Linux版本和Windows VS的插件。

