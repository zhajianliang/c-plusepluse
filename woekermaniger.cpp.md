```c++
#include"workManiger.h"
workManiger::workManiger()//职工管理类
{
	ifstream ifs;
	ifs.open(FILENAME, ios::in);
	if (!ifs.is_open())
	{
		cout << "文件不存在" << endl;
		this->Empnum = 0;
		this->m_EmpArry = NULL;//初始化记录人数
		this->m_Fileisempty = true;//初始化数组指针为空
		ifs.close();
		return;

	}
	char ch;
	ifs>> ch;
	if (ifs.eof())
	{
		cout << "文件为空" << endl;
		this->Empnum = 0;
		this->m_EmpArry = NULL;
		this->m_Fileisempty = true;
		ifs.close();
		return;
	}
	//当文件存在，且记录了数据
	int num = this->get_filenum();
	//cout << "职工人数为：" << num << endl;
	this->Empnum = num;
	this->m_EmpArry = new Worker*[this->Empnum];
	this->init_num();
	//测试代码
	/*for (int i = 0; i < this->Empnum; i++)
	{
		cout << "职工编号" << this->m_EmpArry[i]->m_ID
			<< "  姓名" << this->m_EmpArry[i]->m_name
			<< "  部门编号" << this->m_EmpArry[i]->detID << endl;
	}*/
	/*this->Empnum = 0;
	this->m_EmpArry = NULL;
*/
}
void workManiger::Add_num()
{
	cout << "请输入添加职工的数量" << endl;
	int addnum = 0;
	cin >> addnum;//保存用户输入的数量
	if (addnum > 0)
	{
		//计算新添加空间的大小
		int newSize = this->Empnum + addnum;
		//开辟新空间
		Worker **newspace = new Worker *[newSize];
		//将原来空间下数据，拷贝到新空间下
		if (this->m_EmpArry != NULL)
		{
			for (int i = 0; i < this->Empnum; i++)
			{
				newspace[i] = this->m_EmpArry[i];
			}
		}
		//批量添加新数据
		for (int i = 0; i <addnum; i++)
		{
			int ID;
			string name;
			int dselect;
			cout << "请输入第 " << i + 1 << "个新职工编号" << endl;
			cin >> ID;
			cout << "请输入第 " << i + 1 << "个新职工姓名" << endl;
			cin >> name;
			cout << "请选择该职工岗位："<< endl;
			cout << "1、普通职工" << endl;
			cout << "2、经理" << endl;
			cout << "3、老板" << endl;
			cin >> dselect;
			Worker *work = NULL;
			switch (dselect)
			{
			case 1:
				work = new Employee(ID, name, 1);
				break;
			case 2:
				work = new Manager(ID, name, 2);
				break;
			case 3:
				work = new Boss(ID, name, 3);
				break;
			default:
				break;
			}
			//将创建的职工职责，保存到数组中
			newspace[this->Empnum + i] = work;

		}
		//先释放原有空间
		delete[] this->m_EmpArry;
		//更改新空间的指向
		this->m_EmpArry = newspace;
		//更新新的职工人数
		this->Empnum = newSize;
		//更新职工不为空的情况
		this->m_Fileisempty = false;
		//提示添加成功
		cout << "添加成功" << addnum << "名新职工" << endl;
		
		this->save();

	}
	else
	{
		cout << "输入的数据有误" << endl;
	}
	system("pause");
	system("cls");
}
void workManiger::Showmune()
{
	cout << "************************************************" << endl;
	cout << "*************欢迎使用职工管理系统*************" << endl;
	cout << "*************0、退出管理系统*************" << endl;
	cout << "*************1、增加职工信息*************" << endl;
	cout << "*************2、显示职工信息*************" << endl;
	cout << "*************3、删除职工信息*************" << endl;
	cout << "*************4、修改职工信息*************" << endl;
	cout << "*************5、查找职工信息*************" << endl;
	cout << "*************6、按编号排序*************" << endl;
	cout << "*************7、清空职工信息*************" << endl;
	cout << "************************************************" << endl;
}
void workManiger::exitSystem()
{
	cout << "欢迎下次使用" << endl;
	system("pause");
	exit(0);//不管在那行代码，执行到这里都退出程序
}

void workManiger::save()
{
	ofstream ofs;
	ofs.open(FILENAME,ios::out);
	for (int i = 0; i < this->Empnum; i++)
	{
		ofs <<this->m_EmpArry[i]->m_ID << " "
			<< this->m_EmpArry[i]->m_name << " "
			<<this->m_EmpArry[i]->detID << endl;
	}
	ofs.close();

}
//添加一个成员函数，统计文件中的人数
int workManiger::get_filenum()
{
	ifstream ifs;
	ifs.open(FILENAME, ios::in);//读的方式打开文件
	int id;
	string name;
	int did;
	int num = 0;
	while (ifs>>id&&ifs>>name&&ifs>>did)
	{
		//统计人数
		num++;
	}

	return num;
}
//初始化员工
void workManiger::init_num()
{
	ifstream ifs;
	ifs.open(FILENAME, ios::in);//读文件
	int id;
	string name;
	int did;
	int index = 0;
	while (ifs >> id && ifs >> name && ifs >> did)
	{
		Worker*worker = NULL;
		if (did == 1)//普通员工
		{
			worker = new Employee(id, name, did);
		}
		else if(did==2)//经理
		{
			worker = new  Manager(id, name, did);
		}
		else//老板
		{
			worker = new Boss(id, name, did);
		}
		this->m_EmpArry[index] = worker;
		index++;
	}
	ifs.close();
}

//显示职工
void workManiger::show_emp()
{
	if (this->m_Fileisempty)
	{
		cout << "当前职工为空" << endl;
	}
	else
	{
		for (int i = 0; i < this->Empnum; i++)
		{
			//利用多态调用程序接口
			this->m_EmpArry[i]->showINfo();
		}

	}
	system("pause");
	system("cls");
}
//删除职工
void workManiger::del_emp()
{
	if (this->m_Fileisempty)
	{
		cout << "当前没有任何职工" << endl;
	}
	else
	{
		int ID = 0;
		cout << "请输入要删除职工的编号" << endl;
		cin >> ID;
		int index=empisexit(ID);
		if (index != -1)//证明存在且要删除改职工
		{
			for (int i = index; i < this->Empnum-1; i++)
			{
				this->m_EmpArry[i] = this->m_EmpArry[i + 1];
			}
			this->Empnum--;
			//数据同步更新到文件中
			this->save();
			cout << "删除成功" << endl;
		}
		else
		{
			cout << "删除失败未找到该职工" << endl;
		}
		
	}
	system("pause");
	system("cls");
}
//判断职工是否存在
int workManiger::empisexit(int id)//如果存在，则返回职工所在数组中的位置
{
	int index = -1;
	for (int i = 0; i < this->Empnum; i++)
	{
		if (this->m_EmpArry[i]->m_ID == id)
		{
			index = i;
			break;
		}
	}
	return index;
}
//修改职工
void workManiger::changeemp()
{
	if (this->m_Fileisempty)
	{
		cout << "文件为空" << endl;
	}
	else
	{
		cout << "请输入要修改职工的编号" << endl;
		int id;
		cin >> id;
		int ret=this->empisexit(id);
		if (ret != -1)
		{
			//查找到这个职工
			delete this->m_EmpArry[ret];
			int newid = 0;
			string newname = "";
			int newdselect = 0;
			cout << "查找到" << id << "号职工，请输入新的职工的号：" << endl;
			cin >> newid;
			cout << "请输入新的姓名：" << endl;
			cin >> newname;
			cout << "请输入新的岗位：" << endl;
			cout << "1.普通职工" << endl;
			cout << "2.经理" << endl;
			cout << "3.老板" << endl;
			cin >> newdselect;
			Worker*worker = NULL;
			switch (newdselect)
			{
			case 1:
				worker = new Employee(newid, newname, newdselect);
				break;
			case 2:
				worker = new Manager(newid, newname, newdselect);
				break;
			case 3:
				worker = new Boss(newid, newname, newdselect);
				break;
			default:
				break;
			}
			//更新数据到数组中
			this->m_EmpArry[ret] = worker;
			cout << "修改成功" << endl;
			//保存到文件中
			this->save();
		}
		else
		{
			cout<<"查找失败"<<endl;
		}
	}
	system("pause");
	system("cls");
}
//查找职工
void workManiger::find_emp()
{
	if (this->m_Fileisempty)
	{
		cout << "文件为空或不存在" << endl;
	}
	else
	{
		cout << "请输入查找的方式："<< endl;
		cout << "1.按职工编号进行查找" << endl;
		cout << "2.按职工姓名进行查找" << endl;
		int select = 0;
		cin >> select;
		if (select == 1)
		{
			int id;
			cout << "请输入查找职工的编号" << endl;
			cin >> id;
			int ret = this->empisexit(id);
			if (ret != -1)
			{
				cout << "查找成功,该职工的信息如下：" << endl;
				this->m_EmpArry[ret]->showINfo();
			}
			else
			{
				cout << "查找失败,查无此人" << endl;
			}
		}
		else if(select==2)
		{
			string name;
			//加入是否查找到的标志
			//bool flag = false;
			cout << "请输入要查找的姓名" << endl;
			cin >> name;
			for (int i = 0; i < this->Empnum; i++)
			{
				if (name == this->m_EmpArry[i]->m_name)
				{
					cout << "查找成功，该职工编号为"
						<< this->m_EmpArry[i]->m_ID
						<< "的职工信息如下：" << endl;
					//flag = true;
					//调用显示信息的接口
					this->m_EmpArry[i]->showINfo();
				}
				else
				{
					cout << "查找失败,查无此人" << endl;
				}
			}
			/*if (flag == false)
			{
				cout << "查找失败" << endl;
			}*/


		}
		else
		{
			cout << "输入有误" << endl;
		}
	}
	system("pause");
	system("cls");
}

//按照编号进行排序
void workManiger::sort_emp()
{
	if (this->m_Fileisempty)
	{
		cout << "文件不存在或为空" << endl;
		system("pause");
		system("cls");
	}
	else
	{
		cout << "请选择排序的方式" << endl;
		cout << "1.升序的方式" << endl;
		cout << "2.降序的方式" << endl;
		int select=0;
		cin >> select;
		for (int i = 0; i < this->Empnum; i++)
		{
			int minormax = i;//声明最小值或最大值
			for (int j = i + 1; j < this->Empnum; j++)
			{
				if (select == 1)//升序
				{
					if (this->m_EmpArry[i]->m_ID > this->m_EmpArry[j]->m_ID)
					{
						minormax = j;
					}
				}
				else//降序
				{
					if (this->m_EmpArry[i]->m_ID < this->m_EmpArry[j]->m_ID);
					minormax = j;
				}

			}
			//判断一开始认定的最小值或最大值，是不是计算中的最大值最小值，如果不是则交换
			if (i!=minormax)
			{
				Worker*temp =this->m_EmpArry[i];
				this->m_EmpArry[i] = this->m_EmpArry[minormax];
				this->m_EmpArry[minormax] = temp;
			}
			
		}
	}
	cout << "排序成功,排序后的结果为：" << endl;
	this->save();//将数据保存到文件中
	this->show_emp();//显示所有职工
}
// 清空职工
void workManiger::clean_file()
{
	cout << "确定清空吗？" << endl;
	cout << "1.确定"<<endl;
	cout << "2.不确定" << endl;
	int select = 0;
	cin >> select;
	if (select == 1)
	{
		ofstream ofs(FILENAME, ios::trunc);//清空文件后重新创建
	
		ofs.close();
		if (this->m_EmpArry != NULL)//删除堆区的每个职工
		{
			for (int i = 0; i < this->Empnum; i++)
			{
				if (this->m_EmpArry[i] != NULL)
				{
					delete this->m_EmpArry[i];
					this->m_EmpArry[i] = NULL;
				}

			}
			//删除堆区数组指针
			delete[] this->m_EmpArry;
			this->m_EmpArry = NULL;
			this->Empnum = 0;
			this->m_Fileisempty = true;
		}
		cout << "清空成功" << endl;
		system("pause");
		system("cls");
	}
	else
	{
		system("pause");
		system("cls");
	}
}
workManiger::~workManiger()
{
	if (this->m_EmpArry != NULL)//删除堆区的每个职工
	{
		for (int i = 0; i < this->Empnum; i++)
		{
			if (this->m_EmpArry[i] != NULL)
			{
				delete this->m_EmpArry[i];
				this->m_EmpArry[i] = NULL;
			}

		}
		//删除堆区数组指针
		delete[] this->m_EmpArry;
		this->m_EmpArry = NULL;
		
	}
}

```

