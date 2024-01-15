---
{"url":"https://zhuanlan.zhihu.com/p/347371752","title":"梅登黑德 (Maidenhead) 网格","date":"2024-01-15 14:01:44","tags":null,"summary":null,"dg-publish":true,"permalink":"/9/maidenhead/","dgPassFrontmatter":true}
---

## 概述

梅登黑德网格，也被称为 “梅登黑德定位系统”，是业余无线电业务中常用的全球地理坐标描述方法。梅登黑德网格将地球表面的二维位置编码字母和数字，便于在电文中传输。扩展的网格增加编码的字段，以提升位置描述的精度。

## 历史

业余无线电界的网格定位系统起源于 1950 年代后期。当时在欧洲，随着电子技术发展，VHF 波段被用于业余无线电通联。与传统的短波波段不同，VHF 主要依赖视距传播，传输距离比较近，传统的基于国家字头或全球分区的位置统计显得太粗糙了，需要一种新的定位体系来计算通联覆盖的范围。欧洲的爱好者们在一些竞赛中开始应用一些区域的格网定位系统。

1958 年在德国小城魏恩海姆 (Weinheim) 举行的德国 VHF 会议上，DL3NQ 引入了覆盖欧洲地区的五位编码的格网地理位置描述方案 “QRA-Kennen”。这一方案在当时欧洲一些竞赛上试用后，于 1959 年正式写入业余无线电联盟一区(IARU R1) 的建议书。

当欧洲之外的爱好者也需要使用格网地理位置时，将欧洲方案全球化被提上议程。1980 年 4 月，在英国梅登黑德举行的欧洲 VHF 管理者会议上，提出了新的 “梅登黑德定位系统”，随后在 1982 年被 IARU R1 采用，在 1985 年 1 月 1 日后成为 1 区新的网格定位体系，并被全世界的业余无线电爱好者使用至今。

## 编码

标准梅登黑德网格由 3 个字段组成，第一个字段是 2 位大写字母，中间字段是 2 位数字，第三个字段是 2 位小写字母。每个字段中，前一个字母或数字表示经度，后一个表示纬度。

第一个字段，即前 2 位字母将地球表面按照经纬度划分为 18x18 个网格，每个网格的大小是经度 20 度，纬度 10 度。把西南角（左下角）位于东经西经 180 度，南纬 90 度的网格定义为 AA，向东向北编码到 RR。

