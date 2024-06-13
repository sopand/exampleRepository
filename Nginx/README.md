# Nginx Error Note
[1.AWS EC2 LocalHost 이슈](#1.AWS EC2 LocalHost 이슈)

### 1.AWS EC2 LocalHost 이슈
- AWS EC2의 기본 Bridge 게이트웨이가 172.17.0.1이기 때문에 별도의 설정을 해주지 않으면 localhost로 부를 경우 Nginx는 찾을 수 없다.
- 그래서 localhost 대신 172.17.0.1을 기입해주면 알아서 찾아감

