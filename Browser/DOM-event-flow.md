# DOM의 이벤트 흐름
W3C DOM Level 3에 삽입된 내용의 번역입니다.

원문 : https://www.w3.org/TR/DOM-Level-3-Events/#event-target

## 이벤트 전파와 DOM이벤트 흐름

이 섹션에서는 이벤트를 보내는 작동원리와 어떻게 DOM트리로 전파되는지 알아본다. 응용 프로그램들은 dispatchEvent() 메소드를 이용하여 이벤트 오브젝트를 보낼수 있고, 이벤트 오브젝트는 _DOM 이벤트 흐름_을 따라 결정된대로 전파될 것이다.

![eventflow.svg](https://www.w3.org/TR/DOM-Level-3-Events/images/eventflow.svg)

이벤트 오브젝트들은 이벤트 대상으로 전달된다. 하지만 이벤트 전달이 시작되기 전에, 이벤트 오브젝트의 전파경로가 먼저 결정되어야 한다.

[전파 경로](https://www.w3.org/TR/DOM-Level-3-Events/#propagation-path)(propagation path)는 이벤트가 지나가게 되는 현재 이벤트 대상들(current event targets)의 정렬된 리스트이다. 이 전달 경로는 문서의 트리 계층구조를 반영한다. 이 리스트의 마지막 아이템이 [이벤트 대상](https://www.w3.org/TR/DOM-Level-3-Events/#event-target)(event target)이며, 리스트의 선행 아이템들은 <em>대상의 조상들(target's ancestors)</em>로, 바로 앞의 선행 아이템은 <em>대상의 부모(target's parents)</em>로 참조된다.

일단 [전파 경로](https://www.w3.org/TR/DOM-Level-3-Events/#propagation-path)(propagation path)가 결정되면, 이벤트 오브젝트는 한단계 이상의 [이벤트 단계](https://www.w3.org/TR/DOM-Level-3-Events/#event-phase)(event phase)를 거치게 된다. 이벤트의 세 단계는 다음과 같다 : [캡쳐 단계](https://www.w3.org/TR/DOM-Level-3-Events/#capture-phase)(capture phase), [목표 단계](https://www.w3.org/TR/DOM-Level-3-Events/#target-phase)(target phase), 그리고 [버블 단계](https://www.w3.org/TR/DOM-Level-3-Events/#bubble-phase)(bubble phase). 이벤트 오브젝트들은 이 단계들을 아래에 기술한대로 완료한다. 각 단계는 지원되지 않거나 이벤트 오브젝트의 전파가 멈추면 생략될 수 있다. 예를들어 [버블](https://dom.spec.whatwg.org/#dom-event-bubbles)(bubbles) 속성이 false로 설정되어 있다면, 버블 단계는 생략될 것이다. 그리고 만약 [stopPropagation()](https://dom.spec.whatwg.org/#dom-event-stoppropagation)이 전달 전에 불렸다면, 모든 단계들이 생략될 것이다.

- **캡쳐 단계(capture phase)**: 이벤트 오브젝트가 [Window](https://www.w3.org/TR/DOM-Level-3-Events/#window)에서 대상의 부모에 이르기까지, 대상의 조상들을 통해 전파한다. 이 단계는 <em>캡쳐링 단계(capturing phase)</em>로도 알려져있다.
<br>

- **타겟 단계(target phase)**: 이벤트 오브젝트가 이벤트 오브젝트의 [이벤트 대상](https://www.w3.org/TR/DOM-Level-3-Events/#event-target)(event target)으로 도착한다. 이 단계는 또한 <em>목표 단계(at-target phase)</em>로도 알려져 있다. 만약 [이벤트 종류](https://www.w3.org/TR/DOM-Level-3-Events/#event-type)(event type)가 버블(bubble)이 일어나지 않음을 나타내고 있다면, 이벤트 오브젝트는 이 단계가 완료된 후에 종료할 것이다.
<br>

- **버블 단계(bubble phase)**: 이벤트 오브젝트가 대상의 부모(target's parent)부터 시작하여 [Window](https://www.w3.org/TR/DOM-Level-3-Events/#window)에 이르기까지 대상의 조상들을 역순으로 전파한다. 이 단계는 또한 <em>버블링 단계(bubbling phase)</em>로도 알려져 있다.


## 기본 동작과 취소 가능한 이벤트들

이벤트들은 일반적으로 유저들의 행동에 따른 구현이나, 작업의 완료에 대한 응답, 혹은 비동기 작업(예: 네트워크 요청)중에 진행상황을 알리는 것들에 의해 전달된다. 어떤 이벤트들은 다음에 취할 동작의 구현(혹은 이미 진행된 구현의 취소)을 제어하기 위해 사용할 수 있다. 이 카테고리에 있는 이벤트들은 <em>취소가능한(cancelable)</em>이벤트로 불리며, 그것들이 취소하는 동작을 그들의 <em>[기본 행동](https://www.w3.org/TR/DOM-Level-3-Events/#default-action)(default action)</em>이라 부른다. 취소 가능한 이벤트 오브젝트들은 하나 이상의 '기본 행동(default action)'과 연결될 수 있다. 이벤트를 취소하기 위해서는 [preventDefault()](https://dom.spec.whatwg.org/#dom-event-preventdefault)메소드를 호출하면 된다.


> 예제 1:
[mousedown](https://www.w3.org/TR/DOM-Level-3-Events/#mousedown)이벤트는 유저가 포인팅 장치(일반적으로 마우스다)의 버튼을 누른후에 즉시 전달된다. 구현에 의해 취해질 수 있는 한가지 [기본행동](https://www.w3.org/TR/DOM-Level-3-Events/#default-action)(default action)은 유저들이 이미지를 드래그하거나 텍스트를 선택할 수 있도록 기계의 상태를 설정하는 것이다. [기본행동](https://www.w3.org/TR/DOM-Level-3-Events/#default-action)(default action)은 다음에 무엇이 일어나느냐에 따라 달려있다. 예를들어, 만약 유저의 포인팅 장치가 텍스트위에 있다면 텍스트 선택이 시작될 것이고, 이미지위에 있다면 이미지 드래그 행동이 시작될 수 있을 것이다. [mousedown](https://www.w3.org/TR/DOM-Level-3-Events/#mousedown)이벤트의 [기본행동](https://www.w3.org/TR/DOM-Level-3-Events/#default-action)(default action) 방지는 이러한 동작들이 일어나는것을 막는다.

[기본 행동들](https://www.w3.org/TR/DOM-Level-3-Events/#default-action)(default actions)은 주로 이벤트 전달이 완료된 후에 수행되지만, 예외적으로 이벤트가 전달되기 전에 바로 수행되는 경우들도 있다.

> 예제 2:
> 기본 행동(default action)은 <input type="checkbox">엘리먼트의  [click](https://www.w3.org/TR/DOM-Level-3-Events/#click)이벤트는 해당 엘리먼트의 `checked` IDL attribute의 값을 토글한다. 만약 [click](https://www.w3.org/TR/DOM-Level-3-Events/#click)이벤트의 기본 행동(default action)이 취소되면, 그 값은 원래 상태로 복구된다.


이벤트가 취소되면, 이벤트와 연결되었던 조건부 [기본행동들](https://www.w3.org/TR/DOM-Level-3-Events/#default-action)(default actions)들이 생략된다(혹은 위에 언급했던것 처럼 [기본행동들](https://www.w3.org/TR/DOM-Level-3-Events/#default-action)(default actions)이 전달 전에 수행된다면, 그 효과가 취소된다). 이벤트 오브젝트의 취소 가능여부는 [cancelable](https://dom.spec.whatwg.org/#dom-event-cancelable) 속성에 표시된다. [preventDefault()](https://dom.spec.whatwg.org/#dom-event-preventdefault)를 호출하는것은 이벤트 오브젝트의 모든 연관된 [기본행동들](https://www.w3.org/TR/DOM-Level-3-Events/#default-action)(default actions)을 멈추게 한다. [defaultPrevented](https://dom.spec.whatwg.org/#dom-event-defaultprevented)속성은 이벤트가 이미 취소되었는지를 나타낸다(예: 선행 이벤트 리스너에 의해서). 만약 [DOM application](https://www.w3.org/TR/DOM-Level-3-Events/#dom-application) 자신이 전달을 시작했다면, [dispatchEvent()](https://dom.spec.whatwg.org/#dom-eventtarget-dispatchevent)메소드의 리턴값은 그 이벤트 오브젝트가 취소되었는지를 나타낸다.

> 참고:
> 많은 구현들은, 취소될 취소가능한 이벤트들의 [기본행동](https://www.w3.org/TR/DOM-Level-3-Events/#default-action)(default action)을 의미하는 값으로 false를 추가적으로 리스너의 리턴값으로 넣는다.

## 동기와 비동기 이벤트들

이벤트들은 동기적으로 혹은 비동기 적으로 전달될 수 있다.

동기적 이벤트들("sync events")은 다른 이벤트들, DOM변경 순서, 그리고 유저 상호작용의 일시적인 발생 순서로 정렬되어 있는 가상의 선입선출 모델 큐에 있는것 처럼 다뤄진다. 이 가상큐에 있는 각각의 이벤트는 직전의 이벤트가 전파행동을 완료하거나 취소되기까지 기다린다. 어떤 동기 이벤트들은 마우스 버튼 이벤트와 같은 특정한 장치나 프로세스에 의해 실행된다. 이 이벤트들은 이벤트들의 집합을 위해 정의된 [이벤트 순서](https://www.w3.org/TR/DOM-Level-3-Events/#event-order)(event order) 알고리즘에 의해 지배되고, 유저 에이전트들은 이 이벤트들을 정의된 순서에 따라 전달할 것이다.

비동기적 이벤트들("async event")들은 동작이 완료된 결과에 의해서만 전달된다. 다른 이벤트들이나 DOM에서의 변화, 그리고 유저 상호작용과도 연관이 없다.

> 예제 3:
> 문서를 로딩하는 동안, 인라인 스크림트 엘리먼트는 파싱되고 실행된다. [load](https://www.w3.org/TR/DOM-Level-3-Events/#load)이벤트는 스크립트 엘리먼트에서 비동기적으로 실행되도록 대기된다. 하지만, 비동기 이벤트이기 때문에 문서가 로드되는동안 실행되는 다른 동기 이벤트들(예: [HTML5](https://www.w3.org/TR/DOM-Level-3-Events/#biblio-html5)의 `DOMContentLoaded` 이벤트)과 관련된 순서는 보장되지 않는다.

## 신뢰할 수 있는 이벤트들

[유저 에이전트](https://www.w3.org/TR/DOM-Level-3-Events/#user-agent)(user agent)나 유저 상호작용의 결과 혹은 DOM변화의 직접적인 결과에 의해 생성되는 이벤트들은, [createEvet()](https://dom.spec.whatwg.org/#dom-document-createevent)메소드를 통해 생성되었거나, [initEvent()](https://dom.spec.whatwg.org/#dom-event-initevent)메소드를 통해 수정되었거나, 혹은 [dispatchEvent()](https://dom.spec.whatwg.org/#dom-eventtarget-dispatchevent)메소드에 의해 전달된 이벤트들에는 제공되지 않는 특권들을 갖고 있는 [유저 에이전트](https://www.w3.org/TR/DOM-Level-3-Events/#user-agent)(user agent)에 의해 신뢰된다. 신뢰할 수 있는 이벤트의 [isTrusted](https://dom.spec.whatwg.org/#dom-event-istrusted)속성은 `true`를 값으로 갖는다. 반면에 신뢰할 수 없는 이벤트들은 [isTrusted](https://dom.spec.whatwg.org/#dom-event-istrusted)속성의 값을 `false`로 갖는다.

대부분의 신뢰할 수 없는 이벤트들은 [click](https://www.w3.org/TR/DOM-Level-3-Events/#click)이벤트를 제외하고는 [기본 행동들](https://www.w3.org/TR/DOM-Level-3-Events/#default-action)(default actions)들을 발생시키지 않는다. 이 이벤트는 [isTrusted](https://dom.spec.whatwg.org/#dom-event-istrusted)속성의 값이 `false`일지라도 언제나 [기본 행동](https://www.w3.org/TR/DOM-Level-3-Events/#default-action)(default action)을 발생시킨다(이런 동작들은 이전버전과의 호환을 위해 남아있다). 다른 모든 신뢰할 수 없는 이벤트들은 [preventDefault()](https://dom.spec.whatwg.org/#dom-event-preventdefault)메소드가 이벤트에서 불려진것처럼 행동하게 된다.

## 활성화 트리거들과 행동

어떤 [이벤트 대상](https://www.w3.org/TR/DOM-Level-3-Events/#event-target)(예: 링크나 버튼 엘리먼트)들은 [활성화 트리거](https://www.w3.org/TR/DOM-Level-3-Events/#activation-trigger)(activation trigger)(예: 링크의 클릭)에대한 응답으로 수행하는 구현인 연관된 [활성화 행동](https://www.w3.org/TR/DOM-Level-3-Events/#activation-behavior)(activation behavior)을 가지고 있을 수 도 있다.
 
 > 예제 4:
 > HTML과 SVG는 모두 링크를 표시하는 \<a> 엘리먼트를 갖고 있다. \<a>엘리먼트를 위한 적절한 [활성화 트리거들](https://www.w3.org/TR/DOM-Level-3-Events/#activation-trigger)(activation triggers)은 \<a>엘리먼트의 내용인 텍스트나 이미지의 [click](https://www.w3.org/TR/DOM-Level-3-Events/#click)이벤트이거나 혹은  [key](https://www.w3.org/TR/DOM-Level-3-Events/#dom-keyboardevent-key)속성의 값이 \<a>엘리먼트가 포커스를 갖고 있을때 "Enter"키의 값을 가지는 [keydown](https://www.w3.org/TR/DOM-Level-3-Events/#keydown)이벤트이다. \<a> 엘리먼트를 위한 활성화 행동(activation behavior)은 일반적으로 외부 링크의 경우에는  창의 내용을 새로운 문서로 바꾸는 것이고, 내부 링크의 경우에는 새로운 앵커의 위치로 현재 문서를 이동하는 것이다.

[활성화 트리거](https://www.w3.org/TR/DOM-Level-3-Events/#activation-trigger)(activation trigger)는 활성화 행동(activation behavior)이 시작되어야 하는 구현을 가리키는 유저 행동이나 이벤트이다. 사용자가 시작한(user-initated) [활성화 트리거들](https://www.w3.org/TR/DOM-Level-3-Events/#activation-trigger)(activation triggers)은 활성가능한 엘리먼트에 마우스 버튼 클릭을 하거나, 포커스를 갖고 있는 활성가능한 엘리먼트에 `Enter`키를 누르는 것이나, 또는 포커스를 갖고 있지 않은 엘리먼트더라도 활성가능한 엘리먼트와 어떻게든 연결되어 있는 키('핫키' 또는 '엑세스 키')를 누르는 것을 포함한다. 이벤트 기반의 [활성화 트리거들](https://www.w3.org/TR/DOM-Level-3-Events/#activation-trigger)(activation triggers)는 특정 시각이나 일정 시간이 지난 후에 실행되는 타이머 기반의 이벤트나, 어떤 동작이 완료되었을때 진행되는 이벤트, 또는 조건 기반이나 상태 기반의 이벤트들을 포함할 수 있다.

## 마우스와 키보드 이벤트의 생성

일반적으로, [Event](https://dom.spec.whatwg.org/#event) 인터페이스의 생성자나 [Event](https://dom.spec.whatwg.org/#event)인터페이스를 상속받은 이벤트가 호출되면, [DOM](https://dom.spec.whatwg.org/#event)에 기술된 단계들을 따라야 한다. 하지만 [keyboardEvent](https://www.w3.org/TR/DOM-Level-3-Events/#keyboardevent-keyboardevent)와 [MouseEvent](https://www.w3.org/TR/DOM-Level-3-Events/#mouseevent) 인터페이스들은 [Event](https://dom.spec.whatwg.org/#event)오브젝트의 키 변경자 상태의 초기화를 위한 추가적인 딕셔내리 멤버들을 제공한다: 구체적으로 [getModifierState()](https://www.w3.org/TR/DOM-Level-3-Events/#dom-keyboardevent-getmodifierstate)의 사용하기 위해 퀴리된 내부 상태  와 [getModifierState()](https://www.w3.org/TR/DOM-Level-3-Events/#dom-keyboardevent-getmodifierstate)메소드가 있다. 이 섹션은 이런 선택가능한 변경자 상태를 갖고있는 새로운 [Event] 오브젝트를 초기화하기 위한 DOM4 과정들을 제공한다.

[KeyboardEvent](https://www.w3.org/TR/DOM-Level-3-Events/#keyboardevent-keyboardevent)와 [MouseEvent](https://www.w3.org/TR/DOM-Level-3-Events/#mouseevent) 혹은 아래의 알고리즘을 사용하고 있는 이런 오브젝트들로부터 파생된 오브젝트를 생성하기 위한 목적으로, 모든 [KeyboardEvent](https://www.w3.org/TR/DOM-Level-3-Events/#keyboardevent-keyboardevent), [MouseEvent](https://www.w3.org/TR/DOM-Level-3-Events/#mouseevent)그리고 파생된 오브젝트들은   [[UIEvents-Key](https://www.w3.org/TR/DOM-Level-3-Events/#biblio-uievents-key)]의 [변경자 키 테이블](http://www.w3.org/TR/uievents-key/#keys-modifier)(Modifiers Keys table)에 기술된 [키 변경자 이름](https://www.w3.org/TR/DOM-Level-3-Events/#keys-modifiers)(key modifier names)을 사용하여 설정하고 검색할 수 있는 <em>**내부 키 변경자 상태(internal key modifier state)** </em>를 갖는다.

다음의 과정들은 DOM4에서 이벤트들을 생성하기 위해 정의된 알고리즘을 제공한다.

    - 만약 생성중인 [Event](https://dom.spec.whatwg.org/#event)가 [KeyboardEvent](https://www.w3.org/TR/DOM-Level-3-Events/#keyboardevent-keyboardevent)혹은 [MouseEvent](https://www.w3.org/TR/DOM-Level-3-Events/#mouseevent) 오브젝트 혹은 이런것들로부터 파생된 다른 오브젝트이고, [EventModifierInit]() 인자(argument)가 생성자에 제공되었다면, 다음의 하위절차를 따른다.
- 모든 [EventModifierInit](https://www.w3.org/TR/DOM-Level-3-Events/#dictdef-eventmodifierinit) 인자들 중에서, 만약 딕셔내리 멤버가 문자 "modifier"로 시작한다면 <em>**key modifier name**</em>이 딕셔내리 멤버의 접두어인 "modifier"를 제외하도록 하고, [키 변경자 이름](https://www.w3.org/TR/DOM-Level-3-Events/#modifier-key-name)(key modifier name)과 일치하는 [Event](https://dom.spec.whatwg.org/#event) 오브젝트의 [내부 키 변경자 상태](https://www.w3.org/TR/DOM-Level-3-Events/#internal-key-modifier-state)(internal key modifier state)를 해당 값으로 설정한다.