![](https://pic2.zhimg.com/v2-24b41ee2a2e8d25da80c55517b5ee7cd_b.gif)

第二个字段，进一步将网格细分为 10x10 个二级网格，每个网格经度 2 度，纬度 1 度。一级网格中西南角的二级网格编码为 00，向东向北编码到 99。

![](https://pic4.zhimg.com/v2-2afb758d773d4bf4c522b5fc8f1ba63b_b.gif)

第三个字段，再将二级网格细分为 24x24 个三级网格，每个网格经度 5 分，纬度 2 分 30 秒。二级网格中西南角的三级网格编码为 aa，向东向北编码到 xx。

![](https://pic3.zhimg.com/v2-750b8d9e5c4a16cc57d0dd201d9fdd6a_b.gif)

扩展的梅登黑德网格编码重复上述第二、第三字段的编码方式不断提高精度，扩展到 10 个字段时，坐标描述精度达到厘米级。考虑到经纬度在地球上的分布不是均匀等间隔的，所以从经纬度网格衍生得到的梅登黑德网格，在不同地理位置具有不同的精度。每个网格覆盖的面积，在赤道地区更大一些，而在极地会很快减小。

![](https://pic1.zhimg.com/v2-64991a0e9a770d7bba376313e17a6988_r.jpg)

## 网格边界的处理

虽然 ARRL 推荐的刊载于 1989 年 QST 杂志的论文_Conversion Between Geodetic and Grid Locator Systems_ 定义的网格转换算法，明确规定了处于边界上的坐标处理方法，即算入边界西南的网格中。但实际上边界上的值没有很好定义。通常为了电脑编程简便，几乎所有的网格转换程序都会把边界算入东北方的网格里。例如，经度纬度都为零的点 0.00000N 0.00000E 的网格是 JJ00aa。

南极点和北极点的：纬度为 - 90 或 + 90 是地球上存在的点，转换程序应当正确处理这两个极点。我们设计的转换程序会将南极点应当显示为 AA00aa～RA90xa，经度项依赖于输入的经度值；将北极点显示为 AR09ax～RR99xx，经度项依赖于输入的经度值，第二个字符不会出现'S'。

180 度经线：经度 + 180 和 - 180 表示相同的地点，应当输出同样的网格结果。应当照前述边界处理，算入东北方的网格里。当然，在文末提到的 C 语言实现的转换函数中，我限制了只能用 - 180 度描述这些位置，+180 被视为错误的输入。

超出范围的经纬度：将主值范围外的输入转换到主值范围内的功能，实际应用中十分有用，特别是追踪飞行器的时候，但我考虑使用另一个独立函数实现，应当在转换前调用。

## 绿宝书上的 BASIC 程序

作为中国业余无线电爱好者的入门必读教材，被称为 “绿宝书” 的《业余无线电通信》一书上刊载了一段将经纬度坐标转换为梅登黑德网格的 BASIC 代码。

![](https://pic3.zhimg.com/v2-73621bc1113a9ade788886a45021ea9e_r.jpg)

我见过不止一位同学试图将这段代码敲进 BASIC 解释器中运行，但不幸的是，这段代码中存在一些错误，无法在 BASIC 解释器上运行。我们订正了这段程序中的错误，并改用大写字母让它更具有 BASIC 风范。但是由于找不到靠谱的解释器，这段修改后的代码并没有经过测试，不保证在某个 BASIC 解释器上能不经移植地运行。

```
10	PRINT "请依次输入所求地点经度的度、分、秒，中间用逗号隔开"
20	INPUT "东经为正，西经为负：": A, B, C
30	X = A + B/60 + C/3600 + 1/1000000 
40	IF X < -180 OR X > 180 THEN PRINT "输入错误，请重新输入" : GOTO 10
50	PRINT "请依次输入所求地点纬度的度、分、秒，中间用逗号隔开"
60	PRINT "北纬为正，南纬为负：": D, E, F
70	Y = D + E/60 + F/3600 + 1/1000000
80	IF Y < -90 OR Y > 90 THEN PRINT "输入错误，请重新输入" : GOTO 50
90	A = X/20 + 9 : B = INT(A) : L$ = CHR$(B+65) 
100	C = Y/10 + 9 : D = INT(C) : L$ = L$ + CHR$(D+65) 
110	A = (A-B)*10 : B = INT(A) : L$ = L$ + CHR$(B+48) 
120	C = (C-D)*10 : D = INT(C) : L$ = L$ + CHR$(D+48)
140	B = INT((A-B)*24) : L$ = L$ + CHR$(B+65)
150	D = INT((C-D)*24) : L$ = L$ + CHR$(D+65)
160	PRINT ""
170	PRINT "**************************"
180	PRINT "你所求地点的网格位置是："; L$
190	PRINT "**************************"
200	END
```

相比原书中的程序，修改点如下：  
30 行，1+1000000 应当改为 1/100000，这里是为了避免浮点数精度造成取整错误问题。  
40 行，应当跳转重新输入，添加 “GOTO 10”。  
80 行，应当跳转重新输入，添加 “GOTO 50”。  
90～130 行，每一组计算分别写在了不同的行中，代码条理不清晰。  
120 行，最后的赋值语句缺少左值，添加被赋值的变量 “D”。  
180 行，最后的句号要作为字串写在引号里面，或者去掉 。

关于 BASIC 额外的注释：  
BASIC 中变量不须声明即可使用。普通的变量使用浮点型，以 $ 结尾的是字符串变量或字符串函数。代码中 INT() 表示取整，CHR$() 表示将数字转换成对应的 ASCII 码字符。PRINT 语句结尾的分号表示打印输出结尾不换行，冒号用于在一行内分割多个语句。

为了弥补 BASIC 程序没有运行的缺憾，我还把它改成了 C 语言，并增加 3 段以上的扩展编码。

函数原型是

```
char* lbh2mdg(double lon, double lat, int stage);
/* inputs:
 *  lon [degree] - longitude, positive number for East, [-180, 180)
 *  lat [degree] - latitude, positive number for North, [-90, 90]
 *  stage        - Maidenhead Grid character number
 *                 stage      3  4   5  6 ... 10
 *                 characters 6  8  10 12 ... 20
 */
```

运行结果是

![](https://pic4.zhimg.com/v2-c72af6c0d5260bb923589e69ba825327_r.jpg)

C 语言的版本大概会在我学会 Git/GitHub 之类工具用法后传到网上，有需要的话现在可以发邮件给我 mioyuki@foxmail.com 索取源代码和 CodeBlocks 的工程，此工程在 Ubuntu Linux 下用 GCC 编译可以运行。

## 参考

TYSON E. onversion Between Geodetic and Grid Locator SystemsQS TJanuary 1989, pp. 29-30, 43. [http://www.w9dup.org/pdfs/conversion_geodetic_grid.pdf](https://link.zhihu.com/?target=http%3A//www.w9dup.org/pdfs/conversion_geodetic_grid.pdf)  
[http://www.arrl.org/grid-squares](https://link.zhihu.com/?target=http%3A//www.arrl.org/grid-squares)  
[https://www.amsat.org/amsat-new/tools/grids.php](https://link.zhihu.com/?target=https%3A//www.amsat.org/amsat-new/tools/grids.php)  
[http://blog.chinaunix.net/uid-210143-id-5762033.html](https://link.zhihu.com/?target=http%3A//blog.chinaunix.net/uid-210143-id-5762033.html)  
[https://www.dxzone.com/grid-square-locator-system-explained/](https://link.zhihu.com/?target=https%3A//www.dxzone.com/grid-square-locator-system-explained/)

修订

TODO: 换用规范的地图底图重制图片 1～4  
TODO: 将 GIF 格式的图片改成 PNG 避免查看时出现播放 GIF 的图标