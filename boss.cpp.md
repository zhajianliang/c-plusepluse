```c++
#include"boss.h"
Boss::Boss(int ID, string name, int DID)
{
	this->m_ID = ID;
	this->m_name = name;
	this->detID = DID;
}
void Boss::showINfo()
{
	cout << "职工编号：" << this->m_ID << "\t";
	cout << "职工姓名：" << this->m_name << "\t";
	cout << "职位：" << this->getDepname() << "\t";
	cout << "岗位职责：下达给经理任务" << endl;
}
string Boss::getDepname()
{
	return string("老板");
}
```

