> **Forked from 👉 [running_page](https://github.com/yihong0618/running_page)** 

## 工作原理

### 周期性同步

1. `.github/workflows/run_data_sync.yml`中定义了周期性的执行github action workflow，做的事情是：执行scripts中的脚本拉取strava中的跑步数据，并保存到`data.db`文件，再push到master分支

    > strave api鉴权所需凭证存放在repo下的secrets设置中，作为环境变量提供给脚本

2. `.github/workflows/gh-pages.yml`中定义了workflow的触发方式是当`run_data_sync`这个workflow完成时触发。gh-pages做的事情是根据第一步生成的数据重新生成静态网页，然后将改动push到gh-pages分支

3. 通过在repo的settings -> pages中配置github pages从gh-pages分支生成，那么每次当gh-pages分支有更新时，就会自动触发[pages-build-deployment](https://github.com/cvvz/running/actions/workflows/pages/pages-build-deployment)流水线，生成并部署新的静态页面到指定domain下

![image](https://github.com/cvvz/running/assets/44308864/7af935b6-7636-4836-986c-c9af56808c20)

### 历史数据导入

历史运动数据，可以通过先导出gpx文件，放到GPX_OUT目录，再执行`python3 scripts/gpx_sync.py`一次性的导入到data.db中



