# 竞彩规则及计算
做项目有机会接触了下体彩中心发行的竞彩业务，这里对投注规则及注数计算方法进行总结。
## 足球竞赛
足球竞赛投注有两种模式，*单关* 和 *串关*。

单关投注计算方式是从投注比赛中每场选一个投注项和其它场次组合成一个最终投注单。形式简单不多介绍。

串关投注计算方式比较复杂，可以解决单关投注方式效率低的问题。以下详细介绍。
### 串关玩法介绍
共有6种玩法，*胜平负*，*让球胜平负*，*比分*，*半全场*，*总进球*，*混合过关*。不同玩法规则下每场比赛的投注选项不一样：
+ 胜平负

    每场比赛有三个投注选项。

        ['胜'，'平'，'负']
+ 让球胜平负

    每场比赛有三个投注选项。

        ['让胜'，'让平'，'让负']
+ 比分

    每场比赛有31个投注选项。
    
        [
            '1:0', '2:0', '2:1', '3:0', '3:1', '3:2', '4:0', '4:1', '4:2', '5:0', '5:1', '5:2', '胜其他',
            '0:0', '1:1', '2:2', '3:3', '平其他',
            '0:1', '0:2', '1:2', '0:3', '1:3', '2:3', '0:4', '1:4', '2:4', '0:5', '1:5', '2:5', '负其他'
        ]
+ 半全场

    每场比赛有9个投注选项。

        ['胜胜', '胜平', '胜负', '平胜', '平平', '平负', '负胜', '负平', '负负']
+ 总进球

    每场比赛有8个投注选项。

        ['0', '1', '2', '3', '4', '5', '6', '7+']
+ 混合

    每场比赛投注选项由以上选项混合而成。

### 串关过关方式
足球竞彩投注，用户至少选择两场比赛，最多选10-15场进行投注，每场比赛可选多个投注项。串关是对所选比赛场数进行组合搭配的一种快捷方法，`M串N` 计作 `MxN`。最多8串。

串关模式下过关方式分为两种：
1. 自由过关

    自由选择过关的场数组合，可选2场、3场...8场。分别组成串关组合，如2串1，3串1...8串1。投注时可多选。
2. 多串过关

    按照固定的模式对场数组合，至少3场，如3串3，3串4...8串247。投注时不可多选。
    
    *固定模式的一般解释*：对投注比赛场次先按照过关场数进行分组，然后对每组按照过关分组数区间进行分组，最终对分组后每组内的比赛每场选一个投注项进行组合。
    
    例如：有投注比赛`['A','B','C','D']`（ABCD分别代表一场比赛的投注内容，每场比赛可选多个内容），选择模式`3串3`，则过关场数是3，对该投注结果先按照3场一组进行组合得到
        
        [ 
            [ 'A' , 'B' , 'C' ],
            [ 'A' , 'B' , 'D' ],
            [ 'A' , 'C' , 'D' ],
            [ 'B' , 'C' , 'D' ]
        ]
    又`3串3`的过关分组数区间为2，然后对分组后的每组按照2场一组进行组合得到

        [ 
            [ 'A', 'B' ],
            [ 'A', 'C' ],
            [ 'B', 'C' ],
            [ 'A', 'B' ],
            [ 'A', 'D' ],
            [ 'B', 'D' ],
            [ 'A', 'C' ],
            [ 'A', 'D' ],
            [ 'C', 'D' ],
            [ 'B', 'C' ],
            [ 'B', 'D' ],
            [ 'C', 'D' ] 
        ] 
    再然后对每一组的投注内容进行交叉组合，最后形成多个投注项。假设比赛'A'投注内容为`['胜','平']`，'B'投注内容为`['让平','让负']`，则以上分组['A','B']形成投注项为
    
        [
            ['胜', '让平'],
            ['胜', '让负'],
            ['平', '让平'],
            ['平', '让负'],
        ]
    以此类推，遍历计算每组比赛投注内容的组合，得出最后的投注组合。

#### 多串过关方式列表
针对投注的比赛赛果，每种串关方式由过关场数，过关分组数区间构成。按照过关场数分组后，对过关分组数区间进行以上规则的循环计算。
+ 3串3 过关场数3，分组数区间2。
+ 3串4 过关场数3，分组数区间2-3。
+ 4串4 过关场数4，分组数区间3。
+ 4串5 过关场数4，分组数区间3-4。
+ 4串6 过关场数4，分组数区间2。
+ 4串11 过关场数4，分组数区间2-4。
+ 5串5 过关场数5，分组数区间4。
+ 5串6 过关场数5，分组数区间4-5。
+ 5串10 过关场数5，分组数区间2。
+ 5串16 过关场数5，分组数区间3-5。
+ 5串20 过关场数5，分组数区间2-3。
+ 5串26 过关场数5，分组数区间2-5。
+ 6串6 过关场数6，分组数区间5。
+ 6串7 过关场数6，分组数区间5-6。
+ 6串15 过关场数6，分组数区间2。
+ 6串20 过关场数6，分组数区间3。
+ 6串22 过关场数6，分组数区间4-6。
+ 6串35 过关场数6，分组数区间2-3。
+ 6串42 过关场数6，分组数区间3-6。
+ 6串50 过关场数6，分组数区间2-4。
+ 6串57 过关场数6，分组数区间2-6。
+ 7串7 过关场数7，分组数区间6。
+ 7串8 过关场数7，分组数区间6-7。
+ 7串21 过关场数7，分组数区间5。
+ 7串35 过关场数7，分组数区间4。
+ 7串120 过关场数7，分组数区间2-7。
+ 8串8 过关场数8，分组数区间7。
+ 8串9 过关场数8，分组数区间7-8。
+ 8串28 过关场数8，分组数区间6。
+ 8串56 过关场数8，分组数区间5。
+ 8串70 过关场数8，分组数区间4。
+ 8串247 过关场数8，分组数区间2-8。

#### 串关模式的选取
当投注比赛场数大于串关要求数量是可以选择该串关方式，如投注4场比赛：
+ 自由过关，则可选2x1，3x1，4x1。不可选5x1。
+ 多串过关，则可选3x3，3x4，4x4，4x5，4x6，4x11。不可选5x5。

### 串关投注结果展示
如胜平负玩法：
    
    [
        ['胜', '负'], //拉齐奥 vs 乌迪内斯
        ['胜', '平', '负'], //桑普 vs 罗马
        ...
    ]
