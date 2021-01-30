# Setup Hadoop 
> 개발환경<br> 
> OS : Macbook Pro, macOS Mojave<br>
> VirtualBox : Version 6.0.4<br>
> 가상환경 : Enterprise Linux<br>
> JDK : jdk1.8.0_211<br>

## [0] 하둡 관리 및 하둡 프로그래밍용 리눅스 유저 생성
* 가상환경 리눅스에서 oracle 유저로 로그인후 root 유저로 전환
###    
      [orcl:~]$ su -
      Password: oracle    <- 보이지 않습니다.

      --hadoop 그룹 생성
      [root@edydr1p0 ~]# groupadd hadoop
      
      --hadoop 유저 생성
      [root@edydr1p0 ~]# useradd hadoop -g hadoop -G vboxsf

      --hadoop 유저 암호 설정  
      [root@edydr1p0 ~]# passwd hadoop
      Changing password for user hadoop.
      New UNIX password: hadoop          <- 보이지 않습니다.
      BAD PASSWORD: it is based on a dictionary word    <-너무 쉬운 비밀번호라 표시된다. 그래도 그냥 아래처럼 hadoop으로 설정
      Retype new UNIX password: hadoop   <- 보이지 않습니다.
      passwd: all authentication tokens updated successfully.

      --하둡 관련 파일 설치용 디렉토리 생성 
      [root@edydr1p0 ~]# mkdir /opt/hadoop

      --디렉토리 소유자 및 그룹 설정
      [root@edydr1p0 ~]# chown hadoop:hadoop /opt/hadoop

* hadoop 유저로 로그인
###
      [root@edydr1p0 ~]# su - hadoop    <- root 유저에서 su를 실행하면 암호를 물어보지 않습니다.
    
      [hadoop@edydr1p0 ~]$ pwd
      /home/hadoop
    
      [hadoop@edydr1p0 ~]$ vi .bash_profile
      export JAVA_HOME=/usr/java/jdk1.8.0_211
      export PATH=$JAVA_HOME/bin:$PATH
      
<img width="826" alt="Screen Shot 2019-04-19 at 3 29 04 PM" src="https://user-images.githubusercontent.com/46523571/56411895-49529280-62bd-11e9-9445-8dd8d87fc0a9.png">

      [hadoop@edydr1p0 ~]$ . .bash_profile
      
      [hadoop@edydr1p0 ~]$ echo $JAVA_HOME
      /usr/java/jdk1.8.0_211

      --하둡 (수업) 관련 파일을 저장할 디렉토리 생성
      [hadoop@edydr1p0 ~]$ mkdir download

<br>
<br>
<br>

## [1] Hadoop Setup : Single Node Setup 
* SSH 설정
  - 하둡은 SSH 프로토콜을 이용해 하둡 클러스터 간의 내부 통신을 수행합니다. 네임노드에서 SSH 공개키를 설정하고, 이 공개키를 전체 서버에 복사해야 합니다.
###    
      [hadoop@edydr1p0 ~]$ whoami
      hadoop
    
      [hadoop@edydr1p0 ~]$ cd
      
      [hadoop@edydr1p0 ~]$ ssh-keygen -t rsa
      Generating public/private rsa key pair.
      Enter file in which to save the key (/home/hadoop/.ssh/id_rsa):   <-- 그냥 Enter
      Created directory '/home/hadoop/.ssh'.
      Enter passphrase (empty for no passphrase):   <-- 그냥 Enter
      Enter same passphrase again:    <-- 그냥 Enter
      Your identification has been saved in /home/hadoop/.ssh/id_rsa.
      Your public key has been saved in /home/hadoop/.ssh/id_rsa.pub.
      The key fingerprint is:  <-- 그냥 Enter
      93:11:2f:ce:70:9a:01:39:98:25:c7:9b:d6:e3:7f:f2 hadoop@edydr1p0.us.oracle.com
    
      [hadoop@edydr1p0 ~]$ ls -lR .ssh
      .ssh:
      total 8
      -rw------- 1 hadoop hadoop 1679 Apr 19 15:29 id_rsa
      -rw-r--r-- 1 hadoop hadoop  411 Apr 19 15:29 id_rsa.pub
    
      [hadoop@edydr1p0 ~]$ cp ~/.ssh/id_rsa.pub ~/.ssh/authorized_keys
      
      [hadoop@edydr1p0 ~]$ ssh localhost
      The authenticity of host 'localhost (127.0.0.1)' can't be established.
      RSA key fingerprint is a3:43:5b:0b:8c:76:75:bc:37:b6:29:7f:df:29:ab:f0.
      Are you sure you want to continue connecting (yes/no)? yes      <-- yes
      Warning: Permanently added 'localhost' (RSA) to the list of known hosts.
    
      [hadoop@edydr1p0 ~]$ 

<br>
<br>
<br>

