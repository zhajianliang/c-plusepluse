```c++
//普通职工文件
#pragma once
#include<iostream>
#include"worker.h"
class Employee :public Worker
{
public:
	Employee(int ID,string name,int DID);
	virtual void showINfo();
	virtual string getDepname();
};
```

