# 06-Parcel & ESLint

* Parcel ( Bundler - Build Tool 특별한 설정 없이 바로 사용 가능한 빌드 도구 )
  * ES Module을 적극 활용하는 Vite도 굉장히 빠름 - 실제 사용해 보았을 때 대략 7배 차이를 보았음.
    * 프로젝트의 규모나 설정에 따라 빌드속도가 다르므로 본인의 프로젝트에서 확인이 필요합니다.
  * 예시 - image를 static폴더에 넣어 사용해보기&#x20;
    * &#x20; { "extends": \["@parcel/config-default"], "reporters": \["...", "parcel-reporter-static-files-copy"] }&#x20;
  * npx parcel build ( 빌드 ) -> npx servor ./dist ( build된 폴더 dist 에서 정적 서버를 실행)
* Lint ( ESLint )
  * 사용하는 이유
    1. 스타일 통일(협업시 유용)
    2. 잠재적 문제 발견
    3. 베스트 프랙티스 추천 -> 최신 트랜드를 학습하는데 활용할 수 있다.
