---
categories: react
---

# 상태 관리 라이브러리 "Redux"

---

## 목차

---

1. [용어정리](#-용어-정리)
2. [관련 용어 정리](#-관련-용어-정리)
3. [왜 쓸까?](#-왜-쓸까)
4. [상태 변화 흐름1](#-상태-변화-흐름1)
5. [상태 변화 흐름2](#-상태-변화-흐름2)
6. [그외 정보](#-그외-정보)
7. [설치 사용법](#-설치-사용법)

---

## 🟣 용어 정리

---

### 1. 액션 (Action)

    - 상태에 변화가 필요하다면 액션을 일으켜야 한다.
    - 션은 객체로 표현되며 type 필드를 반드시 가지고 있어야한다.

### 2. 액션 타입

    - 상태에 어떠한 변화가 예상될 때는 액션을 발생
    - swith에 들어가는 값

### 3. 액션 생성함수(Action Creator)

    - 액션 객체를 만들어주는 함수이다.
    - 실제로 컴포넌트에서 디스패치 되는 함수
    - type값이 필수적으로 있어야한다.

### 4. 리듀서 (Reducer)

    - 현재 상태와 액션 객체를 받아 필요하다면 새로운 상태를 리턴하는 함수이다.
    - 액션 유형을 기반으로 이벤트 처리하는 이벤트 리스너
    - 여러개의 리듀서를 만들고 이를 합쳐 루트 리듀서를 만들 수 있다.
    - 스테이트 수정 방법을 다 정의
    - 스토어에 만듬
    - pi를 만드는 것과 동일하다.

### 5. 스토어 (Store)

    - 상태가 들어있다.
    - 현재의 앱 상태와 리듀서를 스토어에 만든다.
    - 하나의 프로젝트에는 하나의 스토어만 가질 수 있음.

### 6. 디스패치 (Dispatch)

    - 액션 객체를 넘겨줘서 상태를 업데이트 하는 유일한 방법
    - 액션을 발생 시키는 것
    - 리듀서를 dispatch로 가져와 사용

### 7. 구독 (Subscribe)

    - 파라미터에 리스너 함수를 넣어 호출하면 상태가 업데이트될 때마다 호출
    - 액션이 디스패치 되었을때마다 전달해준 함수가 호출
    - 구독함수를 직접 사용하는 일은 별로 없다.
    - react-deux 라이브러리에서 제공하는 connect함수 사용

### 8. 셀렉터 (Selector)

    - 상태 값을 가져오기 위해 getState 사용

### 9. 프리젠테이셔널 컴포넌트

    - 리덕스 스토어에 직접 접근 하지 않고 필요한 값 또는 함수를 props로 만 받아와서 사용하는 컴포넌트이다.
    - 보통 components 디렉터리에 생성
    - 리덕스 기능이 없는 컴포넌트
    - 부품으로써 여러곳에 사용 가능

### 10. 컨테이너 컴포넌트

    - 리덕스 스토어의 상태를 조회하거나, 액션을 디스패치 할 수 있는 컴포넌트이다.
    - 다른 프리젠테이셔널 컴포넌트들을 불러와서 사용한다.
    - 보통 components 디렉터리에 생성
    - 다른곳에서 재사용 불가

### 11. 리덕스 모듈

    - 액션타입, 액션 생성함수, 리듀서가 포함된 파일
    - 리듀서와 액션관련 코드를 하나의 파일로 몰아서 개발하는 방식을 Ducks 패턴이라 한다.

---

## 🟣 관련 용어 정리

---

### 1. Ducks 패턴

    - 기능중심으로 파일을 나눈다.
    - 단일기능을 작성할때나 기능을 수정할 때 하나의 파일만 다루면 되므로 직관적인 코드작성이 가능
    - action type, action생성자 함수, saga, reducer를 하나의 파일에서 관리= 리덕스 모듈
    - 각각의 액션/ 액션함수/ 리듀서를 모아둔 것을 module이라고 부른다.

---

## 🟣 왜 쓸까?

---

    - props 문법 귀찮을 때
    - 컴포넌트가 중첩 되어있을 때 하위에 계속해서 state를 props로 전달한다.
    - 스토어를 만들어놓고
    - 스테이트를 직접 꺼내 쓸 수 있다.
    - Props로 전달하는게 소문이라면
    - 스토어를 만들어놓고 사용하는 건 "언론사"를 두는 것과 같다.
    - 컨테이너 컴포넌트와 리듀서를 작성할 때 "타입 체킹" 뿐만 아니라 "자동완성"이 - 므로 생산성이 매우 향상 된다.
    - 기존 리액트 코드 : 각각의 컴포넌트들이 props로 길게 연결 되어있음
    - 리덕스 활용한 코드 : 리덕스를 이용하게 되면 스토어를 이용하는데 그 안에는 state값을 저장해 각각에 필요한 컴포넌트들을 그 스토어에 구독을 시킨다.

---

## 🟣 상태 변화 흐름1

---

    1. 유저가 버튼을 클릭
    2. 유저가 클릭한 버튼에 맞는 디스패치를 실행해 액션을 일으킴
    3. 이전상태와 현재 액션으로 리듀서 함수를 스토어에서 실행, 그 값을 새로운 상태로 저장
    4. 스토어를 구독하고 있던 UI들에게 업데이트 알림
    5. 스토어의 데이터가 필요한 UI들은 필요한 상태가 업데이트 되었는지 확인
    6. 데이터 변경된 것을 업데이트 하여 화면에 표시

---

## 🟣 상태 변화 흐름2

---

    1. 컴포넌트에 변화가 생길때 구독중인 모든 컴포넌트들에게 변화 사실 통보
    2. Store을 생성하고 createStore api를 활용하여 store을 만듬
    3. 첫번째 인자에 함수를 주게 되는데 이를 리듀서라고 함
    4. 리듀서는 두개의 인자를 받는데
       - state는 데이터
       - action 가해져야하는 행위
       - 리턴값은 state
    5. 액션에 디스패치 해줘야함
    6. Strore를 import해서 store.dispatch를 통해서 타입을 지정
    7. 디스패치를 수령하는 store 쪽에서 반영하고자 하는 컴포넌트에 store.getState()로 값을 가져온다.
    8. store 값이 바뀌게 되면 변경된 사실을 통보하기 위해 state값을 변경
    9. 이때 랜더메서드는 재 호출 된다.
    10. Store.subscribe 함수로 구독을 설정

---

## 🟣 그외 정보

---

    - 리덕스의 경우 자체적으로 typescript가 지원되기 때문에 따로 typescript 버전을 받을 필요 없음.
    - Immutable, redux는 Typescript 지원이 내장되어있다.
    - .ts 과 .tsx 차이점 : .tsx는 react 문법을 담아내기 위한 확장자로써 .jsx에 대응된다.

---

## 🟣 설치 사용법

---

    Redux 라이브러리 설치
    - Yarn add redux
