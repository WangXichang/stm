
# Introduction 导言

score transform model

分数转换模型

models include: zhejiang/shanghai/shandong/beijing/tianjian level score model

模型中包括： 浙江 上海 山东 北京 天津 的等级分数转换模型

also include: Zscore, Tscore，level score model that Tao Baiqiang designed in his article.

也包括： Z分数、T分数转换，陶百强提出的等级分数转换模型

designer, Wang Xichang, wxc1964@126.com

设计者，姓名，信箱 

# Level Score Model 等级分数模型

(1) 用于浙江、上海、北京、天津等省新高考中的等级赋分模型。

the level score model, that is used to dispatch score value in Zhejiang, Shanghai, Beijing, Tianjin New High Test。

等级分数模型通过以下步骤完成：
1. 指定比例，划分原始分数为各个等级区间
2. 每个区间指定等级
3. 每个等级指定分数
4. 从各个考生的原始分数，通过其所在区间得到等级，通过等级得到等级分数。

stm的计算过程：
1. 计算分数字段的分段表，每个字段包括四个值：分段值（某一分值），对应该分段值的的人数，百分比，累积百分比，分段值按照顺序（默认从高到低）
2. 在分段表中计算，每个分段值对应的等级，等级分数
3. 使用分段表计算每个考生该分数字段的等级分数

(2) 用于山东省等级分数转换的模型
the model used in Shandong New High-Test Project

# use module 使用模块

1. import module 导入模块
   
   import pyex_stm as stm
   
2. get module information from function see() 调用see()函数查看模块的信息

   stm.see（）
   
   module function and class:
    
   [function] 模块中的主要函数
   
          run(name, df, field_list, ratio, level_max, level_diff, input_score_max, input_score_min,
          
           output_score_decimal=0, approx_mode='near'): 运行各个模型的接口函数
           
          
          通过指定name=‘shandong'/'shanghai'/'zhejiang'/'beijing'/'tianjin'/'tao'
          
          可以计算山东、上海、浙江、北京、天津、陶百强模型
          
          通过指定name = 'zscore'/'tscore'/'tlinear'
          
          也可以计算Z分数、T分数、线性转换T分数
          
          ---
          
          interface function
          
          caculate shandong... model by name = 'shandong' / 'shanghai'/'zhejiang'/'beijing'/'tianjin'/'tao'
          
          caculate Z,T,liear T score by name = 'zscore'/ 'tscore' / 'tlinear'
          
          ---
          
          parameters specification:
          
          name: model name
          
          df: input raw score data, type DataFrame of pandas
          
          field_list: score field to calculate in df
          
          ratio: ratio list including percent value for each interval of level score
          
          level_max: max value of level score
          
          level_diff: differentiao value of level score
          
          input_score_max: raw score max value
          
          input_score_min: raw score min value
          
          output_score_decimal: level score precision, decimal digit number
          
          approx_mode: how to approxmate score points of raw score for each ratio vlaue
          
          ---
          
          usage:
          
          >>>import pyex_stm as stm
          
          >>>result = stm.run(name='shandong', df=data, field_list=['ls'])
          
          >>>result.report()
          
          >>>result.output.head()
          
       plot(): 各方案按照比例转换后分数后的分布直方图
          
          plot models distribution hist graph including shandong,zhejiang,shanghai,beijing,tianjin
          
       round45i():四舍五入函数
          
          function for rounding strictly at some decimal position
          
       get_norm__dist_table(size, mean, std, stdnum): 根据均值和标准差数生成正态分布表
       
       creating norm data dataframe with assigned mean and standard deviation
    
    [class] 模块中的类
    
       PltScore: 分段线性转换模型, 山东省新高考改革使用 shandong model

       LevelScore: 等级分数转换模型, 浙江、上海、天津、北京使用 zhejiang shanghai tianjin beijing model

       Zscore: Z分数转换模型 zscore model
       
       Tscore: T分数转换模型 tscore model
       
       Tlinear: T分数线性转换模型 tscore model by linear transform mode
       
       SegTable: 计算分段表模型 segment table model
       
       TaoScore: 陶百强等级分数模型（由陶百强在其论文中提出）Tao Baiqiang model
    

   
3. use interface function run() 使用模块的入口函数run()

   >>>result = stm.run(name='shandong', df=data, field_list=['ls'])  
   
   calculate level score by using shandong model, subject is 'ls' in input dataframe "data"
   
4. result of model 模型运行结果

   display report 显示运行报告
   
   >>>result.report()
   
   ---<< score field: [lsn] >>---
   
    input score  mean, std: 43.84, 18.01

    input score percentage: [0.03, 0.07, 0.16, 0.24, 0.24, 0.16, 0.07, 0.03]

    input score  endpoints: [(92, 74), (73, 67), (66, 58), (57, 46), (45, 32), (31, 20), (19, 8), (7, 0)]

    output score endpoints: [(100, 91), (90, 81), (80, 71), (70, 61), (60, 51), (50, 41), (40, 31), (30, 21)]

    transform formulas: 
    
         0.5*(x-74)+91

         1.5*(x-67)+81

         1.125*(x-58)+71

         0.818182*(x-46)+61

         0.692308*(x-32)+51

         0.818182*(x-20)+41

         0.818182*(x-8)+31

         1.285714*(x-0)+21

    ------------------------------------------------------------------------------------------

# 模块中的分段表模型 SegTable
  用于生成某些分数范围的每个分值点的某些科目得分你的人数。
  