## Hadoop 다운로드 및 설치 
* http://mirror.apache-kr.org/hadoop/common에서 1.2.X stable 버전을 사용합니다.
###
      --설치할 경로로 이동
      [hadoop@edydr1p0 ~]$ cd /opt/hadoop
      
      --hadoop 다운로드
      [hadoop@edydr1p0 hadoop]$ wget https://archive.apache.org/dist/hadoop/core/hadoop-1.2.1/hadoop-1.2.1.tar.gz
      --2019-04-19 15:33:03--  https://archive.apache.org/dist/hadoop/core/hadoop-1.2.1/hadoop-1.2.1.tar.gz
      Resolving archive.apache.org... 163.172.17.199
      Connecting to archive.apache.org|163.172.17.199|:443... connected.
      HTTP request sent, awaiting response... 200 OK
      Length: 63851630 (61M) [application/x-gzip]
      Saving to: `hadoop-1.2.1.tar.gz'
    
      100%[======================================>] 63,851,630  4.70M/s   in 16s     
    
      2019-04-19 15:33:20 (3.90 MB/s) - `hadoop-1.2.1.tar.gz' saved [63851630/63851630]
      
      --hadoop 압축 해제
      [hadoop@edydr1p0 hadoop]$ tar xvfz hadoop-1.2.1.tar.gz
      엄청 길게 실행 됨+_+
      
      --심볼릭 링크 생성
      [hadoop@edydr1p0 hadoop]$ ln -s hadoop-1.2.1 hadoop
    
      [hadoop@edydr1p0 hadoop]$ cd 
    
      [hadoop@edydr1p0 ~]$ vi + .bash_profile
      가장 아래쪽에 추가하세요.
      export HADOOP_HOME=/opt/hadoop/hadoop
      export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
<img width="827" alt="Screen Shot 2019-04-19 at 3 35 32 PM" src="https://user-images.githubusercontent.com/46523571/56411960-8c146a80-62bd-11e9-942e-22d45121a480.png">
    
      [hadoop@edydr1p0 ~]$ . .bash_profile
    
      [hadoop@edydr1p0 ~]$ env|grep HOME
      HADOOP_HOME=/opt/hadoop/hadoop
      JAVA_HOME=/usr/java/jdk1.8.0_211
      HOME=/home/hadoop
    
      [hadoop@edydr1p0 ~]$ pwd
      /home/hadoop

<br>
<br>
<br>

## Hadoop 환경설정 파일 수정 : 6개
      [hadoop@edydr1p0 ~]$ cd $HADOOP_HOME/conf
      
      [hadoop@edydr1p0 conf]$ pwd
      /opt/hadoop/hadoop/conf

      [hadoop@edydr1p0 conf]$ ls
      capacity-scheduler.xml      hadoop-policy.xml      slaves
      configuration.xsl           hdfs-site.xml          ssl-client.xml.example
      core-site.xml               log4j.properties       ssl-server.xml.example
      fair-scheduler.xml          mapred-queue-acls.xml  taskcontroller.cfg
      hadoop-env.sh               mapred-site.xml        task-log4j.properties
      hadoop-metrics2.properties  masters

      [hadoop@edydr1p0 conf]$ ls *.sh
      hadoop-env.sh

      [hadoop@edydr1p0 conf]$ ls *.xml
      capacity-scheduler.xml  hadoop-policy.xml      mapred-site.xml
      core-site.xml           hdfs-site.xml
      fair-scheduler.xml      mapred-queue-acls.xml
###
      [hadoop@edydr1p0 conf]$ vi hadoop-env.sh    -- 위에서 여덟번째 줄 부근에 적절하게 입력하세요. 
      export JAVA_HOME=/usr/java/jdk1.8.0_211
      export HADOOP_HOME=/opt/hadoop/hadoop
      export HADOOP_HOME_WARN_SUPPRESS=1 
<img width="825" alt="Screen Shot 2019-04-19 at 3 39 32 PM" src="https://user-images.githubusercontent.com/46523571/56411990-abab9300-62bd-11e9-86df-2144fbe99ab9.png">

###
      [hadoop@edydr1p0 conf]$ more masters        -- SecondaryNameNode 설정
      localhost

###    
      [hadoop@edydr1p0 conf]$ more slaves         -- DataNode, Task Tracker 설정
      localhost

###    
      [hadoop@edydr1p0 conf]$ vi core-site.xml    -- NameNode의 호스트명과 포트 설정
      -- STS 플러그인으로 접속하려면 localhost 대신 ip address 사용할 것

      <configuration>
          <property>
              <name>fs.default.name</name>
              <value>hdfs://192.168.56.102:9000</value>     
          </property>
          <property>
              <name>hadoop.tmp.dir</name>
              <value>/opt/hadoop/hadoop-tmp-dir/</value>
          </property>
      </configuration>
