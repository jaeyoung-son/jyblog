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

default라는 이름으로 Knobs를 사용하는 새 스토리를 만들었는데, 기본적으로 export const를 사용해서 default라는
이름으로 내보내려면 키워드가 충돌하기 때문에 내보낼 수 없습니다.
그 대신 스토리를 만들고 해당 스토리의 멤버 변수로 story 객체를 설정해서 이름을 변경할 수 있습니다.

위와같이 적용하면 스토리북에서 하단 Actionds 옆에 Knobs탭이 생기게되고, big Props를 전달하는 토글 버튼과
name값을 설정하는 text입력창이 생기게 됩니다.

## Knobs의 종류들

- text: 텍스트를 입력합니다.
- boolean: true/false 값을 체크박스로 설정합니다.
- number: 숫자를 입력 할 수 있습니다. (1~10 처럼간격설정 가능)
- color: 컬러 팔레트를 통해 색상설정을 할 수 있습니다.
- object: JSON 형태로 객체 또는 배열을 설정 할 수 있습니다.
- array: 쉼표로 구분된 텍스트 형태로 배열을 설정 할 수 있습니다.
- select: 셀렉트 박스를 통해 여러 옵션 중 하나를 선택 할 수있습니다.
- radios: 라디오 버튼을 통해 여러 옵션 중 하나를 선택 할 수 있습니다.
- options: 여러가지 옵션을 선택하는 UI를 커스터마이징 할 수 있습니다.(radio, inline-radio, check, select ...)
- files: 파일을 선택할 수 있습니다.
- date: 날짜를 선택할 수 있습니다.
- button: 특정 함수를 실행하게 하는 버튼을 만들 수 있습니다.

Knobs를 사용 할 때 넣어야 하는 인자는 Knobs의 이름, 기본값, group id 가 있습니다. 그룹아이디는 생략이 가능합니다.

```js
const big = boolean('big', false, 'Group 1')
```

위처럼 그룹을 설정해주면 스토리북에서 Knobs를 클릭했을때 그룹이 분류되어 나타납니다.

## Actions 애드온 적용

Actions 애드온은 컴포넌트를 통해 특정 함수가 호출됐을때 어떤 함수가 호출됐는지, 어떤 매개변수를 넣어서 호출했는지에 대한
정보를 확인할 수 있게 해주는 애드온 입니다.  
리액트 라우터의 주소가 변경될 때나, 리덕스 스토어의 디스패치가 발생할 때 디스패치 되는
액션의 정보를 보는것도 가능합니다.  
설치는 첫 프로젝트 셋팅에 storybook CLI를 사용했는데, 이때는 별도로 설치할 필요가 없습니다.  
Hello 컴포넌트에 함수 적용

```jsx
// Hello/Hello.js
function Hello({ name, big, onHello, onBye }) {
  return (
    <div>
      {big ? <h1>안녕하세요, {name}</h1> : <p>안녕하세요, {name}</p>}
      <div>
        <button onClick={onHello}>Hello</button>
        <button onClick={onBye}>Bye</button>
      </div>
    </div>
  )
}

export default Hello
```

onHello 와 onBye 함수를 props로 전달받아 button을 만들어 클릭 이벤트를 등록해줍니다.  
그 후 Hello의 스토리 파일을 수정합니다.

```jsx
// Hello/Hello.stories.js
...
import { action } from '@storybook/addon-actions';
...
export const hello = () => {
  // knobs 만들기
  const big = boolean('big', false);
  const name = text('name', '재영스토리북');
  return (
    <Hello
      name={name}
      big={big}
      onHello={action('onHello')}
      onBye={action('onBye')}
    />
  );
};

```

새로운 액션을 만들 땐 action('액션이름')으로 작성합니다.  
스토리북을 확인해보면 버튼 클릭 시 Actions탭에 발생한 액션들이 잘 나타나고 있습니다.  
각 함수를 Hello에 props로 전달할때의 값을 함수호출로 넘겨주는데 헷갈릴 수 있으니 유의해야 겠습니다.

## Docs 애드온 적용

Docs 애드온은 MDX(파일형식) 형식으로 문서를 작성할 수 있게 해주고, 컴포넌트의 props와 주석에 기반하여
자동으로 문서를 생성해줍니다.  
우선 애드온 패키지를 설치합니다.

```
npm install --save-dev @storybook/addon-docs
```

그 후 .storybook/main.js 에 애드온을 추가하고 mdx확장자도 처리하도록 정규식을 수정합니다.

