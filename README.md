#include<iostream>
#include<string>
#include <vector>
#include<cstring>
#include<map>
#include <typeinfo>
using namespace std;

string num[11] = {"零","一","二","三","四","五","六","七","八","九","十"};

//中文数字转化阿拉伯数字
int getNum(string a){
	int number;
	if(!a.compare(num[0]))	number = 0;
	if(!a.compare(num[1]))	number = 1;
	if(!a.compare(num[2]))	number = 2;
	if(!a.compare(num[3]))	number = 3;
	if(!a.compare(num[4]))	number = 4;
	if(!a.compare(num[5]))	number = 5;
	if(!a.compare(num[6]))	number = 6;
	if(!a.compare(num[7]))	number = 7;
	if(!a.compare(num[8]))	number = 8;
	if(!a.compare(num[9]))	number = 9;
	if(!a.compare(num[10]))	number = 10;
	return number;
} 

// 以空格拆分字符串string 
void SplitSpace(string ent,vector<string> &entV){
	size_t start = 0,index = ent.find_first_of(' ',0);
    while(index != ent.npos)
    {
        if(start != index)
            entV.push_back(ent.substr(start,index-start));
        start = index+1;
        index = ent.find_first_of(' ',start);
    }
	if(!ent.substr(start).empty())
        entV.push_back(ent.substr(start));
}

//增加 减少 等于计算 
void getCalculatNumber(map<string,int> &varibale,string calculatSign,int number,string name){
	if(!calculatSign.compare("增加"))	varibale[name] = varibale[name] + number;
	if(!calculatSign.compare("减少"))	varibale[name] = varibale[name] - number;
	if(!calculatSign.compare("等于"))	varibale[name] = number;
}

bool judge(map<string,int> &varibale,string calculatSign,int number,string name){
	bool flag = false;
	if(!calculatSign.compare("大于"))
		if(varibale[name] > number)    flag = true;
	if(!calculatSign.compare("小于"))
		if(varibale[name] < number)    flag = true; 
	if(!calculatSign.compare("等于"))
		if(varibale[name] == number)   flag = true;
	return flag;
}

//输出
void output(map<string,int> &varibale,string name){
	int number = varibale[name];
	if(number < 0){
		cout << "负";
		number = -number; 
		}
	switch(number){
		case 0: cout << "零" << endl; break;
		case 1: cout << "一" << endl; break; 
		case 2: cout << "二" << endl; break; 
		case 3: cout << "三" << endl; break; 
		case 4: cout << "四" << endl; break; 
		case 5: cout << "五" << endl; break; 
		case 6: cout << "六" << endl; break; 
		case 7: cout << "七" << endl; break; 
		case 8: cout << "八" << endl; break; 
		case 9: cout << "九" << endl; break; 
		case 10: cout << "十" << endl; break; 
	}
} 

int main(){
	
	string ent;
	map<string,int> varibale;
	while(getline(cin,ent)){
		vector<string> entV;
    	SplitSpace(ent,entV);
//    	for (int i=0;i<entV.size();i++)
//        	cout<<entV.at(i)<<endl;
//        cout << entV.size() << endl;
		
		if(!entV.at(0).compare("整数")){
			//第一个为“整数”，则是储存变量 
			varibale.insert(map<string,int>::value_type(entV.at(1),0));
			int number = getNum(entV.at(3));
			getCalculatNumber(varibale,entV.at(2),number,entV.at(1));
		}
		else if(!entV.at(0).compare("看看")){
			//第一个为“看看”，则是输出变量 
			output(varibale,entV.at(1));
		}else if(!entV.at(0).compare("如果")){
			//第一个为“如果”，则是判断操作 
			int number = getNum(entV.at(3));
			if(judge(varibale,entV.at(2),number,entV.at(1))){
				if(!entV.at(5).compare("看看")){
					//判断看看后面是要求输出的字符串，还是变量名 
					string ch = entV.at(6).substr(0, 2);
					if(!ch.compare("“")){
						string str = entV.at(6).substr(2, entV.at(6).length()-3);
						cout << str << endl;
					}else{
						output(varibale,entV.at(6));
					} 
				}if(!entV.at(5).compare("无")){
					//跳过，不做操作 
				}else{
					//若则后面为改变操作，如 气温 增加 二 
					int number = getNum(entV.at(7));
					getCalculatNumber(varibale,entV.at(6),number,entV.at(5));
				} 
			}else{
				//从后往前找否则的位置 
				int xx;
				for(int i = entV.size() - 1; i >= 0; i--){
					if(!entV.at(i).compare("否则")){
						xx = i;
						break;
					}
				}
				if(!entV.at(xx + 1).compare("看看")){
					//判断看看后面是要求输出的字符串，还是变量名
					string ch = entV.at(xx + 2).substr(0, 2);
					if(!ch.compare("“")){
						string str = entV.at(xx + 2).substr(2, entV.at(xx + 2).length()-3);
						cout << str << endl;
					}else{
						output(varibale,entV.at(xx + 2));
					}
				}else if(!entV.at(xx + 1).compare("无")){
					//跳过，不做操作 
				}else{
					//若则后面为改变操作，如 气温 增加 二 
					int number = getNum(entV.at(xx + 3));
					getCalculatNumber(varibale,entV.at(xx + 2),number,entV.at(xx + 1));
				} 
			}
		}else{
			//第一个为变量名，则为修改参数 
			int number = getNum(entV.at(2));
			getCalculatNumber(varibale,entV.at(1),number,entV.at(0));
		}
	}
	return 0;
} 
