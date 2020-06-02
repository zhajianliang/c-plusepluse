```c++
#pragma once;
#include<iostream>
using namespace std;
#include"worker.h"
#include"employee.h"
#include"manager.h"
#include"boss.h"
#include<fstream>
#define FILENAME "empfile.txt"
class workManiger
{
public:
	workManiger();//构造函数
	void Showmune();//显示菜单
	void exitSystem();//退出系统
	//记录职工人数
	int Empnum;
	//职工数组的指针
	Worker **m_EmpArry;
	void Add_num();//添加职工
	void save();//保存文件
	bool m_Fileisempty;//判断文件是否为空
	//添加一个成员函数，统计文件中的人数
	int get_filenum();
	//初始化员工
	void init_num();
	//显示职工
	void show_emp();
	//删除职工
	void del_emp();
	
	//判断职工是否存在
	int empisexit(int id);//如果存在，则返回职工所在数组中的位置

	//修改职工
	void changeemp();
	//查找职工
	void find_emp();

	//按照编号进行排序
	void sort_emp();

	//清空职工
	void clean_file();
	
	~workManiger();//析构函数

};
```

