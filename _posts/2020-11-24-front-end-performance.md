---
title: Web 성능 개선
tags:
  - front-end
---
# Rendering  성능 개선

Chrome browser의 **Timeline Panel**을 이용하여 분석을 한다.



### Rendering 과정

```javascript
document.getElementById('testBox').style.width = "100px";
```

- Recalculate Style
- width 속성이 변경되어 좌표 계산이 필요한가를 판단하여 필요시 **Layout**이 발생한다.
- **Update Layer Tree** 발생
- 계산된 영역의 정보를 비트맵으로 저장하기 위해 **paint** 발생
- Composite Layer 작업에서 각 레이어를 병합 후, 화면에 출력

*`부드러운 화면 전환을 위해서는 위의 작업이 모두 16.6ms 내에 처리되어야 함.`*

### Layer의 구성

- layer 모델 설명

### Rendering 성능 개선의 목표 분석

- 전체 구조 중 Recalculate Style, Update Layer Tree, Composite Layer 단계는 반드시 발생.
- 중간에 관여할 수 있는 요소인 Layout과 Paint 그리고 Layer에 의해 결정 됨.
- layout, paint를 줄이고 최적의 layer를 구성

### Layout, Paint 비용 줄이기

#### 업데이트 flow

`script` -> **CPU** -> *Recalculate style* -> **<u>Layout</u>** -> *Update Layer Tree* -> **Main Memory** -> **<u>Paint</u>** -> **Video Memory** -> *Composite Layer* -> **GPU** -> **화면 Rendering**

Layout, Paint를 유발하는 속성을 사용하지 않고, 대안을 사용하여 같은 효과를 구현.

- **left/top**에 의한 위치 이동은 **transform: translate**
- **show/hide**는 alpha 값을 이용하는 **opacity**

### 최적의 Layer 구성하기

대상의 DOM node가 주변이나 자신에 의해 자주 변경되지 않는 경우에 적용한다.

- LayerPanel이나, RenderingPanel의 show composite layer border를 통해 확인 후 불필요한 Layer는 제거.
- 사용하지 않는 Layer는 display:none 처리



### Chrome Rendering 탭 활용

- show paint rectangles - paint 되는 영역이 녹색으로 표시
- show potential scroll bottlenecks - touch, mousewheel과 같이 스크롤에 영향을 미치는 이벤트 핸들러 표시



참조: [FrontEnd 개발자가 수행하는 성능 개선 작업 - 손찬욱](https://sculove.github.io/slides/improveBrowserRendering/#/)