```js
module.exports = {
  stories: [
    '../src/**/*.stories.mdx',
    '../src/**/*.stories.@(js|jsx|ts|tsx|mdx)',
  ],
  addons: [
    '@storybook/addon-links',
    '@storybook/addon-essentials',
    '@storybook/addon-knobs',
    '@storybook/addon-docs',
  ],
}
```

그 후 스토리북에서 Canvans 옆에 Docs 탭을 클릭해보면,
No inputs found for this component.
Read the docs 라는 메시지가 나옵니다.

### Props 문서화 하기

Props를 채우기 위해 props-types 또는 타입스크립트를 사용하면 그에 기반해서 자동으로 props table을 만들어줍니다.
deafaultProps도 지정해주면 이를 추출해서 보여줄 수 있습니다.

```jsx
import PropTypes from 'prop-types';
...
Hello.propTypes = {
  // 보여주고 싶은 이름
  name: PropTypes.string.isRequired,
  // 값이 true면 h1태그로 렌더링
  big: PropTypes.bool,
  // Hello 버튼 클릭 시 호출 함수
  onHello: PropTypes.func,
  // Bye 버튼 클릭 시 호출 함수
  onBye: PropTypes.func,
};

Hello.defaultProps = {
  big: false,
};
```

위처럼 propTypes를 설정 할 때 props위에

```js
/** 내용 */
```

주석으로 내용을 채우면 이 내용이 문서에서 나타납니다.  
이제 Docs탭을 눌러보면 name,big,onHello,onBye props가 정리되어 주석에 적혀있는 내용대로 description이
추가되어 나타납니다.
또한 defaultProps를 설정해준 big은 default false로 나타납니다.

### 컴포넌트 부제목 및 설명 추가

컴포넌트에 부 제목을 설정하기 위해서는 스토리의 parameters부분을 설정해줍니다.

```js
// Hello/Hello.stories.js
export default {
  title: 'components/basic/Hello', // 스토리북에서 보여질 그룹과 경로
  component: Hello, // 어떤 컴포넌트를 문서화 할지
  decorators: [withKnobs], // 애드온 적용
  parameters: {
    componentSubtitle: '"안녕하세요" 컴포넌트',
  },
}
```

위처럼 parameters에 componentSubtitle값을 추가해주면 넣어준 값대로 스토리북에 부제목이 표시됩니다.  
설명을 추가 할 때는 컴포넌트 파일에서 컴포넌트 코드 바로 윗 부분에 주석으로 작성합니다.

```jsx
// Hello/Hello.js
/**
 * 안녕하세요 라고 보여주고 싶을 땐 `Hello` 컴포넌트를 사용하세요.
 *
 * - `big` 값을 `true`로 설정하면 **크게** 나타납니다.
 * - `onHello` 와 `onBye` props로 설정하여 버튼이 클릭했을 때 호출 할 함수를 지정 할 수 있습니다.
 */

function Hello({ name, big, onHello, onBye }) {
  ...
```

설명을 작성할 때는 멀티라인 주석을 작성해서 시작 부분이 /\*\* 와 같은 형태로 작성해야 Docs가 만들어질 때
이를 파싱합니다.  
위와같이 적용해주고 나면 스토리북의 컴포넌트 제목 부제목 밑에 작성한 설명이 보이게 됩니다.

## MDX로 문서 작성

MDX를 사용하면 마크다운형식으로 리액트 컴포넌트를 편리하게 문서화 할 수 있습니다.  
DocsPage로 설명을 추가하며 문서화를 할 때는 정해진 형식에 공간이 고정되어있기 때문에 제한적인 부분이 있지만,
MDX를 사용한다면 원하는 곳 어디든지 설명을 추가 할 수 있기 때문에 자유롭게 문서화가 가능합니다.  
따라서 컴포넌트 이외에도 색상, 타이포그래피에 관한 문서에 대한 설명을 작성할 때도 유용합니다.

Hello.mdx라는 파일을 만들어서 작성해보겠습니다.

```jsx
// Hello/Hello.mdx
import Hello from './Hello';
import {
  Story,
  Props,
  Description,
  Preview,
} from '@storybook/addon-docs/blocks';

# Hello

<Hello name="MDX" big={true} />
<Description of={Hello} />

## Props

이 컴포넌트에서 사용되는 Props들

<Props of={Hello} />

## 작은 Helllo

<Preview>
  <Story id="components-basic-hello--standard" />
</Preview>

작은 Hello 입니다.

## 커다란 Hello

'big' props를 true로 설정하면 커다란 Hello가 됩니다.

<Preview>
  <Story id="components-basic-hello--big" />
</Preview>

```

