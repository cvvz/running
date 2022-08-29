> **Forked from 👉 [running_page](https://github.com/yihong0618/running_page)** 

## 工作原理

1. run_data_sync.yml 中定义了schedule，会周期性的自动执行github action workflow，这个workflow做的事情是：执行脚本拉取app中的跑步数据，并保存到本地的`data.db`文件，并生成一些svg图片，再将改动push到master分支

    > 拉取跑步数据需要调用对应app的api，api鉴权所需凭证存放在repo下的secrets设置中，作为环境变量提供给脚本

    > keep可以通过手机号、密码直接拉取运动数据；而有些运动软件不支持直接提供接口的话，就需要先配置自动同步到strava，再调用strava的api拉取数据

    > 有的app里的历史运动数据，可以通过先导出gpx文件，放到GPX_OUT目录，再执行`python3 scripts/gpx_sync.py`一次性的导入到data.db中

2. push到master分支这个行为则会触发gh-pages.yml中定义的workflow，这个workflow做的事情是根据第二步生成的数据和图片重新生成静态网页，然后将改动push 到gh-pages分支

3. 通过在repo的settings -> pages中配置github pages从gh-pages分支生成，那么每次gh-pages分支发生变化时，就会触发这个流水线运行，生成并发布新的网页


