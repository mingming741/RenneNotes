# gcloud
Google Cloud用于manager云服务的console界面，提供了多种的窗口界面的API

### project
Project创建了本地gcloud的工作环境，用于在和云端交互的时候提供默认的一些参数。

创建project：
`gcloud projects create [PROJECT_ID]`

删除一个project：
`gcloud projects delete PROJECT_ID`

选择当前project进入config，ID为project的名字
`gcloud config set project <PROJECT ID>`

去除project的config选择：
`gcloud config unset project`


### Check
查看全部的project
`gcloud projects list`

查看当前本地环境：
`gcloud config list`
`gcloud info`

查看当前给予权限的google账号：
`gcloud auth list`

### VM
查看instance
`gcloud compute instances list`

查看某个instance的细节
`gcloud compute instances describe my-instance`

连接到instance：
```
  gcloud compute ssh my-instance
  gcloud compute ssh user@my-instance
``` 

