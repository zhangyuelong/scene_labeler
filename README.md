# scene_labeler
driving scene label tool

## 场景标注工具

因为需要从路试采集的数据中选取典型场景，因此开发了一个场景标注的小工具。
之前用表格形式对数据进行标注，但因类型之间不一定是互斥的的关系，采用表格会产生一个大的稀疏表格，人工管理起来不太直观。
采用tag的形式，一个场景可以选取不同的标签。如: ["高速","逆光","前车切除"]。
此外，程序还能根据自定义的测试目录，搜索场景的tag，将符合的场景加入测试列表中。

## 步骤
1. 用 data_input.py 来标注数据。定义了scene的对象，包括：输入场景的文件名，起始终止时间，标签（可输入多个），及文字描述。

``` python
video_id = 'DR2330322'
time_start = 11.1
time_end = 20.5
tag = ['ACC','目标误识别',]
desc = '临车道车辆识别成ACC目标' # description
```
输入完成后运行scene.save脚本会把数据自动添加到csv表格。

2. 用create_test.py 生成测试用例
定义了testcase对象。testcase需要两个输入， 测试目录和场景数据：testcase1 = Testcase(reg_test,dat)
-测试数据从csv文件读取，转成 pandas dataframe格式输入。
-测试目录按嵌套的python dictionary 格式创建。

``` python
reg_test = {'ACC':{'跟随前车':[],\
                   '前车切出':[],\
                   '前车切出':[]},\
            'AEB':{'车辆':[],\
                   '行人':[]},\
            'LKA':{'直道':[],\
                   '弯道':[]},}
```
测试目录可视化testcase1.show_test()

3.将加入场景自动加入测试目录 testcase1.add_scene2test()。脚本会搜索场景的tag, 将符合的场景加入测试列表中。
测试案例可视化 testcase1.show_testcase()

4.将挑选的测试场景生成测试案例list，导入matlab pipeline 进行回归测试。
``` python
# create test case
testcase1 = Testcase(reg_test,dat)
```
