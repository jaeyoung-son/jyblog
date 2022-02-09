---
title: 'Vue 3.0 공식 런칭, React Hook API와의 차이는?'
date: 2022-02-09 15:21:13
category: 'react'
draft: false
---

## Vue를 접하며

처음 Vue를 접할 때 React와는 다른 Template문법, 양방향 바인딩 등으로 인해 많이 낯설게 느꼈지만, 3.0버전에서 추가된 composition API를 통해 React와 비슷하게 컴포넌트/비즈니스 로직을 분리하여 구조적으로 관리할 수 있게 되면서 구조적으로 관리하고 있습니다.

아직 Nuxt에서는 정식 런칭되지 않았고 nuxtjs/composition 라이브러리를 사용해서 setup 함수에서 로직 단위의 분리를 하고 있습니다.

React의 hook API를 통해 뷰/비즈니스 로직 분리, 재사용이 용이한 관리 방식이 매우 마음에 들던 저로서는 매우 기쁜 소식이었는데요, 막상 실제 프로젝트 적용해보니 예상했던 것과는 다르게 작동하는 부분들이 있어서 당황했던 경험이 있었습니다.

간단한 예제를 통해 두 API를 살펴보고 어떤 차이가 있는지 확인해보겠습니다.

## React의 hook API

React는 class형 컴포넌트 방식에서 상태 관리 로직을 재사용하기 어려웠던 점과 상태 관리 로직이 한 공간에 묶여있어 유지보수하기 어려운 문제, javascript class의 복잡함을 해결하기 위해 hook API를 도입했습니다.

React 16.8v 이후 hook API가 추가되어 React 컴포넌트가 아닌 일반 자바스크립트 함수에서도 `useState`와 `useEffect`를 통해 상태 값, 생명주기를 활용할 수 있게 되었고, 해당 함수를 사용해서 컴포넌트/비즈니스 로직을 추출하여 사용 할 수 있습니다.

hook을 잘 이해할 수 있게 카운터 예제에 적용해보겠습니다.

**카운터 훅**

```jsx
import { useState, useEffect } from 'react'

const useCounter = () => {
  const [value, setValue] = useState(0)

  useEffect(() => {
    console.log(value, '새로운 value')
  }, [value])

  const onIncrease = () => {
    setValue(value + 1)
  }
  const onDecrease = () => {
    setValue(value - 1)
  }

  return {
    value,
    onIncrease,
    onDecrease,
  }
}
```

**카운터 컴포넌트**

```jsx
const Counter = () => {
  const { value, onIncrease, onDecrease } = useCounter()

  return (
    <>
      <h2>{value}</h2>
      <button onClick={onIncrease}>+</button>
      <button onClick={onDecrease}>-</button>
    </>
  )
}

export default Counter
```

## Vue의 composition API

Vue에서도 대규모 어플리케이션을 다룰 때 로직의 재사용과 논리적 관심사를 분리할 수 있게 하기 위해 composition API를 도입했습니다.

Vue 에서는 3.0버전에 도입된 composition API의 `ref`를 통해 반응성 변수를 외부에서 관리할 수 있고 기존 생명주기에서 on접두사가 추가된 라이프사이클 훅을 통해 라이프사이클 또한 사용 가능하게 되었습니다. `setup` 컴포넌트 옵션으로 `setup`내부에서 커스텀 함수를 불러와 사용이 가능합니다. `setup` 은 `beforecreate`와 `created`사이에 실행됩니다.

React와 동일한 카운터 예제에 composition API를 적용해보겠습니다.

**카운터 훅**

```jsx
import { ref, watch } from 'vue'

const useCounter = () => {
  const counterValue = ref(0)

  watch(counterValue, newValue => {
    console.log(newValue, '새로운 value')
  })

  const onIncrease = () => {
    counterValue.value += 1
  }

  const onDecrease = () => {
    counterValue.value -= 1
  }

  return {
    counterValue,
    onIncrease,
    onDecrease,
  }
}

export default useCounter
```

