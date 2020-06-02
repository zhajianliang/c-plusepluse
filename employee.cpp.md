```c++
#include"employee.h"
Employee::Employee(int ID,string name,int DID)
{
	this->m_ID = ID;
	this->m_name = name;
	this->detID = DID;
}
void Employee::showINfo()
{
	cout << "职工编号：" << this->m_ID << "\t";
	cout << "职工姓名：" << this->m_name << "\t";
	cout << "岗位：" << this->getDepname() <<"\t";
	cout << "岗位职责：完成经理交给的任务" << endl;
}
string Employee::getDepname()
{
	return string("员工");
}
```

