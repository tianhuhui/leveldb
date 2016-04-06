1. ���Ի�����Win10 + WMware Workstation 11 + CentOS 7.
2. ���Բ���

2.1 ����Դ��
��leveldbĿ¼��ִ��make���ִ�гɹ��󣬻���leveldbĿ¼��������Ŀ¼��out-shared��out-static��libleveldb.a�ļ���out-staticĿ¼�С�
![make](https://github.com/tianhuhui/leveldb/blob/branch_test_/images_about/make.png)

2.2 ��д�����ļ�tes.cpp
```C++
#include <assert.h>
#include <string.h>
#include <leveldb/db.h>
#include <iostream>

int main(){
	leveldb::DB* db;
	leveldb::Options options;
	options.create_if_missing = true;
	leveldb::Status status = leveldb::DB::Open(options,"/tmp/testdb", &db);
	assert(status.ok());

	//write key1,value1
	std::string key="key";
	std::string value = "value";

	status = db->Put(leveldb::WriteOptions(), key,value);
	assert(status.ok());

	status = db->Get(leveldb::ReadOptions(), key, &value);
	assert(status.ok());
	std::cout<<value<<std::endl;
	std::string key2 = "key2";
    
	//move the value under key to key2
	
	status = db->Put(leveldb::WriteOptions(),key2,value);
	assert(status.ok());
	status = db->Delete(leveldb::WriteOptions(), key);

	assert(status.ok());
	
	status = db->Get(leveldb::ReadOptions(),key2, &value);
	
	assert(status.ok());
	std::cout<<key2<<"==="<<value<<std::endl;
	
	status = db->Get(leveldb::ReadOptions(),key, &value);
	
	if(!status.ok()) std::cerr<<key<<"  "<<status.ToString()<<std::endl;
	else std::cout<<key<<"==="<<value<<std::endl;
	
	delete db;
	return 0;
}
```

2.3 ��������
ע��libleveldb.a ��leveldb include��·����
> g++ -o test test.cpp out-static/libleveldb.a -lpthread -I include/

![g++](https://github.com/tianhuhui/leveldb/blob/branch_test_/images_about/g%2B%2B.png)

2.4 ���У�./test
![test](https://github.com/tianhuhui/leveldb/blob/branch_test_/images_about/test.png)

