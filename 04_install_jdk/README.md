# Install Java JDK on Linux on VirtualBox on macOS
> 개발환경<br> 
> OS : Macbook Pro, macOS Mojave<br>
> VirtualBox : Version 6.0.4<br>
> 가상환경 : Enterprise Linux<br>
> JDK : jdk1.8.0_211<br>

## 가상환경 접속
* Username : 사용자명 입력
* Password : 비밀번호 입력
<img width="721" alt="Screen Shot 2019-04-05 at 3 07 44 PM" src="https://user-images.githubusercontent.com/46523571/55633697-e3173b80-57f7-11e9-8c31-d95dcbbf0b61.png">
<img width="719" alt="Screen Shot 2019-04-05 at 3 07 55 PM" src="https://user-images.githubusercontent.com/46523571/55633699-e3173b80-57f7-11e9-9953-7491430888cf.png">

<br>
<br>
<br>

## jdk 확인
      1. 32bit/64bit 확인
      [orcl:~]$ getconf LONG_BIT
      32
      
      2. 사용자 확인
      [orcl:~]$ whoami
      oracle
      
      3. 자바 버전 확인
      [orcl:~]$ java -version
    
      java version "1.6.0_18"
      Java(TM) SE Runtime Environment (build 1.6.0_18-b07)
      Java HotSpot(TM) Server VM (build 16.0-b13, mixed mode)
      
      4. 설치된 자바 위치 확인
      [orcl:~]$ which java
      /usr/java/jdk1.6.0_18/bin/java

<img width="573" alt="Screen Shot 2019-04-21 at 10 08 50 PM" src="https://user-images.githubusercontent.com/46523571/56470536-173e5d80-6482-11e9-81b2-54550ce554aa.png">

<br>
<br>
<br>

## 최신버전 JDK 다운로드 및 설치
      http://www.oracle.com/technetwork/java/javase/downloads/index.html 에서 최신 jdk를 다운로드하세요.
      >> jdk1.8.0_211 다운로드하는게 실습에 에러가 적다!
         jdk-*-linux-i586.tar.gz
    
      [orcl:~]$ su -
      Password: oracle   <- 보이지 않습니다.
    
      [root@edydr1p0 ~]# ls /home/oracle/Desktop
    
      [root@edydr1p0 ~]# mv /home/oracle/Desktop/jdk* /usr/java
      
      [root@edydr1p0 ~]# cd /usr/java
    
      [root@edydr1p0 java]# ls -l
    
      [root@edydr1p0 java]# chmod 755 jdk-*-linux-i586.tar.gz
    
      [root@edydr1p0 java]# ls -l
    
      [root@edydr1p0 java]# tar xvfz jdk-*-linux-i586.tar.gz
    
      [root@edydr1p0 java]# exit
      
<br>
<br>
<br>

## 자바 환경 변수 설정      
      [orcl:~]$ cd
      [orcl:~]$ whoami
      oracle
    
      [orcl:~]$ vi .bash_profile
    
      # JAVA_HOME 환경 변수의 값을 적절하게 바꾸세요.
    
        export JAVA_HOME=/usr/java/jdk1.8.0_211
    
      [orcl:~]$ . .bash_profile
      [orcl:~]$ java -version 
    
      java version "1.8.0_211"
      Java(TM) SE Runtime Environment (build 1.8.0_211-b12)
      Java HotSpot(TM) Server VM (build 25.121-b13, mixed mode)

<img width="574" alt="Screen Shot 2019-04-21 at 10 06 45 PM" src="https://user-images.githubusercontent.com/46523571/56470495-c890c380-6481-11e9-81db-2a0d10d03050.png">
<img width="574" alt="Screen Shot 2019-04-21 at 10 09 00 PM" src="https://user-images.githubusercontent.com/46523571/56470535-173e5d80-6482-11e9-8e08-d552f4c4792f.png">
   
<br>
<br>
<br>
 
## 자바 연습
      [orcl:~]$ vi HelloWorldApp.java
    
        class HelloWorldApp {
            public static void main(String[] args) {
                System.out.println("Hello World!"); // Display the string.
            }
        }
    
      [orcl:~]$ javac HelloWorldApp.java
     
      [orcl:~]$ java HelloWorldApp
      Hello World!
     
      [orcl:~]$ v
<img width="304" alt="Screen Shot 2019-04-19 at 3 21 55 PM" src="https://user-images.githubusercontent.com/46523571/56410155-13aaab00-62b7-11e9-9c30-b94c60f12e2c.png">