MDX를 사용해서 컴포넌트들을 마크다운 내부에서 렌더링 할 수 있습니다.  
'@storybook/addon-docs/blocks' 안에 있는 Block 컴포넌트를 사용하면 기존에 자동 생성된 DocsPage 에서
보여줬었던 것 처럼 컴포넌트의 설명 및 Props정보를 추출하여 보여줄 수 있습니다.
사전에 만든 스토리들을 DocsPage안에서 보여줄 때는 Story컴포넌트에 id Props를 전달해야 하는데,
이 id값은 스토리북에서 스토리를 선택했을 때 주소창 story/{id}을 확인하면 확인이 가능합니다.  
mdx 파일을 작성한 뒤. 전에 만들었던 스토리 파일에서 docs파라미터를 수정합니다.

```js
// Hello/Hello.stories.js
...
import mdx from './Hello.mdx';

export default {
  title: 'components/basic/Hello', // 스토리북에서 보여질 그룹과 경로
  component: Hello, // 어떤 컴포넌트를 문서화 할지
  decorators: [withKnobs], // 애드온 적용
  parameters: {
    componentSubtitle: '"안녕하세요" 컴포넌트',
    docs: {
      page: mdx,
    },
  },
};
```

위와 같이 파라미터를 설정해주고 Docs페이지를 확인하면 작성한 mdx파일대로 커스터마이징 된 상태로 보여집니다.

## MDX로만 스토리 작성

stories.js 파일을 따로 만들지 않고 mdx파일 내부에서 모든 작업도 가능합니다.

Bye.js 컴포넌트를 새로 만듭니다.

```jsx
// Bye/Bye.js

import React from 'react'
import PropTypes from 'prop-types'

const Bye = ({ name }) => {
  return <p>안녕히 가세요, {name}</p>
}

Bye.propTypes = {
  name: PropTypes.string,
}

export default Bye
```

그 후 Bye.stories.mdx 파일을 만듭니다.

```jsx
// Bye/Bye.stories.mdx

import { Meta, Story, Props, Preview } from '@storybook/addon-docs/blocks';
import Bye from './Bye';
import { withKnobs, text, boolean } from '@storybook/addon-knobs';

<Meta title="components|basic/Bye" component={Bye} decorators={[withKnobs]} />

# Button

<Preview>
  <Story name="Default">
    <Bye name={text('name', 'stories.js')} />
  </Story>
</Preview>

## Props

<Props of={Bye} />

```

Meta Block을 사용하면 기존에 CSF 형태로 스토리를 작성할 때 넣어주는 값들을 설정할 수 있습니다.  
Docs 애드온을 통하여 자동으로 만들어진 문서에서 제공하는 정보가 부족해서 컴포넌트의 기능을 자세하게 표현하기 어려운 상황이라면
mdx를 사용하면 됩니다.
컴포넌트가 아닌 Introduction, Colors, Typography 등은 MDX-only로 작성하면 됩니다.  
컴포넌트에 대한 MDX를 작성할 때는 MDX-only로 문서를 작성하게 된다면 Typescript를 사용할때 IDE에서 .mdx확장자에 대한
타입스크립트 지원이 제대로 이루어지지 않습니다.

## 타입스크립트로 props 문서화

스토리북에서 props를 나타내기 위해 propTypes를 사용했었는데,
타입스크립트를 사용하면 proptypes를 대체 할 수 있습니다.  
우선 타입스크립트의 tsconfig파일을 루트 디렉토리에 작성해줍니다.

```json
{
  "compilerOptions": {
    "target": "es5",
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react"
  },
  "include": ["src"]
}
```

그 후 main.js에 타입스크립트 필드를 사용합니다.

```js
// .storybook/main.js

module.exports = {
  stories: ['../src/**/*.stories.mdx', '../src/**/*.stories.@(js|jsx|ts|tsx)'],
  addons: [
    '@storybook/addon-links',
    '@storybook/addon-essentials',
    '@storybook/addon-knobs',
    '@storybook/addon-docs',
  ],
  typescript: {
    check: false,
    checkOptions: {},
    reactDocgen: 'react-docgen-typescript',
    reactDocgenTypescriptOptions: {
      shouldExtractLiteralValuesFromEnum: true,
      propFilter: prop =>
        prop.parent ? !/node_modules/.test(prop.parent.fileName) : true,
    },
  },
}
```

그리고 ts파일에서 mdx 파일을 불러오면 모듈이 없다는 에러가 생기기 때문에 typings.d.ts 파일을 만들어줍니다.
