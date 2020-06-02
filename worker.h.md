```c++
#pragma once
#include<iostream>
#include<string>
using namespace std;
//职工的抽象类
class Worker
{
public:
	virtual void showINfo() = 0;//显示个人信息
	virtual string getDepname() = 0;//获取岗位名称
	int m_ID;
	string m_name;
	int detID;
};
```

