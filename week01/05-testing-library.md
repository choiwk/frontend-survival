# 05-Testing Library

4\. Testing Library

### Jest

거의 모든 것을 갖춘 테스팅 도구.

Mocha와 Chai처럼 RSpec의 describe-it을 지원하고, expect로 단언(assertion)할 수 있다.

Mocking도 다양한 레벨에서 쉽게 사용할 수 있다.

**기본적인 테스트 코드**

```js
test('add', () => {
  expect(add(1, 2)).toBe(3);
});
```

**BDD 스타일의 테스트코드**

```js
describe('add', () => {
  it('returns sum of two numbers', () => {
    expect(add(1, 2)).toBe(3);
  });
});
```

### Given-When-Then

의도한 것과 다른 일이 벌어졌을 때, 버그가 발생했다고 한다.

명세와 다른 프로그램 또는 명세가 불완전해서 알 수 없는 방식으로 작동하는 프로그램이 버그가 발생하는 프로그램이다.

올바른 명세가 있다면, 그걸 확인하는 테스트 코드를 작성하는 건 쉽다.

### 무슨 일이 벌어지나

예를 들어보자.

> 전자렌지에 3분만 돌리면 완성!

여기서 우리가 해야 하는 건 뭔가요?

> 전자렌지에 3분만 돌리면

그럼 무슨 일이 벌어지죠?

> 완성!

```ruby
ruby로 된 간단한 코드

# When
food = cook(3.minutes)

# Then
assert food.complete?`
```

### 준비가 필요합니다

그러고 보니 전자렌지를 먼저 준비해야함

> 700W 전자렌지를 준비해서 3분만 돌리면 완성!

모호함이 하나 줄어들었다.

```ruby
# Given
stove = Stove.new(700.watts)

# When
food = stove.cook(3.minutes)

# Then
assert food.complete?`
```

### 상황을 더 눈에 띄게

> Given - 700와트 전자렌지를 준비하고, 3분이란 시간이 주어졌을 때When - 주어진 시간동안 전자렌지를 돌리면Then - 요리가 완성된다.

```ruby
# Given
power = 700.watts
time = 3.minutes
stove = Stove.new(power)

# When
food = stove.cook(time)

# Then
assert food.complete?`
```

### 태세 전환

만약 1분만 돌리면 어떻게 될까? 완성 안된다. 5분 돌리면? 탄다.

### 정리하며

하려는 작업을 올바르게 이해하고 있는지가 관건.

Given-When-Then 개념을 이용해 테스트코드를 짠다.

[코딩을 하기전에](https://www.youtube.com/watch?v=N4FV788fNiQ)

[내가 한 일 증명하기](https://www.youtube.com/watch?v=wd8OmjB\_eUI)

\


### React Testing Library

[React Testing Library 공식문서](https://testing-library.com/docs/react-testing-library/intro/)

[jest-dom](https://testing-library.com/docs/ecosystem-jest-dom/)

[실습](https://github.com/heyho00/env-setting/pull/1/files)

UI 테스트에 특화된 라이브러리. 거의 E2E Test처럼 쓸 수 있다. 브라우저에서 사용자가 쓰듯이 테스트.

“F/E 테스트 = only React 컴포넌트 테스트”가 되는 상황은 최대한 피하는 게 좋다.

화면을 검사하는것보다 상태를 검사하는게 나을 수 있기 때문에 리액트 컴포넌트 뷰만 테스트 하는 상황이 좋은 건 아니다.

본질에 집중하지 못하고 너무 많은 테스트 코드를 작성할 위험이 있다.

유지보수를 돕기 위해 테스트 코드를 작성하는데, 테스트 코드를 잘못 작성하면 오히려 유지보수를 저해할 수 있다.

[프론트엔드도 테스트 해야 하나?](https://www.youtube.com/watch?v=-kUmsKRmOnA)\
