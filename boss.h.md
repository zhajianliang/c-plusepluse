```c++
#pragma once
#include<iostream>
#include"worker.h"
using namespace std;
class Boss :public Worker
{
public:
	Boss(int ID, string name, int DID);
	virtual void showINfo();
	virtual string getDepname();
};
```

