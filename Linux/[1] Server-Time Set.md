# Linux Server TimeZone Setting


- 시간대를 변경하기 위해서는 사용하려는 시간대의 이름을 찾아야한다.
  - 표준 시간대 명명 규칙은 일반적으로 "지역/도시"형식을 사용
  - 사용 가능한 모든 시간대를 보려면  timedatectl 명령을 사용하거나 /usr/share/zoneinfo 디렉토리에 있는 파일을 나열
  - timedatectl list-timezones
  - sudo timedatectl set-timezone Asia/Seoul  

#
#
- 위의 명령어로 변경되지 않는다면 etc/localtime을 /usr/share/zoneinfo 디렉토리의 시간대에 심볼릭 링크하여 시간대를 변경할 수 있습니다.
  현재 심볼릭 링크 또는 파일을 제거합니다.
  - sudo rm -rf /etc/localtime
- 구성하려는 시간대를 식별하고 symlink를 생성
  - sudo ln -s /usr/share/zoneinfo/Asia/Seoul /etc/localtime
- /etc/localtime 파일을 나열하거나 date 명령어로 변경된 시간을 확인
  - date

