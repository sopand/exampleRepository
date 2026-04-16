# React Setting

1. Node 설치
2. NPM 설치
3. Yarn 설치
    - npm install --global yarn
    - yarn -v
4. CRA ( Create-React-App )전역으로 설치 - 리액트 프로젝트에 필요한 개발 환경 세팅
    - yarn add -g create-react-app
5. 새 React 프로젝트 생성
    - npx create-react-app 프로젝트 명  ( WebPack으로 실행 )
        - 소스 코드와 모든 종속 관계의 번들링 후 서버가 준비 ( 다소 느림 ) - 개발 단계
        - 각 모듈을 범위마다 함수로 매핑하여 결합 - 프로덕션 단계
    - npm init vite  ( Vite로 실행 )    
        - EsBuild로 미리 번들링한 모듈을 필요할 때 동적으로 가져와 서버가 즉각 구동 - 개발 단계
        - 하나의 파일에 모든 종속 모듈을 전역 범위로 선언해 결합, 중복을 제거하여 가볍고 빠르게 빌드 - 프로덕션 단계



