---

id: hooks-intro
title: Introducing Hooks
permalink: docs/hooks-intro.html
next: hooks-overview.html
---

*Hooks*는 React 16.8 버전에 새로 추가되었습니다. Hooks는 여러분이 class를 사용할 필요 없이 state와 React의 특징들을 사용할 수 있게 합니다.

```js {4,5}
import React, { useState } from 'react';
function Example() {
// Declare a new state variable, which we'll call "count"
const [count, setCount] = useState(0);
return (
<div>
<p>You clicked {count} times</p>
<button onClick={() => setCount(count + 1)}>
Click me
</button>
</div>
);
}
```

`useState`는 우리가 Hooks에 대해 처음으로 배우게 될 함수입니다. 그러나 이 예제는 티져에 불과합니다. 아직 이해가 안 되어도 걱정하지 마세요!

**당신은 [다음 페이지](/docs/hooks-overview.html) 부터 Hooks에 대해 배울 수 있습니다.** 이번 페이지에서는 우리는, 우리가 왜 Hooks를 React에 추가하였는지, 어떻게 Hooks가 여러분의 훌륭한 어플리케이션을 개발하는 데 도움을 주는지 설명할 것입니다.

>Note
>
>React 16.8.0은 Hooks를 지원하는 첫 번째 release입니다. 업그레이드 할x`xxx 때 React DOM을 포함한 모든 패키지를 업데이트 하는 것을 잊지 마세요. React-Native는 다음번 안정된 release부터 지원할 것입니다.

## Video Introduction {#video-introduction}

2018 React Conf에서 Sophie Alpert와 Dan Abramov는 Hooks를 소개하였습니다. 이어서 Ryan Florence가 Hooks를 사용하여 어떻게 어플리케이션을 피팩토링 할것인지 보여주었습니다.

동영상을 보시죠!

<br>

<iframe width="650" height="366" src="//www.youtube.com/embed/dpw9EHDh2bM" frameborder="0" allowfullscreen></iframe>

## 변경사항 없음 {#no-breaking-changes}

먼저 Hooks의 특징은:

- **완벽한 동의** : 당신은 현재 코드를 다시 쓸 필요 없이 몇몇 컴포넌트들에 Hooks를 사용할 수 있습니다. 그러나 당신이 Hooks의 사용을 원하지 않는다면 지금 당장 사용할 필요는 없습니다.
- **100% 이전 버전과의 호환성** : Hooks는 어떠한 변화도 포함하지 않습니다.
- **현재 이용가능** : Hooks는 현재 버전 16.8.0과 함께 이용이 가능합니다.

**React는 class들을 제거할 계획이 없습니다.** 당신은 아래에 있는 Hooks를 위한 [점진적인 적용 전략]((#gradual-adoption-strategy))을 읽어볼 수 있습니다.

**Hooks는 여러분의 React에 대한 컨셉을 대체할 수 없습니다.** 대신, Hooks는 당신이 이미 알고 있는 React 컨셉에 더욱 직관적 API로 제공합니다. : props, state, context, refs and lifecycle까지. 우리가 나중에 보여드리겠지만, Hooks는 또한 그들과 결합하는 새롭고 강력한 방법을 제공해 줄 것입니다.

**만약 당신이 바로 배우기를 원하신다면, 자유롭게 [다음 페이지]((/docs/hooks-overview.html))로 가셔도 좋습니다**. 이 페이지에서는 우리가 왜 Hooks를 추가하였고 우리가 어떻게 우리 앱을 재작성하지 않고 사용할 것인지 알아볼 것입니다.

## Hooks의 동기 {#motivation}

Hooks는 우리가 수만 개의 컴포넌트들을 유지하고 사용해온 5년 동안 만났던 넓고 다양한 React에서의 연결되지 않은 문제들을 해결하였습니다. 당신이 React를 배우는 중이든, 매일 사용하든, 심지어 비슷한 컴포넌트 모델과 함께 다른 라이브러리를 선호하든지 간에, 당신은 이러한 문제들을 인식해야 합니다.

### 컴포넌트사이의 상태적인 로직 재사용이 어렵습니다.{#its-hard-to-reuse-stateful-logic-between-components}

React는 컴포넌트에 재활용하는 방법인 "붙이는" 방법을 제공하지 않습니다. (예를 들어, store에 연결하는 것 등) 만약 여러분이 React로 일해 왔었다면 여러분은 이런 문제들을 해결하기 위해 시도된 render props나 HOC같은 패턴들이 꽤 익숙할 것입니다. 그러나 이러한 패턴들은 여러분이 그 패턴을 사용할 때 다시 재 구조화를 요구하고 더 번거롭고 코드를 따르기 어려워질 수 있습니다. 만약 당신이 React DevTools에서 전형적인 어플리케이션을 본다면, 당신은 추상적 컴포넌트, [render props]((/docs/render-props.html)), [HOC]((/docs/higher-order-components.html)), consumer, provider층의 의해 둘러 쌓인 "wrapper hell" 컴포넌트를 발견할 것입니다. 우리는 DevTools로 그것들을 필터링할 수 있는 반면에, 포인트는 더 깊은 문제입니다: React는 stateful 로직을 공유하기 위해 더 좋은 기초 요소를 필요합니다.

Hooks와 함께 당신은 컴포넌트로부터 stateful 로직을 추출할 수 있습니다. 그래서 그것은 의존성과 재사용성을 테스트할 수 있습니다. Hooks는 컴포넌트의 계층 변화 없이 stateful 로직을 재사용할 수 있게 해줍니다. 이렇게 하면 많은 컴포넌트 사이와 커뮤니티 사이에서 Hooks를 공유하는 것이 더 쉬워집니다.

우리는 [우리만의 Hooks]((/docs/hooks-custom.html))를 구축하는데 더 알아봅시다.

### 복잡한 컴포넌트들은 이해하기가 어렵습니다. {#complex-components-become-hard-to-understand}

우리는 종종 간단하게 시작했지만 이끌 수 없는 side effects와 stateful 로직 덩어리로 성장한 컴포넌트들을 유지해 왔었습니다. 각각의 라이프싸이클 메소드는 종종 연관되지 않은 로직을 섞어 포함하였습니다. 예를 들어, 컴포넌트들은 `componentDidMount와` `componentDidUpdate로` 몇몇 data를 fetcing하는 것을 수행할 수 있습니다. 그러나 몇몇 `componentDidMount` 메소드는 이벤트 리스너를 설정하는 데 연관 없는 로직 들을 포함할 수 있으며 `componentWillUnmount는` cleanup을 수행할 수도 있습니다. 함께 변경되는 상호 관련 코드는 분리되지만 완벽하게 연관 없는 코드들은 단일 메소드로 결합합니다. 이로 인해 버그와 무결성을 너무나 쉽게 발생합니다.

많은 경우에 이 컴포넌트들을 작게 만드는 것은 불가능합니다. 왜냐하면, stateful 로직은 모든 공간에 있기 때문입니다. 그것들을 테스트하는 것은 굉장히 어렵습니다. 이것이 react를 별도의 state관리를 라이브러리랑 결합하는 것을 선호하는 것 이유 중 하나입니다. 그러나 종종 너무 추상적이라고 소개하고 다른 파일들 사이에 건너뛰기를 요구하며 컴포넌트 재사용을 더욱 어렵게 만듭니다.

이런 이슈들을 해결하기 위해 **Hooks는 라이프싸이클 메소드를 기초로 한 분리에 초점을 맞추기보다는 당신이 컴포넌트들을 연관 있는 조각을 기초로 한 더욱 작은 함수로 쪼갤 수 있게 합니다.** (fetching data나 구독을 설정하는 것과 같은). 당신은 예언적인 리듀서를 만듦으로써 컴포넌트 지역 state를 관리하는 것을 선택할 수 있습니다.

우리는 [Effect Hook]((/docs/hooks-effect.html#tip-use-multiple-effects-to-separate-concerns).)에 대해 더 알아보자.

### Classes는 사람과 기계를 혼동시킨다.{#classes-confuse-both-people-and-machines}

재사용성 코드나 코드 구성을 만드는 것은 어려울 뿐만 아니라, 우리는 classes가 React를 배우는데 큰 진입장벽이라는 것을 알고 있습니다. 당신은 다른 대부분의 언어들과는 다르게 작동하는 JavaScript가 어떻게 동작하는지 이해하고 있어야만 합니다. 당신은 모든 이벤트 핸들러가 바인딩하는 것을 기억해야만 한다. 안정적인 문장 제안이 없다면, 코드는 매우 장황하다. 사람들은 props, state, top-down data flow완벽하게 이해할 수 있지만, 여전히 classes는 쉽지가 않다. React안에서의 class와 function 컴포넌트를 구별하고 각 요소를 언제 사용하는지는 숙련된 React 개발자 사이에서도 의견이 일치하지 않는다.

추가적으로, React는 5년 동안 지속되었으며, 향후 5년 뒤에도 관련성이 유지되기를 바랍니다. Svelte, Angular, Glimmer 같은 이들이 보여주길 사전 컴파일 컴포넌트들은 많은 잠재능력을 가지고 있습니다. 특별하게 만약 템프릿이 제한되어지지 않는다면, 최근 우리는 Prepack을 사용한 컴포넌트 folding을 실험해왔고, 우리는 유망한 빠른 결과를 보았습니다. 그러나 우리는 class컴포넌트들은 의도되지 않은 최적화를 통해 더욱 느리게 만드는 패턴을 불러올 수도 있습니다. Classes 오늘날의 도구에게 이슈들을 제시합니다. 예를 들어 클래스는 최소화 시킬 수 없습니다. 그리고 그들은 flakey하고 신뢰할수없는 hot reloading을 만듭니다. 우리는 최적화의 경로에서 유지될 가능성이 있는 코드 API를 제시하고 싶습니다.

이러한 문제를 해결하기 위해서 **Hooks는 classes없이 React의 특징들을 사용할 수 있게 해줍니다.** 이론상, React 컴포넌트들은 항상 함수에서 closer를 가집니다. Hooks는 functions를 수용하지만, React의 정신에 희생하지 않습니다. Hooks는 필수 탈출 해치를 제공하고 당신이 복잡한 함수나 반응프로그래밍 기법을 배울 것을 요구하지 않습니다.

>Examples
>
>[Hooks 살펴보기](/docs/hooks-overview.html)는 Hooks를 배우기 좋은 공간입니다.

## 점진적 적응 전략 {#gradual-adoption-strategy}

>**TLDR: React에서 classes를 지울 계획은 없습니다.**

우리는 React 개발자들이 사용하는 API에 초점을 맞추고 매번 새롭게 출시되는 API에 초점을 맞출 시간이 없다는 것을 알고 있습니다. Hooks는 매우 새롭고, 그들이 적응되거나 배우는 것을 고려하기 전에 더욱 많은 예시와 튜토리얼 들을 기다리는 것이 더 좋을 수도 있습니다.

우리는 이 원시적인 Hooks를 React에 추가하기에는 쉽지 않다는 것을 알고 있습니다. (허들이 매우 높다는 것을 알고 있습니다) 궁금해하는 독자들을 위해 우리는 동기 부여에 대한 자세한 내용을 담고 있는 구체적인 [RFC]((https://github.com/reactjs/rfcs/pull/68))(Request for comment)를 준비하고 특정 디자인 결정 및 관련 선행 기술에 대한 추가적인 관점을 제공합니다.

**결정적으로, Hooks는 당신이 현재 존재하는 코드와 나란히 작동하므로 점진적으로 적용할 수 있습니다.** 우리는 React의 미래 형태에 관해 관심 있는 커뮤니티에 빠른 피드백을 받기 위해서 실험적인 API를 공유하고 있습니다. 그리고 우리는 Hooks는 공개적으로 반복할 것입니다.

마지막으로, Hooks로의 이동을 서두를 필요는 없습니다. 우리는 현존하는 복잡한 class 컴포넌트들의 큰 리팩토링을 피하도록 권유합니다. "thinking in Hooks"를 시작하기 위해선 약간의 마음가짐이 필요합니다. 경험에 의하면, 처음엔 새롭고 비판적이지 않은 컴포넌트들을 대상으로 Hooks를 사용하고 그러고 나서 당신의 팀 모두가 안정감을 느끼는지 확인하는 것이 가장 좋습니다. 그 이후 당신은 Hooks를 시행하고 우리에게 긍정적인지, 부정적인지 [피드백]((https://github.com/facebook/react/issues/new))을 보내 주시면 감사하겠습니다.

우리는 현재 class로 사용된 모든 사례들을 Hooks로 바꾸는 것을 원하지만, **우리는 class 컴포넌트들을 미래까지 계속해서 지원할 예정입니다.** 페이스북에서 우리는 수만 개의 class 컴포넌트들을 작성했으며 우리는 그것들을 리팩토링 할 계획이 전혀 없습니다. 우리는 기존의 코드들과 나란히 Hooks를 사용할 예정입니다.

## Frequently Asked Questions {#frequently-asked-questions}

우리는 Hooks에 대한 [자주 묻는 질문들]((/docs/hooks-faq.html))을 이 페이지에 준비했습니다.

## Next Steps {#next-steps}

이 페이지가 끝나고, 당신은 Hooks가 풀고 있는 문제들의 대략적인 아이디어를 가지고 있어야만 합니다. 그러나 아직 구체적으로 명확하진 않을 것입니다. 걱정하지 마세요! ** [다음 페이지]((/docs/hooks-overview.html))로 가서 예제를 통해서 Hooks에 대해 배워 봅시다!**