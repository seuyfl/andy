# andy
 R 学习笔记
               R 学习笔记


s <- read.csv(‘stateData.csv’)

subset(stateInfo, state.region==1)显示子集中所有满足region=1的部分
summary(diamonds$price>=15000) 总结满足条件的子集数

s[s$state.region==1, Colum]取子集中行满足条件，列满足条件的部分
str(data)得到data数据里面参数的类型和总类
names()   pf文件所有列的名称，所有pf可以$的参数
subset(mtcars, mpg>=30 | hp<60) 或者mtcars[mtcars$mpg>=30 |mtcars$hp<60]
length(unique(pf$price))查看所有不同价格的总数
table(reddit$employment.status)查看参数下面所有的数据各类的个数
例如table(unique(pf$price))肯定全部是1，table(pf$price)就可以看到相同价格的个数
summary(reddit)， summary(pf$mobile_likes) 如果是summary(pf$mobile_likes>0) 结果就是总结true or false的数目
因子，levels(reddit$age.range)查看因子顺序
有序因子
reddit$age.range <- ordered(reddit$age.range , levels=c(‘under18’, 。。。排序))
或者   reddit$age.range <- factor(reddit$age.range ,levels=c(‘under18’, 。。。排序), ordered=T)
Ggplot 画图
1.柱状图
ggplot(aes(x = tenure/365), data = pf) + 
   geom_histogram(binwidth = .25, color = 'black', fill = '#F79420')
或者r自带画图 qplot(x=pf$tenure, data=pf,bins=9,fill = '#F79420')
ggplot(aes(x = tenure / 365), data = pf) + 
  geom_histogram(color = 'black', fill = '#F79420') + 
  scale_x_continuous(breaks = seq(1, 7, 1), limits = c(0, 7)) + 
  xlab('Number of years using Facebook') + 
  ylab('Number of users in sample')
线图
ggplot(aes(x = age, y = friend_count), 
              data = subset(pf, !is.na(gender))) +   geom_line(aes(color = gender), stat = 'summary', fun.y = median)  #取y值得平均，子集为gender非NA，按照gender分颜色画线图
pf<-filter(pf,!is.na(gender))过滤gender里面NA的部分，等价于data = subset(pf, !is.na(gender)

数据转换为log10(x), 
ggplot(aes(x = friend_count), data = pf) + scale_x_log10()
    qplot(x=log10(friend_count),data=pf),默认的y给出关于x的数目的直方图

频率多边形，把histo的曲线画出来，并显示在一个坐标轴里面
ggplot(aes(x = friend_count, y = ...), data = subset(pf, !is.na(gender))) +
  geom_freqpoly(aes(color = gender), binwidth=10) + 
  scale_x_continuous(limits = c(0, 1000), breaks = seq(0, 1000, 50)) + 
  xlab('好友数量') + 
  ylab('Percentage of users with that friend count')

箱线图
qplot(x=gender,y=friend_count,data=subset(pf,!is.na(gender)), geom=’boxplot’) +
coord_cartesian(ylim=c(0,500))限制y轴

by(pf$friendships_initiated, pf$gender, summary) 显示第一个数据，用第二组分开，总结


限制X轴， 
ggplot(aes(x = friend_count, y = ..count../sum(..count..)), data = subset(pf, !is.na(gender))) +
  geom_freqpoly(aes(color = gender), binwidth=10) + 
  scale_x_continuous(limits = c(0, 1000), breaks = seq(0, 1000, 50)) + 
  facet_wrap(~gender) 切分 gender   C()限制x轴范围，breaks 显示每50显示一次

 table (pf$gender)查看各自总数分布或者 summary (pf$gender)

by(diamonds$price,diamonds$cut,summary)查看各自的summary，均值中位数等

  xlab('好友数量') + 
  ylab('Percentage of users with that friend count')

函数by（）计算总数
 by(pf$www_likes, pf$gender, sum)同时显示男性和女性的like总数
在查看因子变量的总数以及满足条件的因子变量比例时，使用len（）计算其总数
sum(pf$mobile_check_in ==1)/ len(pf$mobile_check_in)

同时画多图
p1<- ..
p2<-..

grid.arrange(p1,p2,p3.., ncol=列的数目)

二维变量的分析
散点图
ggplot(aes(x = age, y = friend_count), data = pf) + 
  geom_point()
ggplot(aes(x = age, y = friend_count), data = pf) + 
  geom_jitter()   #x轴变化趋于连续，不是一列一列的，增加抖动

ggplot(aes(x = age, y = friend_count), data = pf)+
     geom_point(alpha=0.05， position=position_jitter(h=0))+
      xlim (13 , 90)+ 
      coord_trans(y='sqrt') #坐标轴的转换另一种方法
不能使用jitter，因为jitter抖动可能出现负数。设置position控制最小值

ggplot(aes(x = age, y = friend_count), data = pf)+
      geom_point(alpha=0.5, position=position_jitter(h=0), color='yellow')+
      xlim (13 , 90)+ 或者用百分比限制x轴，xlim(0,quantile(pf$age, 0.95))取95%的age的数据集合
这边也可以去data的data=subset(diamonds, volume <= quantile(volume, 0.99) & volume > 0 )

      coord_trans(y='sqrt')+
#下面分别取y轴的均值线，10%和90%的线
      geom_line(stat='summary', fun.y=mean)+
      geom_line(stat='summary', fun.y="quantile", fun.args=list(probs=0.1),
      linetype=2, color='blue')+
      geom_line(stat='summary', fun.y="quantile", fun.args=list(probs=0.9),
      linetype=2, color='blue'))+
      coord_cartesian(ylim = c(0, 1000))

