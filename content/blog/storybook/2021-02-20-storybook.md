---
title: '스토리북'
date: 2021-02-20 17:21:13
category: 'storybook'
draft: false
---

프론트엔드 개발자의 업무효율을 개선시켜주는 스토리북, 그에 따른 디자인 시스템등 말로만 들어왔던 스토리북을 학습하며
그 과정을 기록해 보려고합니다.

## 디자인 시스템을 이해하기

- Style Guide
  스타일 가이드란 특정 브랜드 또는 상품에서 디자인을 할 때 지켜야 하는 규칙들로 이루어져 있습니다.

  - 색상

  * 아이콘
  * 여백
  * 타이포그래피

  꼭 UI에만 해당하는 것은 아니고 디자인 할 때 전역적으로 적용되는 부분입니다.

- Component Library
  컴포넌트 라이브러리는 재사용 가능한 컴포넌트들의 모음들로 이루어져 있습니다.
  - Buton
  - Modal
  - ToggleSwitch
  - Card
  - Checkbox

디자인 시스템은 위의 스타일 가이드와 컴포넌트의 복합체입니다.  
디자인 시스템의 정의는 사람에따라, 기업에 따라 다른 정의를 가지고 있을 수 있습니다.  
하지만 공통점은 디자인과 개발의 생산성을 높여주기 위한 문서화된 가이드 라는것입니다.  
문서화가 되어있지 않다면 디자인 시스템이라고 부를 수 없는데, 이처럼 중요한 문서화를
하기 위해 사용하는 도구가 스토리북입니다.

## 스토리북 시작하기

우선 프로젝트를 생성합니다.

```
mkdir storybook-tutorial
cd storybook-tutorial
npm init -y
npx -p @storybook/cli sb init --type react
```

위와같이 새 프로젝트를 생성하는것이 아닌 CRA로 만든 프로젝트에서도 적용이 가능합니다.

```
npm i react react-dom
```

react 와 react-dom 패키지를 설치합니다.

스토리북 서버 실행

```
npm run storybook
```

그 후 스토리북 서버를 실행하고 localhost:6006에 접속하면 스토리북 화면이 나옵니다.

프로젝트에 stories 디렉토리를 src하위에서 관리하기 위해 src 디렉토리를 생성하고 그 안으로 stories 디렉토리를 옮깁니다.  
디렉토리를 바꾸게 되면 .storybook 디렉토리의 main.js의 설정 경로와 달라지게 됩니다.  
경로를 바뀐 경로로 변경해줍니다.

```js
//.storybook/main.js
module.exports = {
  stories: ['../src/**/*.stories.mdx', '../src/**/*.stories.@(js|jsx|ts|tsx)'],
  addons: ['@storybook/addon-links', '@storybook/addon-essentials'],
}
```

## 컴포넌트와 스토리 만들기

src 에 Hello라는 디렉토리를 만들고 Hello.js파일을 생성합니다.

```jsx
// src/Hello/Hello.js
import React from 'react'

function Hello({ name, big }) {
  if (big) {
    return <h1>안녕하세요, {name}</h1>
  }
  return <p>안녕하세요, {name}</p>
}

export default Hello
```

컴포넌트를 작성한 뒤, 스토리를 만듭니다. 스토리는 .stories.js확장자로 작성합니다.  
스토리를 작성할 때는 Component Story Format(CSF)형식을 사용합니다.  
그 후 스토리도 작성해줍니다.

```jsx
// src/Hello/Hello.stories.js
import React from 'react'
import Hello from './Hello'

export default {
  // 스토리북에서 보여질 그룹과 경로
  title: 'components/basic/Hello',
  // 어떤 컴포넌트를 문서화 할지
  component: Hello,
}

export const standard = () => <Hello name="재영스토리북" />
export const big = () => <Hello name="재영스토리북" big />
```

위와 같이 컴포넌트와 스토리를 만들어주고 나면, Storybook에서 Components/basic/hello 라는 스토리가 생기게 되고,
그 하위에 Standard 와 Big의 스토리가 보이게 됩니다.  
주석의 내용대로 export default 를 통해 내보낼 때 title 값은 스토리북에서 보여지는 그룹과 경로를 나타내고,
component는 해당 컴포넌트를 가리킵니다.

## Knobs 애드온 적용

Knobs 애드온은 컴포넌트의 props를 스토리북 화면에서 바꾸고 바로 반영시킬 수 있게 해줍니다.

패키지 설치

```
npm i --save-dev @storybook/addon-knobs
```

패키지를 설치한 뒤, .main.js 에 addob 추가해줍니다.

```js
// .storybook/main.js
module.exports = {
  stories: ['../src/**/*.stories.mdx', '../src/**/*.stories.@(js|jsx|ts|tsx)'],
  addons: [
    '@storybook/addon-links',
    '@storybook/addon-essentials',
    '@storybook/addon-knobs',
  ],
}
```

그 후 Hello.stories.js 에 knobs애드온을 적용합니다.

```jsx
// Hello/Hello.stories.js
...
import { withKnobs } from '@storybook/addon-knobs';
export default {
  title: 'components/basic/Hello', // 스토리북에서 보여질 그룹과 경로
  component: Hello, // 어떤 컴포넌트를 문서화 할지
  decorators: [withKnobs], // 애드온 적용
};

```

그 후 knobs를 만들어 줍니다.

```jsx
// Hello/Hello.stories.js
...
import { boolean, text, withKnobs } from '@storybook/addon-knobs';

export const hello = () => {
  // knobs 만들기
  const big = boolean('big', false);
  const name = text('name', '재영스토리북');
  return <Hello name={name} big={big} />;
};

hello.story = {
  name: 'Default',
};
```
