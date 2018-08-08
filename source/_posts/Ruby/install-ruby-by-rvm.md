---
title: ʹ��RVM��װ����Ruby
date: 2018-08-07 23:36:00
tags: [Ruby]
---

> RVM(Ruby Version Manager)��һ������װ������ͬ�汾��Ruby�����������й��ߡ�

### ��װRVM

- ȷ���Ƿ��Ѱ�װ`curl`��û����ִ������`yum install curl`���а�װ��
- ��[RVM����](https://www.rvm.io/)��ʹ�������ṩ�����װRVM��
```shell
$ gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
$ \curl -sSL https://get.rvm.io | bash -s stable
```
- ʹ�����ļ���Ч��
  - ���������ļ�λ�ã� `find / -name rvm.sh`
  - �����ļ���Ч�� `source /etc/profile.d/rvm.sh`

- ����RVM������ `rvm requirements`



### ��װRuby
- �鿴RVM������Ruby�汾�� `rvm list known`
- ָ����װ����ĳ���汾��Ruby ��`rvm install ruby-2.5.1`

### ��������
- �鿴�����Ѱ�װ��Ruby�汾��Ϣ��`rvm list`
- ����Ruby�ĵ�ǰ��Ҫ�汾�� `rvm use 2.5.1`
- ����ΪĬ�ϰ汾��`rvm use 2.5.1 default`
- ɾ������ĳ���汾��`rvm remove 2.5.1`

