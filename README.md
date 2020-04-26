# Android_RecyclerView_QianTao
Android-学习-016-RecyclerView嵌套RecyclerView点击事件实例-2020-4-26
代码**来源**，例子很**好**

> https://www.jianshu.com/p/eab9d2019cda
> https://github.com/a824676719/RecycleNestDemo

如果懒的改，或者有问题不知道怎么改，我改后的

> https://github.com/zhengduolctao/Android_RecyclerView_QianTao

推荐去看一下他的博客，去下载代码包
我**只**做**分析**
@[TOC](目录)
# 一、结构
(我以Empty Activity修改)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200426180630309.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MTA5NjU2OQ==,size_16,color_FFFFFF,t_70#pic_center)
## 五个类:
Bean类**用来存储返回数据**
GridSpacingItemDecoration类**一个动态设置item个数,间距的工具类**
RvAdapter类最外层**RecyclerView的适配器**
RvvAdapter类最内层**RecyclerView的适配器**
MainActivity类:**新建数据，主类**
## 三个layout.xml
activity_main.xml  **放置一个recyclerview**
item_detail_list.xml(原名item_detaillist)**外层recyclerview**
item_detail_option.xml **内层recyclerview**
## 两个drawable.xml
background_grid_select.xml            **选中时选项外框样式**
background_grid_unselect.xml        **未选中时选项外框样式**
## 一个values.xml
colors.xml              **颜色**

# 二、问题
当你下载了上面代码
[问题:The given artifact contains a string literal with a package reference 'andro](https://blog.csdn.net/weixin_41096569/article/details/105756751)

[问题:Static interface methods are only supported starting with Android N (--min-api 24): void butterkn](https://blog.csdn.net/weixin_41096569/article/details/105756714)

[问题: Error inflating class RecyclerView](https://blog.csdn.net/weixin_41096569/article/details/105756573)
# 三、修改
因为那篇博客旧，版本要更新
我就那个**代码包**进行修改,新建一个**Empty Activity**,然后copy
### 1.五个类不改
### 2.drawable不改
### 3.values不改
### 4.layout改
**activity_main.xml**和**item_detail_list.xml**(原名item_detaillist.xml） 
android.support.v7.widget.RecyclerView
改为
androidx.recyclerview.widget.RecyclerView
### 5.依赖改
1.
build.gradle(Module:app)->dependencise
```bash
implementation 'com.jakewharton:butterknife:10.2.1'
annotationProcessor 'com.jakewharton:butterknife-compiler:10.2.1'

implementation 'com.yqritc:recyclerview-flexibledivider:1.4.0'
implementation "androidx.recyclerview:recyclerview:1.1.0"
implementation "androidx.recyclerview:recyclerview-selection:1.1.0-rc01"
```
2.
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200425210538215.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MTA5NjU2OQ==,size_16,color_FFFFFF,t_70#pic_center)

```bash
    compileOptions {

        sourceCompatibility JavaVersion.VERSION_1_8

        targetCompatibility JavaVersion.VERSION_1_8

    }
```
## 四、分析
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200426183304707.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MTA5NjU2OQ==,size_16,color_FFFFFF,t_70#pic_center)


### 1.循环同结构(原代码效果)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200426183211906.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MTA5NjU2OQ==,size_16,color_FFFFFF,t_70#pic_center)

**原** MainActivity.java 里的initdata

```bash
   private void initdata() {
        LinearLayoutManager layoutManager = new LinearLayoutManager(this);
        layoutManager.setOrientation(LinearLayoutManager.VERTICAL);
        c_RecyclerView.setLayoutManager(layoutManager);
        c_RecyclerView.setFocusableInTouchMode(false);

        mBean = new Bean();
        List<Bean.DatasBean> datas = new ArrayList<>();

        //模拟一些数据
        for (int i = 0; i < 20; i++) {
            Bean.DatasBean datasBean = new Bean.DatasBean();
            List<Bean.DatasBean.Option> option = new ArrayList<>();

            Bean.DatasBean.Option optionBean = new Bean.DatasBean.Option();
            optionBean.setDatas("这是选项" + i);
            option.add(optionBean);

            Bean.DatasBean.Option optionBean1 = new Bean.DatasBean.Option();
            optionBean1.setDatas("这是选项" + (i + 1));
            option.add(optionBean1);

            Bean.DatasBean.Option optionBean2 = new Bean.DatasBean.Option();
            optionBean2.setDatas("这是选项" + (i + 2));
            option.add(optionBean2);

            Bean.DatasBean.Option optionBean3 = new Bean.DatasBean.Option();
            optionBean3.setDatas("这是选项" + (i + 3));
            option.add(optionBean3);

            datasBean.setOptions(option);
            datasBean.setTitle("这是标题哦1");
            datas.add(datasBean);
        }

        mBean.setDatas(datas);

        mRvAdapter = new RvAdapter(this, mBean.getDatas());
        c_RecyclerView.setAdapter(mRvAdapter);
        mRvAdapter.notifyDataSetChanged();
    }
```

## 2.不同结构
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200426183512297.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MTA5NjU2OQ==,size_16,color_FFFFFF,t_70#pic_center)
**改后**MainActivity.java 里的initdata
```bash
private void initdata() {
        LinearLayoutManager layoutManager = new LinearLayoutManager(this);
        layoutManager.setOrientation(LinearLayoutManager.VERTICAL);
        c_RecyclerView.setLayoutManager(layoutManager);
        c_RecyclerView.setFocusableInTouchMode(false);

        mBean = new Bean();
        List<Bean.DatasBean> datas = new ArrayList<>();

        //模拟一些数据
        //for (int i = 0; i < 20; i++) {
        Bean.DatasBean datasBean = new Bean.DatasBean();
        List<Bean.DatasBean.Option> option = new ArrayList<>();

        Bean.DatasBean.Option optionBean = new Bean.DatasBean.Option();
        optionBean.setDatas("这是选项" + 1);
        option.add(optionBean);

        Bean.DatasBean.Option optionBean4 = new Bean.DatasBean.Option();
        optionBean4.setDatas("这是选项" + 2);
        option.add(optionBean4);

        datasBean.setOptions(option);
        datasBean.setTitle("这是标题哦" + 1);
        datas.add(datasBean);

        Bean.DatasBean datasBean1 = new Bean.DatasBean();
        List<Bean.DatasBean.Option> option1 = new ArrayList<>();

        Bean.DatasBean.Option optionBean1 = new Bean.DatasBean.Option();
        optionBean1.setDatas("这是选项" + 3);
        option1.add(optionBean1);

        Bean.DatasBean.Option optionBean2 = new Bean.DatasBean.Option();
        optionBean2.setDatas("这是选项" + 4);
        option1.add(optionBean2);

        Bean.DatasBean.Option optionBean3 = new Bean.DatasBean.Option();
        optionBean3.setDatas("这是选项" + 5);
        option1.add(optionBean3);

        datasBean1.setOptions(option1);
        datasBean1.setTitle("这是标题哦" + 2);
        datas.add(datasBean1);
        //}

        mBean.setDatas(datas);

        mRvAdapter = new RvAdapter(this, mBean.getDatas());
        c_RecyclerView.setAdapter(mRvAdapter);
        mRvAdapter.notifyDataSetChanged();
    }

```
