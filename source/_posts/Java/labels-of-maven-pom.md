---
title: Maven POM�ļ����ñ�ǩ����
date: 2016-11-10 23:43:00
tags: [Java,Maven]
---

> ���ĵ�������POM�ļ�����õ�һ���ֱ�ǩ�������ı�ǩ˵�������[�ٷ��ĵ�](http://maven.apache.org/ref/3.2.5/maven-model/maven.html)��
> ���⣬��Ŀ���������������ȼ���local > setting-profiles > pom-repositories > setting-mirror/central

### ������ǩ
| Label | Description |
| :--: | ---- |
| modelVersion | ������ʹ��POMģ�͵İ汾 |
| parent | ��Ŀ�ĸ�ģ�飬��Ҫ��д��ģ���`groupId`��`artifactId`��`version`������`relativePath`�����ø�ģ���λ�ã����ú�����Ȳ��Ҹ�Ŀ¼����Maven�ֿ� |
| groupId | ģ��������group��Ψһid������`parent`����Բ���`groupId`����Ĭ��ʹ��`parent`��`groupId` |
| artifactId | ��ģ���ΨһID |
| version | artifact�İ汾�� |
| packaging | ��ģ�������ͣ���ѡ`jar`��`pom`��`war`��`ear`��Ĭ��Ϊ`jar` |
| name/url/description | Ϊ������Ŀ�����õı�ǩ���Ǳ��� |

#### - modules
�г�����Ŀ����ģ�飬ʹ�����·����

#### - repositories
| Label | Description |
| ---- | ---- |
| releases | �����Ƿ�����release�İ� |
| snapshots | �����Ƿ�����snapshot�İ���һ�㹫���ֿⶼ����Ϊ`false` |
| id | �ֿ�ID����Ҫ��setting�ļ��е�`server`��ID������ͬ |
| name | �ֿ����� |
| url | �ֿ�ĵ�ַ |
| layout | Maven1��`layout`��`legacy`��Ĭ����`default` |

####  - pluginRepositories
���û�����Զ��һ�������ǲ��Ҳ��ʱ�ᵽ����Ĳ���ֿ��ն����ǵ�repositories���õĲֿ����ҡ�

#### - distributionManagement
ͨ��deploy����ѹ����İ��ַ���Զ�ֿ̲⡣
| Label | Description |
| ---- | ---- |
| snapshotRepository | ���ղֿ� |
| repository | release�ֿ� |
| id/name/url | �ֿ������ |

���⣬һ�㻹��Ҫ��setting�ļ����ö�Ӧid��server��

```xml
<servers>
    <server>
        <id>repo1</id>
        <username>repouser</username>
        <password>my_login_password</password>
        <!-- To encrypt passwords: http://maven.apache.org/guides/mini/guide-encryption.html -->
    </server>
</servers>
```

### - dependencyManagement
���������������İ汾����Ϣ������ǩ�µ�dependencyֻ������Ϣ����ֱ�Ӽ�������������POM�ļ����߼̳е�POM�ļ��е�dependencies��ǩ�е�dependencyָֻ����groupId��artifactId��û������version��scope����Ϣ���ͻ�Ĭ�ϴ�dependencyManagement�в��������Ϣ��

### - dependencies
���������Ŀ�����������
| Label | Description |
| ---- | ---- |
| groupId | ����������group��ID |
| artifactId | ��������ID |
| version | �������İ汾�� |
| groupId | ����������group��ID |
| exclusions | ���ô������ų�������ȥ������������������������ |
| classifier | ����ָ���������ж�������ʱӦ��ʹ����һ�������ж���ɲ�ͬjdk��������İ汾ʱ��Ҫָ��ʹ����һ���汾 |
| systemPath | ���������ڱ��ض��ǲֿ�ʱ��`sytemPath`ָ����jar����·�� |
| type | ָ���������İ������ͣ�Ĭ��Ϊjar |
| optional | ��ʾ������Ŀ��������Ŀʱ���Ƿ񴫵ݸ�������`true`��ʾ�����ݣ�Ĭ��Ϊ`false` |
| scope | ���ʱ���������Ĳ�ͬ����ģʽ |

������scope�ļ���ģʽ��
- compile: Ĭ��ֵ����ʾ��ǰ������Ҫ���뵽��Ŀ�ı��롢���ԡ����У����ʱ�ᱻinclude��ȥ��
- test: ��ʾ��ǰ������������ԵĹ������������Դ���ı����ִ�У����ʱ���ᱻinclude��ȥ��
- runtime: ��ʾ��ǰ������������Ŀ�ı��룬����������Ĳ��Ժ�ִ�У����ʱ�ᱻinclude��ȥ��
- provided: �൱��`compile`�������ʱ����include����������ʹ������ҪҲ�����ⲿ�����ṩ��
- system: �൱��`provided`�����Ǹ������ɱ����ļ�ϵͳ�ṩ����Ҫ���`systemPath`��������·����

### - plugins
Maven��������һ�������ܣ����ĺ��Ĳ���ִ���κξ���Ĺ�������������Щ���񶼽����������ɡ�ͨ��plugins�����ÿ������һЩ����Ĺ������񣬻����޸Ĺ���������ʹ�õĲ���İ汾��

### - profiles
ָ�����ڲ�ͬ���������Ĺ�����ʹ�õĸ��ֲ�����ֵ��

### - build
ָ�����������еĸ����Զ��幤����
| Label | Description |
| ---- | ---- |
| sourceDirectory | Java�ļ�Ŀ¼ |
| testSourceDirectory | ����Java�ļ�Ŀ¼ |
| resources | ��Դ�ļ�Ŀ¼ |
| testResources | ������Դ�ļ� |
| outputDirectory | Դ�ļ������Ŀ¼   |
| testOutputDirectory | �����ļ����Ŀ¼ |
| finalName | ָ�����մ���İ��� |
| defaultGoal | ָ��Ĭ�ϵĲ�������ִ��maven����ʱû��ָ��������ʹ��Ĭ�ϲ�������`compile`��`install` |
| filters | ʹ�������ļ��е�ֵ�滻��Ŀ�е�ռλ�� |

### - properties
ͨ��key-value�ķ�ʽ�Զ���һЩ������
