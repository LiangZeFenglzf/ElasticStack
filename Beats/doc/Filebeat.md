

[filebeat默认Index template名称]: https://www.elastic.co/guide/en/beats/filebeat/current/configuration-template.html	"filebeat-%[agent.Version]"
[需要Load Index template]: https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-template.html#overwrite-template

![image-20220816160619153](C:\Users\Administrator\Desktop\Filebeat.assets\image-20220816160619153.png)

# Overview

# How works

## harvester

[^]: https://www.elastic.co/guide/en/beats/filebeat/current/how-filebeat-works.html#harvester

1 负责读取单个文件内容，发送给指定输出

2 分行读取

## input

input负责找到所有的输入源，管理Havesters

## how filebeat keep the state of files

```
Filebeat keeps the state of each file and frequently flushes the state to disk in the registry file. The state is used to remember the last offset a harvester was reading from and to ensure all log lines are sent.  While Filebeat is running, the state information is also kept in memory for each input. When Filebeat is restarted, data from the registry file is used to rebuild the state, and Filebeat continues each harvester at the last known position.
```

通过registry文件去存储文件的状态。

文件状态用来记住harvester读取的offset，确保所有日志line可以被发送。filebeat运行状态信息存储在内存。filebeat重启时registry用来重新建立state。

[]: 

# configure

[index template]: https://www.elastic.co/guide/en/beats/filebeat/current/configuration-template.html	"参考filebeat.reference.yml文件，模板默认名为filebeat-%[version]"

setup.template指定index模板去使用mapping，enable，filebeat加载索引模板自动。

------

那是先在kibana dev tools通过api创建一个index template，然后启动filebeat时配制好这块，这样filebeat就可以用这个映射日志文件了？

------

**需要实验一下这一点**。

[^改写Index template]: https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-template.html



## load balance

https://www.elastic.co/guide/en/beats/filebeat/current/load-balancing.html

kafka不需要配置，kafka自带Balance