计算相关系数, 计算子集的相关系数
cor.test(pf$age,pf$friend_count， method=’pearson’)
with(pf, cor.test(age, friend_count, method='pearson'))
with(subset(pf,pf$age<=70), cor.test(age, friend_count, method='pearson'))

使用dplyr库的summarise函数，计算均值，中位数和计算总数
age_group<- group_by(pf, age_with_months)
pf.fc_by_age<-summarise( age_group,
             friend_count_mean=mean(friend_count),
             friend_count_median=median(friend_count),
             numbers=n())
另一种表达方式(%>%可能是%.%一些版本)
   pf.fc_by_age_months<-pf%>% 
group_by(age_with_months)%>%            
       	summarise( friend_count_mean=mean(friend_count),
       	 friend_count_median=median(friend_count),
        	numbers=n())%>%
        arrange(age_with_months)

pf.fc_by_age<- arrange(pf.fc_by_age,age_with_months)  arrange函数排序
range(pf$age_with_months) range函数看参数范围，最小值最大值

p1 <-ggplot(aes(x = age_with_months, y = friend_count_mean), data =pf.fc_by_age_months) +
        geom_line()+
        geom_smooth()+
        coord_cartesian(xlim=c(0,71),ylim = c(0, 500))
p2 <-ggplot(aes(x = round(age/5)*5, y = friend_count_mean), data =pf.fc_by_age) +
        geom_line()+ 
        geom_smooth()+
        xlim(0,71)                   round（）这个函数将变化大的曲线平滑化类似于geom_smooth
library(gridExtra)
grid.arrange(p1,p2, ncol=1)

geom_smooth()函数取出一条光滑曲线


diamonds_by_color <- group_by(diamonds, color)
diamonds_mp_by_color <- summarise(diamonds_by_color, mean_price = mean(price))
ggplot(diamonds_mp_by_color, aes(x = color, y = mean_price,  group =factor(1))) + 
geom_bar(stat = "identity")
画条形图，x轴变量不连续。如颜色




探索多维数据

pf.fc_by_age_gender<-pf%>% 
        group_by(age,gender)%>%            
       	summarise( friend_count_mean=mean(friend_count),
       	             median_friend_count=median(friend_count),
        	            n=n())
对age和gender分组，同一个年龄分为female和male。
Reshape 模块的功能？？？将分开的gender分为两列，网址ftp://cran.r-project.org/pub/R/web/packages/data.table/vignettes/datatable-reshape.html install.packages(reshape2)
pf.fc_by_age_gender.wide<-dcast(pf.fc_by_age_gender，
                                        age~ gender,（age保留列的变量，gender分裂的变量）
                                        value.var=’median_friend_count’ 这个值是分开gender后对应的值
我们还可以使用 dplyr 和 tidyr 包创建相似的数据框：
pf.fc_by_age_gender.wide <- subset(pf.fc_by_age_gender[c('age', 'gender', 'median_friend_count')],!is.na(gender)) %>% 
      spread(gender, median_friend_count)
核心是spread函数，将之前提取的参数，按gender分开，添加列的结果是median_friend_count参数值，返回值是dataframe


ggplot(aes(x = age, y = female/male), 
              data =pf.fc_by_age_gender.wide) +   
              geom_line()+
              geom_hline(aes(yintercept=1),linetype=2) 在y轴上画区分线
重新定义添加一个参数：pf$year_joined<-  ceiling(2014-pf$tenure/365)
另外一个方法：transform函数 dataFrame <- transform(dataFrame, newColumnName =所需公式）公式中直接写参数名，不需$
或者 dataFrame <- dataFrame%>% mutate(volume=x*y*z)
用cut函数割分产生新的参数：pf$year_joined需要cut的变量，C()括号里参数是选取的cut点
pf$year_joined.bucket <- cut(pf$year_joined,breaks=c(2004,2009,2011,2012,2014))
分割完的yeay_joined对meidan画线图，在一个图里面
ggplot(aes(x = age, y = friend_count), 
              data = subset(pf, !is.na(year_joined.bucket))) + 
  geom_line(aes(color =year_joined.bucket), stat = 'summary', fun.y = median)
关系曲线波动（噪音）太大，无法看到细节。使用smooth函数调整
ggplot(aes(x =tenure, y = friendships_initiated/tenure), 
              data = subset(pf, tenure>=1)) + 
 		 geom_smooth(aes(color =year_joined.bucket))

时间序列：伪随机选取16个id，显示在一个图上，size是点的大小，pch是点形状，1圆2三角形
set.seed(4230)
sample.ids<- sample(levels(yo$id),16)
ggplot(aes(x=time, y=price),
       data=subset(yo,id%in%sample.ids))+
        facet_wrap(~id)+
        geom_line()+
        geom_point(aes(size=all.purchases),pch=1)

散点图矩阵：同时显示很多图
install.packages(“GGally”)
