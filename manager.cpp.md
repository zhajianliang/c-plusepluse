```c++
#include"manager.h"
Manager::Manager(int ID,string name,int DID)
{
	this->m_ID = ID;
	this->m_name = name;
	this->detID = DID;
}
void Manager::showINfo()
{
	cout << "职工编号：" << this->m_ID << "\t";
	cout << "职工姓名：" << this->m_name << "\t";
	cout << "职位：" << this->getDepname() << "\t";
	cout << "岗位职责：完成老板布置的任务" << endl;
}
string Manager::getDepname()
{
	return string("经理");
}
```

