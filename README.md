# README
> 개발환경<br> 
> OS : Macbook Pro, macOS Mojave<br>
> VirtualBox : Version 6.0.4<br>
> VM : Enterprise Linux<br>
> JDK : jdk1.8.0_211<br>
> Hadoop 1.2.1

## 목차
1. VirtureBox 설치
2. VirtureBox에 Enterprise Linux 설치
3. Network 세팅
4. Java JDK 설치
5. Setup Hadoop 설치
6. Mapreduce
7. Hadoop Streaming

<br>
<br>
<br>

## 참고
* Hadoop ≒ Distributed Proramming Framework 
###
    - HDFS      : 분산저장 : NameNode, DataNode       -> hdfs_comic.pdf
    - MapReduce : 분산처리 : JobTracker, TaskTracker  -> MapReduce Design Patterns.pdf

    >> http://terms.naver.com/entry.nhn?docId=1691556&cid=42171&categoryId=42183
    >> Cloudera, Hortonworks, MapR
       -> https://kimws.wordpress.com/2012/02/01/빅데이터-솔루션-업체-관계와-하둡기술기반의-스타/

    >> How To Learn
       -> https://learn.mapr.com/
          https://mapr.com/developer-portal/mapr-tutorials/
       -> https://ko.hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/
          https://ko.hortonworks.com/tutorials/
       -> https://www.cloudera.com/about/training.html
          https://www.edureka.co/blog/cloudera-hadoop-tutorial/

    >> 참고 교재
       -> http://www.yes24.com/Product/Goods/47699009 : 실무 프로젝트로 배우는 빅데이터 기술
       -> http://www.yes24.com/Product/Goods/44307209 : 하둡과 스파크를 활용한 실용 데이터 과학 

<br>
<br>
<br>

## 참고.
* 하둡 에코시스템(Hadoop Ecosystem)
* 하둡은 분산 프로그래밍 프레임워크, 하둡 에코시스템은 하둡을 이루고 있는 다양한 서브 프로젝트들의 모임.
###
    하둡 코어 프로젝트
      - 분산 데이터 저장 HDFS
      - 분산 데이터 처리 MapReduce
        
    하둡 서브 프로젝트
      - 워크플로우 관리 Oozie, Ambari
      - 실시간 SQL 질의 Impala, Tajo
      - 직렬화 Avro
      - 데이터 분석 Pig, Hive
      - 데이터 마이닝 Mahout
      - 메타데이터 관리 HCatalog
      - 비정형 데이터 수집 Chuckwa, Flume, Scribe
      - 정형 데이터 수집 Sqoop, hiho
      - 분산 코디네이터 Zookeeper
      - 분산 데이터베이스 Hbase, Cassandra