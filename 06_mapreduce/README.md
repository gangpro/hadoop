# 맵리듀스
> MapReduce

> 개발환경<br> 
> OS : Macbook Pro, macOS Mojave<br>
> VirtualBox : Version 6.0.4<br>
> 가상환경 : Enterprise Linux<br>
> JDK : jdk1.8.0_211<br>
> Hadoop 1.2.1

## 맵리듀스(MapReduce) 이해하기
###
                Input   	Output
      Map   	<k1, v1>	list (<k2, v2>)             # k1(문장), v1(한줄)           -> Map    함수 ->  list{k2(단어), v2(개수)}  -> 셔플
      Reduce	<k2, list(v2)>	list (<k3, v3>)         # k2(단어), list(v2(개수))     -> Reduce 함수 ->  list{k3(), v3()}
    
      https://www.slideshare.net/gruter/mapreduce-tuning에서 4번 슬라이드
      https://www.tutorialspoint.com/hadoop/hadoop_mapreduce.htm
      http://www.yes24.com/24/Goods/9146116

## WordCount 소스 분석 : http://me2.do/5Z4aYa6k