**카운터 컴포넌트**

```js
<template>
  <h2>{{ counterValue }}</h2>
  <button @click="onIncrease">+</button>
  <button @click="onDecrease">-</button>
</template>
<script>
import useCounter from '../hooks/useCounter'

export default {
  setup() {
    const { counterValue, onIncrease, onDecrease } = useCounter()

    return { counterValue, onIncrease, onDecrease }
  },
}
</script>
```

## 어떻게 다를까?

위 예시를 확인해보면 React와 Vue에서 hook을 사용하는 코드가 비슷하다는 느낌을 받을 수 있는데요

하지만 몇가지 차이로 인해 다르게 동작하는 부분이 생깁니다.

### 실행 환경의 차이

#### React

React에서 useCounter Hook을 사용하는 함수형 컴포넌트는 함수입니다. 함수 컴포넌트는 렌더링이 되면 함수가 다시 실행됩니다.

```jsx
// React
import Counter from './Counter'

console.log('리액트 카운터 컴포넌트의 타입은 ', typeof Counter)
// -> '리액트 카운터 컴포넌트의 타입은 function'
```

함수형 컴포넌트는 기존 클래스형 컴포넌트의 `render()` 메소드와 유사하게 동작하며 리렌더링 시 컴포넌트 함수 자체가 재 실행됩니다. 따라서 컴포넌트가 리렌더링을 하면 내부의 Custom Hook 함수도 다시 호출하게 됩니다.
단 useState는 렌더링할 때 오직 한번만 생성되기 때문에 useState를 통해 반환받은 state값은 항상 최신 상태의 state입니다.

#### Vue

Vue 컴포넌트는 props, setup, render와 같은 컴포넌트의 속성들을 가지고 있는 객체입니다.

```jsx
// Vue
import Counter from './Counter'

console.log('뷰 카운터 컴포넌트의 타입은 ', typeof Counter)
// -> '뷰 카운터 컴포넌트의 타입은 object'
```

Vue에서 Composition API는 setup 메소드에서 사용됩니다.
React 함수 컴포넌트와 같이 setup도 함수이지만 setup은 컴포넌트가 생성되기 전에 한번 실행되며 Composition API의 진입점 역할을 합니다. 라이프 사이클이라고 정의하지는 않았지만 라이프 사이클과 같이 동작합니다. Custom Hook 함수가 실행되는 setup이 한번 실행되기 때문에 Hook 함수도 한번 실행됩니다. 다시 렌더링이 되어도 setup을 다시 실행시키지 않습니다.
따라서 함수 내부에 console.log를 통해 특정 값을 확인하는 등의 액션이 첫 호출 시에만 실행되고 리렌더링 시에는 실행되지 않는 것을 확인할 수 있었습니다.

즉 Vue에서는 해당 함수가 한번 호출되고, React에서는 컴포넌트가 렌더링 하는 만큼 호출됩니다.

위 차이로 인하여 Vue의 커스텀 훅 함수에서 컴포넌트의 props를 파라미터로 받는다면 그 값은 첫 호출시 받았던 값이고 props가 변해도 커스텀 훅이 기억하는 값은 변하지 않습니다. 따라서 watch를 등록하는 등 props에 대한 변경사항을 보장받기 위해선 `toRef`또는 `toRefs`를 사용해서 감싸야 합니다.

```js
// useCounter 예시
setup(props) {
		const { fromParent } = toRefs(props)
    const { counterValue, onIncrease, onDecrease } = useCounter(fromParent);

    return { counterValue, onIncrease, onDecrease };
  },
```

### 새로운 값 계산

Vue는 특정 반응성 값에 의존하며 계산된 값을 표현하기 위해 `computed` 를 제공합니다.

```jsx
const name = ref('재영')
const age = ref(50)
const description = computed(() => `내 이름은 ${name.value}`)
```

