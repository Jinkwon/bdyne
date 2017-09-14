---
layout: post
title:  "Zone.js 의 성능 이슈 해결 사례"
date:   2017-09-14 10:44:00 +0900
categories: javascript
---

zone.js의 성능 이슈 해결 사례를 공유 합니다.  

내용의 요지는,  
zone.js의 MonkeyPatching 으로 인한 WebWorker 및 requestAnimation의 성능 저하 이슈 입니다.

적은 저하지만 매우 여러번 호출되는 부분에서 작은 저하로 인해 전체 성능이 저하되어 보일 수 있어 이 부분을 깊게 파 보았는데요,  
내부 로직의 코드를 외부에서 접근하는 형태 ```window.__zone_symbol__requestAnimationFrame;```를 이용해 일부 로직을 우회하는 방법은 찾았으나,  
공식 API가 지원 되면 좋을듯하여 공식 저장소에 반영을 요청하고 결과물이 zone.js 최신 버전(0.8.17)에 통합 되었습니다.  

(참조: [https://github.com/angular/zone.js/issues/875](https://github.com/angular/zone.js/issues/875))

아래는 제가 실제로 사용하고 있는 zone.js 의 옵션 모습입니다.

mousemove, requestAnimationFrame, onmessage 는 MonkeyPatching 을 사용하지 않게 하고,
Change Detection이 필요한 부분이 있다면 직접 수행할 수 있게 로직을 구성하였습니다.

```javascript
<script>
window.__Zone_disable_requestAnimationFrame  = true;
window.__Zone_ignore_on_properties = [
  {
    target: window,
    ignoreProperties: ['message']
  },
  {
    target: HTMLElement.prototype,
    ignoreProperties: 'mousemove'
  }
];
</script>
```