<img width="825" alt="Screen Shot 2019-04-19 at 3 41 57 PM" src="https://user-images.githubusercontent.com/46523571/56412051-ea414d80-62bd-11e9-9246-54389036b4c0.png">
    
###
      [hadoop@edydr1p0 conf]$ vi hdfs-site.xml         -- HDFS에서 사용할 환경정보 설정
    
      <configuration>
          <property>
              <name>dfs.replication</name>
              <value>1</value>
          </property>
          <property>
              <name>dfs.http.address</name>
              <value>192.168.56.102:50070</value>
          </property>
          <property>
              <name>dfs.secondary.http.address</name>
              <value>192.168.56.102:50090</value>
          </property>
          <property>
              <name>dfs.name.dir</name>
              <value>/opt/hadoop/hadoop-tmp-dir/dfs/name</value>
          </property>
          <property>
              <name>dfs.name.edits.dir</name>
              <value>${dfs.name.dir}</value>
          </property>
          <property>
              <name>dfs.data.dir</name>
              <value>/opt/hadoop/hadoop-tmp-dir/dfs/data</value>
          </property>
          <property>
              <name>dfs.permissions</name>
              <value>false</value>
          </property>
      </configuration>
<img width="826" alt="Screen Shot 2019-04-19 at 3 42 46 PM" src="https://user-images.githubusercontent.com/46523571/56412084-0349fe80-62be-11e9-9932-30d54e6c8fd8.png">

###
      --위에서 dfs.permissions를 false로 해서 STS 플러그인에서 파일 업로드를 할 수 있게 한다.
     [hadoop@edydr1p0 conf]$ vi mapred-site.xml       -- JobTracker의 호스트명과 포트 설정
    
      <configuration>
           <property>
               <name>mapred.job.tracker</name>
               <value>192.168.56.102:9001</value>
           </property>
      </configuration>

<br>
<br>
<br>

## Hadoop 실행
      [hadoop@edydr1p0 conf]$ cd
    
      [hadoop@edydr1p0 ~]$ which hadoop
      /opt/hadoop/hadoop/bin/hadoop
    
      [hadoop@edydr1p0 ~]$ more $HADOOP_HOME/bin/hadoop
      more 100%로 될때까지 엔터 누르기~!
    
      [hadoop@edydr1p0 ~]$ hadoop namenode -format   
    19/04/19 15:48:02 INFO namenode.NameNode: STARTUP_MSG: 
    /************************************************************
    STARTUP_MSG: Starting NameNode
    STARTUP_MSG:   host = edydr1p0.us.oracle.com/127.0.0.1
    STARTUP_MSG:   args = [-format]
    STARTUP_MSG:   version = 1.2.1
    STARTUP_MSG:   build = https://svn.apache.org/repos/asf/hadoop/common/branches/branch-1.2 -r 1503152; compiled by 'mattf' on Mon Jul 22 15:23:09 PDT 2013
    STARTUP_MSG:   java = 1.8.0_211
    ************************************************************/
    19/04/19 15:48:02 INFO util.GSet: Computing capacity for map BlocksMap
    19/04/19 15:48:02 INFO util.GSet: VM type       = 32-bit
    19/04/19 15:48:02 INFO util.GSet: 2.0% max memory = 932184064
    19/04/19 15:48:02 INFO util.GSet: capacity      = 2^22 = 4194304 entries
    19/04/19 15:48:02 INFO util.GSet: recommended=4194304, actual=4194304
    19/04/19 15:48:02 INFO namenode.FSNamesystem: fsOwner=hadoop
    19/04/19 15:48:02 INFO namenode.FSNamesystem: supergroup=supergroup
    19/04/19 15:48:02 INFO namenode.FSNamesystem: isPermissionEnabled=false
    19/04/19 15:48:02 INFO namenode.FSNamesystem: dfs.block.invalidate.limit=100
    19/04/19 15:48:02 INFO namenode.FSNamesystem: isAccessTokenEnabled=false accessKeyUpdateInterval=0 min(s), accessTokenLifetime=0 min(s)
    19/04/19 15:48:02 INFO namenode.FSEditLog: dfs.namenode.edits.toleration.length = 0
    19/04/19 15:48:02 INFO namenode.NameNode: Caching file names occuring more than 10 times 
    19/04/19 15:48:03 INFO common.Storage: Image file /opt/hadoop/hadoop-tmp-dir/dfs/name/current/fsimage of size 112 bytes saved in 0 seconds.
    19/04/19 15:48:03 INFO namenode.FSEditLog: closing edit log: position=4, editlog=/opt/hadoop/hadoop-tmp-dir/dfs/name/current/edits
    19/04/19 15:48:03 INFO namenode.FSEditLog: close success: truncate to 4, editlog=/opt/hadoop/hadoop-tmp-dir/dfs/name/current/edits
    19/04/19 15:48:03 INFO common.Storage: Storage directory /opt/hadoop/hadoop-tmp-dir/dfs/name has been successfully formatted.
    19/04/19 15:48:03 INFO namenode.NameNode: SHUTDOWN_MSG: 
    /************************************************************
    SHUTDOWN_MSG: Shutting down NameNode at edydr1p0.us.oracle.com/127.0.0.1
    ************************************************************/
    
      [hadoop@edydr1p0 ~]$ more $HADOOP_HOME/bin/start-all.sh 
      more 100%로 될때까지 엔터 누르기~!



    
      [hadoop@edydr1p0 ~]$ start-all.sh       // HDFS & 맵리듀스 모두 실행
    starting namenode, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-namenode-edydr1p0.us.oracle.com.out
    localhost: starting datanode, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-datanode-edydr1p0.us.oracle.com.out
    localhost: starting secondarynamenode, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-secondarynamenode-edydr1p0.us.oracle.com.out
    starting jobtracker, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-jobtracker-edydr1p0.us.oracle.com.out
    localhost: starting tasktracker, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-tasktracker-edydr1p0.us.oracle.com.out


      [hadoop@edydr1p0 ~]$ jps
    16450 NameNode
    17058 Jps
    16725 SecondaryNameNode
    16967 TaskTracker
    16814 JobTracker
    16574 DataNode

      [hadoop@edydr1p0 ~]$ 