computed를 사용하지 않고 그냥 string으로 선언했다면, 첫 setup 호출 시 기억하는 name값에서 변하지 않는 형태입니다. 하지만 computed를 사용하면 내부에서 의존하는 값을 자동으로 추적하여 name이 변경될 때 descriptions을 다시 계산합니다.

React의 커스텀 훅 함수는 컴포넌트가 렌더링 하는 만큼 호출된다고 하였습니다. 컴포넌트 렌더링은 컴포넌트에서 관리되는 state값이 setState에 의해 변경되면 발생합니다. 따라서 특정 변수를 선언하고 해당 변수와 관계 없는 값이 변경되더라도, 그 변수를 다시 계산하고 선언합니다.

```jsx
const [name, setName] = useState('재영')
const [age, setAge] = useState(50)
const [number, setNumber] = useState(1234)

const description = `내 이름은 ${name}`
```

위 예시에서 descriptions이 의존하는 값은 name 이지만 age, number이 변경되더라도 description을 다시 계산하게됩니다. 만약 더 많은 상태값이 있고 계산의 비용이 비싸다면 해당 컴포넌트의 성능은 크게 저하됩니다.

React는 이 문제에 대한 해결책으로 `useMemo`와 `useCallback` hook을 제공합니다.

위 예시에 적용해보겠습니다.

```jsx
import { useState, useMemo } from 'react'

const [name, setName] = useState('재영')
const [age, setAge] = useState(50)
const [number, setNumber] = useState(1234)

const description = useMemo(() => `내이름은 ${name}`, [name])
```

useMemo는 콜백과 의존성 배열을 전달하고 메모이제이션된 값을 반환합니다. 이제 descriptions은 name이 변경될 때만 계산하고 age, number등이 바뀌어도 계산하지 않습니다. useCallback은 useMemo와 동일하나 콜백 자체를 반환합니다.

### 반응형 값 설정

위 Counter 예시에서 onIncrease, onDecrease함수를 보면 Vue와 React에서 반응형 값을 설정하는 방식이 차이가 있는 것을 알 수 있습니다.

#### React

React의 useState Hook은 state와 setter함수인 setState를 반환합니다.
값을 설정하기 위해 setState로 값을 변경시키고 이전 값과 현재 값을 비교한 뒤 값이 변경되었다면 해당 컴포넌트를 재 실행시켜 바뀐 값으로 다시 렌더링합니다.
이전 값과 현재 값의 비교는 Object.is 알고리즘을 사용합니다.

#### Vue

Vue에서는 반응형 값을 생성하면서 비교하기 위해 값 생성 시 원본으로의 호출을 중간에 가로채서 처리하기 위해 Proxy를 활용하여 getter, setter를 내부적으로로 지정해주기 때문에 React의 setState와 같은 별도의 setter 함수는 필요없이 반응형 값을 직접 수정합니다. (ref의 경우 RefImpl Class의 Get,Set 접근자 사용)
Vue컴포넌트 템플릿은 컴파일 되어 render 함수로 만들어지고 반응성 값이 변경되면 render 함수를 다시 실행하여 렌더링합니다.
이전 값과 현재 값의 비교는 React와 동일하게 Object.is 알고리즘을 사용합니다.

## 마무리 하며

사내에서 Vue와 React프로젝트를 모두 관리했던 것은 저에게 매우 좋은 경험이었습니다.

긴 고민과 논의 후 기존 Nuxt 프로젝트를 Next 프로젝트로 점진적인 마이그레이션을 계획하고 진행하고 있습니다. 물론 이 과정에서 변경된 css framework와 관련된 차이, 기존 라이브러리들을 온전히 대체하지 못하는 점, Next.js와 Nuxt.js의 다른 middleware 의 역할 등의 차이로 인해 맞춰가는 어려움도 많이 있습니다. 하지만 이 과정에서 다양하고 도전적인 경험들을 할 것으로 예상되며 기대하고 있습니다.
