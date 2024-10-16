# Sass , SCSS

### Sass
> - Sass는 컴파일 과정을 통하여 CSS 파일을 생성해주는 CSS의 확장 언어이자, 전처리기 ( preprocessor )로써,
> CSS에는 존재하지 않는 다양한 기능들을 가지고있다.
> - 이 기능들은 코드 작성에 드는 시간을 줄여주고, 코드를 유지 관리하는데 도움이 된다.
> - Sass는 CSS의 대체언어가 아니라 CSS의 확장언어이고, 이는 결국 CSS 코드를 생산해내기 위한 도구라는 뜻
> - Sass가 제공하는 문법을 기반으로 코드를 작성한 후 , 이를 컴파일해 CSS파일을 빌드하는 것

### SCSS
> - Sass는 코드를 CSS로 해석하는 전처리기로써의 의미와 문법(구문)으로써의 의미를 가지고 있다.
> - Sass 문법을 기반으로 코드를 작성하면, Sass전처리기의 도움을 받아 CSS 파일을 만들어낼 수 있다.
> - 그럼 SCSS란 SCSS 문법을 기반으로 코드를 작성하면, Sass 전처리기의 도움을 받아 컴파일러가 이를 CSS로 빌드해주는것
> - 기존 Sass 구문에 비해 CSS코드와 유사한 형태를 가지고 있다.
> - 둘은 동일한 기능을 가지고 있어 어느것을 사용할지는 사용자의 선택



### Vue SCSS 적용
1. 설치
   - $ npm install sass --save
   - $ yarn add sass
2. 사용 ( 특정 파일 )
```scss
<style lang="scss">
$backgroundColor: red;

.className: {
background: $backgroundColor;
}
</style>
 ```

3. 전역 사용
```scss
<style lang="scss">
    @import "@/style/variables";
    @import "@/style/mixin";
    @import "@/style/functions";
</style>
```

4. config 설정
```vue
// vue.config.js

module.exports = {
  css: {
    loaderOptions: {
      sass: {
        data: `
          @import "@/styles/_variables.scss";
          @import "@/styles/_mixins.scss";
        `
      }
    }
  }
} 
```