<br>
<br>
<br>

## jps는 Java Virtual Machine Process Status Tool의 약자이며,
* 시스템에서 실행 중인 자바 프로세스를 출력합니다. 
###    
      [hadoop@edydr1p0 ~]$ jps            
    
      8981 NameNode
      9377 JobTracker
      9616 Jps
      9119 DataNode
      9267 SecondaryNameNode
      9511 TaskTracker
       ==> 만약 jps 명령의 결과가 적절하지 않다면 다음과 같이 삭제하고 압축 해제하는 부분부터 다시 하세요.

       [hadoop@edydr1p0 ~]$ cd /opt/hadoop

       [hadoop@edydr1p0 hadoop]$ ls
       hadoop        hadoop-1.2.1.tar.gz
       hadoop-1.2.1  hadoop-tmp-dir

       [hadoop@edydr1p0 hadoop]$ rm -rf hadoop-tmp-dir
       [hadoop@edydr1p0 hadoop]$ rm -rf hadoop-1.2.1 
       [hadoop@edydr1p0 hadoop]$ rm -rf hadoop

     * http://192.168.56.101:50070  : NameNode
       http://192.168.56.101:50030  : JobTracker

      [hadoop@edydr1p0 ~]$ stop-all.sh        // HDFS & 맵리듀스 모두 종료
    stopping jobtracker
    localhost: stopping tasktracker
    stopping namenode
    localhost: stopping datanode
    localhost: stopping secondarynamenode
###    
      [hadoop@edydr1p0 ~]$ start-dfs.sh       // HDFS만 실행
    starting namenode, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-namenode-edydr1p0.us.oracle.com.out
    localhost: starting datanode, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-datanode-edydr1p0.us.oracle.com.out
    localhost: starting secondarynamenode, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-secondarynamenode-edydr1p0.us.oracle.com.out

      [hadoop@edydr1p0 ~]$ jps                // NameNode 및 DataNode는 HDFS의 구성요소
    17539 NameNode
    17669 DataNode
    17845 SecondaryNameNode
    17900 Jps
    
      [hadoop@edydr1p0 ~]$ stop-dfs.sh        // HDFS만 종료
    stopping namenode
    localhost: stopping datanode
    localhost: stopping secondarynamenode
###    
      [hadoop@edydr1p0 ~]$ start-mapred.sh    // Map Reduce만 실행
    starting jobtracker, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-jobtracker-edydr1p0.us.oracle.com.out
    localhost: starting tasktracker, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-tasktracker-edydr1p0.us.oracle.com.out
    
      [hadoop@edydr1p0 ~]$ jps                // JobTracker 및 TaskTracker는 맵리듀스의 구성요소
    18160 JobTracker
    18313 TaskTracker
    18383 Jps
    
      [hadoop@edydr1p0 ~]$ stop-mapred.sh     // Map Reduce만 종료
    stopping jobtracker
    localhost: stopping tasktracker
###    
      [hadoop@edydr1p0 ~]$ start-all.sh 
    starting namenode, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-namenode-edydr1p0.us.oracle.com.out
    localhost: starting datanode, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-datanode-edydr1p0.us.oracle.com.out
    localhost: starting secondarynamenode, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-secondarynamenode-edydr1p0.us.oracle.com.out
    starting jobtracker, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-jobtracker-edydr1p0.us.oracle.com.out
    localhost: starting tasktracker, logging to /opt/hadoop/hadoop/logs/hadoop-hadoop-tasktracker-edydr1p0.us.oracle.com.out

