# Setting Network
> Oracle 소프트웨어 설치 : 11gWS1 교재 3장<br>

> 개발환경<br> 
> OS : Macbook Pro, macOS Mojave<br>
> VirtualBox : Version 6.0.4<br>
> 가상환경 : Enterprise Linux 

## 네트워크 세팅
* 가상환경 시작시 네트워크 오류 발생
<img width="442" alt="Screen Shot 2019-04-05 at 10 08 01 AM" src="https://user-images.githubusercontent.com/46523571/55634096-b6afef00-57f8-11e9-8a59-a2dc55b8b78c.png">

* Tools - 3줄짜리 버튼 - network 클릭
<img width="838" alt="Screen Shot 2019-04-05 at 11 00 56 AM" src="https://user-images.githubusercontent.com/46523571/55634097-b6afef00-57f8-11e9-8224-c11279e3ad3a.png">

* HOST 설정 Adapter 설정
  - IPv4 Address : 192.168.56.1
  - IPv4 Network Mask : 255.255.255.0
<img width="841" alt="Screen Shot 2019-04-05 at 11 01 09 AM" src="https://user-images.githubusercontent.com/46523571/55634098-b7488580-57f8-11e9-8900-82cbe6c1960c.png">

* HOST 설정 DHCP Server 설정
  - Server Address : 192.168.56.100
  - Server Mask : 255.255.255.0
  - Lower Address Bound : 192.168.56.101
  - Upper Address Bound : 192.168.56.254
<img width="838" alt="Screen Shot 2019-04-05 at 11 01 17 AM" src="https://user-images.githubusercontent.com/46523571/55634100-b7488580-57f8-11e9-828f-6dce4531a8c8.png">

* VirtualBox - Preferences - Newtwork - Adds New NAT network 버튼 클릭
  - Network Name : NatNetwork
  - Network CIDR : 10.0.2.0/24
  - Network Options : Supports DHCP 체크  
<img width="839" alt="Screen Shot 2019-04-05 at 11 01 45 AM" src="https://user-images.githubusercontent.com/46523571/55634101-b7488580-57f8-11e9-9730-88a612dd18ae.png">
<img width="838" alt="Screen Shot 2019-04-05 at 11 02 00 AM" src="https://user-images.githubusercontent.com/46523571/55634103-b7488580-57f8-11e9-8be0-f8ee863ad38f.png">

* 가상환경 구동 성공
<img width="1213" alt="Screen Shot 2019-04-05 at 11 38 21 AM" src="https://user-images.githubusercontent.com/46523571/55634104-b7e11c00-57f8-11e9-84f3-db5ee6e7ec6a.png">

