<br>
<br>
<br>

## [2] Hadoop programming 예제
* WordCount 소스 분석 : http://me2.do/5Z4aYa6k
###
      [hadoop@edydr1p0 ~]$ cd
    
      [hadoop@edydr1p0 ~]$ hadoop fs -mkdir input
    
      [hadoop@edydr1p0 ~]$ hadoop fs -ls
      Found 1 items
      drwxr-xr-x   - hadoop supergroup          0 2019-04-19 17:09 /user/hadoop/input
  
      [hadoop@edydr1p0 ~]$ hadoop fs -du
      Found 1 items
      0           hdfs://192.168.56.101:9000/user/hadoop/input

      [hadoop@edydr1p0 ~]$ hadoop fs -put $HADOOP_HOME/README.txt input
    
      [hadoop@edydr1p0 ~]$ hadoop fs -ls
      Found 1 items
      drwxr-xr-x   - hadoop supergroup          0 2019-04-19 17:10 /user/hadoop/input
     
      [hadoop@edydr1p0 ~]$ hadoop fs -lsr
      drwxr-xr-x   - hadoop supergroup          0 2019-04-19 17:10 /user/hadoop/input
      -rw-r--r--   1 hadoop supergroup       1366 2019-04-19 17:10 /user/hadoop/input/README.txt

      [hadoop@edydr1p0 ~]$ hadoop fs -cat /user/hadoop/input/README.txt
    For the latest information about Hadoop, please visit our website at:
    
       http://hadoop.apache.org/core/
    
    and our wiki, at:
    
       http://wiki.apache.org/hadoop/
    
    This distribution includes cryptographic software.  The country in 
    which you currently reside may have restrictions on the import, 
    possession, use, and/or re-export to another country, of 
    encryption software.  BEFORE using any encryption software, please 
    check your country's laws, regulations and policies concerning the
    import, possession, or use, and re-export of encryption software, to 
    see if this is permitted.  See <http://www.wassenaar.org/> for more
    information.
    
    The U.S. Government Department of Commerce, Bureau of Industry and
    Security (BIS), has classified this software as Export Commodity 
    Control Number (ECCN) 5D002.C.1, which includes information security
    software using or performing cryptographic functions with asymmetric
    algorithms.  The form and manner of this Apache Software Foundation
    distribution makes it eligible for export under the License Exception
    ENC Technology Software Unrestricted (TSU) exception (see the BIS 
    Export Administration Regulations, Section 740.13) for both object 
    code and source code.
    
    The following provides more details on the included cryptographic
    software:
      Hadoop Core uses the SSL libraries from the Jetty project written 
    by mortbay.org.
    
      * Usage: hadoop jar <jar> [mainClass] args...
      * Usage: hadoop CLASSNAME args...
    
      [hadoop@edydr1p0 ~]$ ls $HADOOP_HOME/*.jar
    /opt/hadoop/hadoop/hadoop-ant-1.2.1.jar
    /opt/hadoop/hadoop/hadoop-client-1.2.1.jar
    /opt/hadoop/hadoop/hadoop-core-1.2.1.jar
    /opt/hadoop/hadoop/hadoop-examples-1.2.1.jar
    /opt/hadoop/hadoop/hadoop-minicluster-1.2.1.jar
    /opt/hadoop/hadoop/hadoop-test-1.2.1.jar
    /opt/hadoop/hadoop/hadoop-tools-1.2.1.jar

      [hadoop@edydr1p0 ~]$ jar -tf $HADOOP_HOME/hadoop-examples*    // jar 파일 내용 확인
    META-INF/
    META-INF/MANIFEST.MF
    org/
    org/apache/
    org/apache/hadoop/
    org/apache/hadoop/examples/
    org/apache/hadoop/examples/dancing/
    org/apache/hadoop/examples/terasort/
    org/apache/hadoop/examples/AggregateWordCount$WordCountPlugInClass.class
    org/apache/hadoop/examples/AggregateWordCount.class
    org/apache/hadoop/examples/AggregateWordHistogram$AggregateWordHistogramPlugin.class
    org/apache/hadoop/examples/AggregateWordHistogram.class
    org/apache/hadoop/examples/DBCountPageView$AccessRecord.class
    org/apache/hadoop/examples/DBCountPageView$PageviewMapper.class
    org/apache/hadoop/examples/DBCountPageView$PageviewRecord.class
    org/apache/hadoop/examples/DBCountPageView$PageviewReducer.class
    org/apache/hadoop/examples/DBCountPageView.class
    org/apache/hadoop/examples/ExampleDriver.class
    org/apache/hadoop/examples/Grep.class
    org/apache/hadoop/examples/Join.class
    org/apache/hadoop/examples/MultiFileWordCount$MapClass.class
    org/apache/hadoop/examples/MultiFileWordCount$MultiFileLineRecordReader.class
    org/apache/hadoop/examples/MultiFileWordCount$MyInputFormat.class
    org/apache/hadoop/examples/MultiFileWordCount$WordOffset.class
    org/apache/hadoop/examples/MultiFileWordCount.class
    org/apache/hadoop/examples/PiEstimator$HaltonSequence.class
    org/apache/hadoop/examples/PiEstimator$PiMapper.class
    org/apache/hadoop/examples/PiEstimator$PiReducer.class
    org/apache/hadoop/examples/PiEstimator.class
    org/apache/hadoop/examples/RandomTextWriter$Counters.class
    org/apache/hadoop/examples/RandomTextWriter$Map.class
    org/apache/hadoop/examples/RandomTextWriter.class
    org/apache/hadoop/examples/RandomWriter$Counters.class
    org/apache/hadoop/examples/RandomWriter$Map.class
    org/apache/hadoop/examples/RandomWriter$RandomInputFormat$RandomRecordReader.class
    org/apache/hadoop/examples/RandomWriter$RandomInputFormat.class
    org/apache/hadoop/examples/RandomWriter.class
    org/apache/hadoop/examples/SecondarySort$FirstGroupingComparator.class
    org/apache/hadoop/examples/SecondarySort$FirstPartitioner.class
    org/apache/hadoop/examples/SecondarySort$IntPair$Comparator.class
    org/apache/hadoop/examples/SecondarySort$IntPair.class
    org/apache/hadoop/examples/SecondarySort$MapClass.class
    org/apache/hadoop/examples/SecondarySort$Reduce.class
    org/apache/hadoop/examples/SecondarySort.class
    org/apache/hadoop/examples/SleepJob$EmptySplit.class
    org/apache/hadoop/examples/SleepJob$SleepInputFormat$1.class
    org/apache/hadoop/examples/SleepJob$SleepInputFormat.class
    org/apache/hadoop/examples/SleepJob.class
    org/apache/hadoop/examples/Sort.class
    org/apache/hadoop/examples/WordCount$IntSumReducer.class
    org/apache/hadoop/examples/WordCount$TokenizerMapper.class
    org/apache/hadoop/examples/WordCount.class
    org/apache/hadoop/examples/dancing/DancingLinks$ColumnHeader.class
    org/apache/hadoop/examples/dancing/DancingLinks$Node.class
    org/apache/hadoop/examples/dancing/DancingLinks$SolutionAcceptor.class
    org/apache/hadoop/examples/dancing/DancingLinks.class
    org/apache/hadoop/examples/dancing/DistributedPentomino$PentMap$SolutionCatcher.class
    org/apache/hadoop/examples/dancing/DistributedPentomino$PentMap.class
    org/apache/hadoop/examples/dancing/DistributedPentomino.class
    org/apache/hadoop/examples/dancing/OneSidedPentomino.class
    org/apache/hadoop/examples/dancing/Pentomino$ColumnName.class
    org/apache/hadoop/examples/dancing/Pentomino$Piece.class
    org/apache/hadoop/examples/dancing/Pentomino$Point.class
    org/apache/hadoop/examples/dancing/Pentomino$SolutionCategory.class
    org/apache/hadoop/examples/dancing/Pentomino$SolutionPrinter.class
    org/apache/hadoop/examples/dancing/Pentomino.class
    org/apache/hadoop/examples/dancing/Sudoku$CellConstraint.class
    org/apache/hadoop/examples/dancing/Sudoku$ColumnConstraint.class
    org/apache/hadoop/examples/dancing/Sudoku$ColumnName.class
    org/apache/hadoop/examples/dancing/Sudoku$RowConstraint.class
    org/apache/hadoop/examples/dancing/Sudoku$SolutionPrinter.class
    org/apache/hadoop/examples/dancing/Sudoku$SquareConstraint.class
    org/apache/hadoop/examples/dancing/Sudoku.class
    org/apache/hadoop/examples/terasort/TeraGen$RandomGenerator.class
    org/apache/hadoop/examples/terasort/TeraGen$RangeInputFormat$RangeInputSplit.class
    org/apache/hadoop/examples/terasort/TeraGen$RangeInputFormat$RangeRecordReader.class
    org/apache/hadoop/examples/terasort/TeraGen$RangeInputFormat.class
    org/apache/hadoop/examples/terasort/TeraGen$SortGenMapper.class
    org/apache/hadoop/examples/terasort/TeraGen.class
    org/apache/hadoop/examples/terasort/TeraInputFormat$TeraRecordReader.class
    org/apache/hadoop/examples/terasort/TeraInputFormat$TextSampler.class
    org/apache/hadoop/examples/terasort/TeraInputFormat.class
    org/apache/hadoop/examples/terasort/TeraOutputFormat$TeraRecordWriter.class
    org/apache/hadoop/examples/terasort/TeraOutputFormat.class
    org/apache/hadoop/examples/terasort/TeraSort$TotalOrderPartitioner$InnerTrieNode.class
    org/apache/hadoop/examples/terasort/TeraSort$TotalOrderPartitioner$LeafTrieNode.class
    org/apache/hadoop/examples/terasort/TeraSort$TotalOrderPartitioner$TrieNode.class
    org/apache/hadoop/examples/terasort/TeraSort$TotalOrderPartitioner.class
    org/apache/hadoop/examples/terasort/TeraSort.class
    org/apache/hadoop/examples/terasort/TeraValidate$ValidateMapper.class
    org/apache/hadoop/examples/terasort/TeraValidate$ValidateReducer.class
    org/apache/hadoop/examples/terasort/TeraValidate.class

      [hadoop@edydr1p0 ~]$ hadoop jar $HADOOP_HOME/hadoop-examples*.jar wordcount input/README.txt output1
    19/04/19 17:13:29 INFO input.FileInputFormat: Total input paths to process : 1
    19/04/19 17:13:29 INFO util.NativeCodeLoader: Loaded the native-hadoop library
    19/04/19 17:13:29 WARN snappy.LoadSnappy: Snappy native library not loaded
    19/04/19 17:13:30 INFO mapred.JobClient: Running job: job_201904191603_0001
    19/04/19 17:13:31 INFO mapred.JobClient:  map 0% reduce 0%
    19/04/19 17:13:37 INFO mapred.JobClient:  map 100% reduce 0%
    19/04/19 17:13:46 INFO mapred.JobClient:  map 100% reduce 33%
    19/04/19 17:13:48 INFO mapred.JobClient:  map 100% reduce 100%
    19/04/19 17:13:49 INFO mapred.JobClient: Job complete: job_201904191603_0001
    19/04/19 17:13:49 INFO mapred.JobClient: Counters: 29
    19/04/19 17:13:49 INFO mapred.JobClient:   Map-Reduce Framework
    19/04/19 17:13:49 INFO mapred.JobClient:     Spilled Records=262
    19/04/19 17:13:49 INFO mapred.JobClient:     Map output materialized bytes=1836
    19/04/19 17:13:49 INFO mapred.JobClient:     Reduce input records=131
    19/04/19 17:13:49 INFO mapred.JobClient:     Virtual memory (bytes) snapshot=1068228608
    19/04/19 17:13:49 INFO mapred.JobClient:     Map input records=31
    19/04/19 17:13:49 INFO mapred.JobClient:     SPLIT_RAW_BYTES=120
    19/04/19 17:13:49 INFO mapred.JobClient:     Map output bytes=2055
    19/04/19 17:13:49 INFO mapred.JobClient:     Reduce shuffle bytes=1836
    19/04/19 17:13:49 INFO mapred.JobClient:     Physical memory (bytes) snapshot=234520576
    19/04/19 17:13:49 INFO mapred.JobClient:     Reduce input groups=131
    19/04/19 17:13:49 INFO mapred.JobClient:     Combine output records=131
    19/04/19 17:13:49 INFO mapred.JobClient:     Reduce output records=131
    19/04/19 17:13:49 INFO mapred.JobClient:     Map output records=179
    19/04/19 17:13:49 INFO mapred.JobClient:     Combine input records=179
    19/04/19 17:13:49 INFO mapred.JobClient:     CPU time spent (ms)=1670
    19/04/19 17:13:49 INFO mapred.JobClient:     Total committed heap usage (bytes)=212598784
    19/04/19 17:13:49 INFO mapred.JobClient:   File Input Format Counters 
    19/04/19 17:13:49 INFO mapred.JobClient:     Bytes Read=1366
    19/04/19 17:13:49 INFO mapred.JobClient:   FileSystemCounters
    19/04/19 17:13:49 INFO mapred.JobClient:     HDFS_BYTES_READ=1486
    19/04/19 17:13:49 INFO mapred.JobClient:     FILE_BYTES_WRITTEN=119863
    19/04/19 17:13:49 INFO mapred.JobClient:     FILE_BYTES_READ=1836
    19/04/19 17:13:49 INFO mapred.JobClient:     HDFS_BYTES_WRITTEN=1306
    19/04/19 17:13:49 INFO mapred.JobClient:   Job Counters 
    19/04/19 17:13:49 INFO mapred.JobClient:     Launched map tasks=1
    19/04/19 17:13:49 INFO mapred.JobClient:     Launched reduce tasks=1
    19/04/19 17:13:49 INFO mapred.JobClient:     SLOTS_MILLIS_REDUCES=9512
    19/04/19 17:13:49 INFO mapred.JobClient:     Total time spent by all reduces waiting after reserving slots (ms)=0
    19/04/19 17:13:49 INFO mapred.JobClient:     SLOTS_MILLIS_MAPS=6037
    19/04/19 17:13:49 INFO mapred.JobClient:     Total time spent by all maps waiting after reserving slots (ms)=0
    19/04/19 17:13:49 INFO mapred.JobClient:     Data-local map tasks=1
    19/04/19 17:13:49 INFO mapred.JobClient:   File Output Format Counters 
    19/04/19 17:13:49 INFO mapred.JobClient:     Bytes Written=1306
    
      [hadoop@edydr1p0 ~]$ hadoop fs -lsr output1
    -rw-r--r--   1 hadoop supergroup          0 2019-04-19 17:13 /user/hadoop/output1/_SUCCESS
    drwxr-xr-x   - hadoop supergroup          0 2019-04-19 17:13 /user/hadoop/output1/_logs
    drwxr-xr-x   - hadoop supergroup          0 2019-04-19 17:13 /user/hadoop/output1/_logs/history
    -rw-r--r--   1 hadoop supergroup      13970 2019-04-19 17:13 /user/hadoop/output1/_logs/history/job_201904191603_0001_1555661610610_hadoop_word+count
    -rw-r--r--   1 hadoop supergroup      50400 2019-04-19 17:13 /user/hadoop/output1/_logs/history/job_201904191603_0001_conf.xml
    -rw-r--r--   1 hadoop supergroup       1306 2019-04-19 17:13 /user/hadoop/output1/part-r-00000

      [hadoop@edydr1p0 ~]$ hadoop fs -cat output1/part-r-00000
    (BIS),  1
    (ECCN)  1
    (TSU)   1
    (see    1
    5D002.C.1,      1
    740.13) 1
    <http://www.wassenaar.org/>     1
    Administration  1
    Apache  1
    BEFORE  1
    BIS     1
    Bureau  1
    Commerce,       1
    Commodity       1
    Control 1
    Core    1
    Department      1
    ENC     1
    Exception       1
    Export  2
    For     1
    Foundation      1
    Government      1
    Hadoop  1
    Hadoop, 1
    Industry        1
    Jetty   1
    License 1
    Number  1
    Regulations,    1
    SSL     1
    Section 1
    Security        1
    See     1
    Software        2
    Technology      1
    The     4
    This    1
    U.S.    1
    Unrestricted    1
    about   1
    algorithms.     1
    and     6
    and/or  1
    another 1
    any     1
    as      1
    asymmetric      1
    at:     2
    both    1
    by      1
    check   1
    classified      1
    code    1
    code.   1
    concerning      1
    country 1
    country's       1
    country,        1
    cryptographic   3
    currently       1
    details 1
    distribution    2
    eligible        1
    encryption      3
    exception       1
    export  1
    following       1
    for     3
    form    1
    from    1
    functions       1
    has     1
    have    1
    http://hadoop.apache.org/core/  1
    http://wiki.apache.org/hadoop/  1
    if      1
    import, 2
    in      1
    included        1
    includes        2
    information     2
    information.    1
    is      1
    it      1
    latest  1
    laws,   1
    libraries       1
    makes   1
    manner  1
    may     1
    more    2
    mortbay.org.    1
    object  1
    of      5
    on      2
    or      2
    our     2
    performing      1
    permitted.      1
    please  2
    policies        1
    possession,     2
    project 1
    provides        1
    re-export       2
    regulations     1
    reside  1
    restrictions    1
    security        1
    see     1
    software        2
    software,       2
    software.       2
    software:       1
    source  1
    the     8
    this    3
    to      2
    under   1
    use,    2
    uses    1
    using   2
    visit   1
    website 1
    which   2
    wiki,   1
    with    1
    written 1
    you     1
    your    1
    
      [hadoop@edydr1p0 ~]$ hadoop fs -rmr output*
    Deleted hdfs://192.168.56.101:9000/user/hadoop/output1
    
      [hadoop@edydr1p0 ~]$ hadoop fs -lsr
    drwxr-xr-x   - hadoop supergroup          0 2019-04-19 17:10 /user/hadoop/input
    -rw-r--r--   1 hadoop supergroup       1366 2019-04-19 17:10 /user/hadoop/input/README.txt    
      * hadoop performance tuning을 검색해 보세요.
    
      [hadoop@edydr1p0 ~]$ vi kor.txt
     
        원하는 내용 붙여넣기
    
      [hadoop@edydr1p0 ~]$ hadoop fs -put kor.txt input
    
      [hadoop@edydr1p0 ~]$ hadoop jar $HADOOP_HOME/hadoop-examples*.jar wordcount input/kor.txt output2
    
      [hadoop@edydr1p0 ~]$ hadoop fs -cat output2/part-r-00000
    
      [hadoop@edydr1p0 ~]$ hadoop fs -get output2/part-r-00000 .