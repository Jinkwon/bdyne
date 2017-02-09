---
layout: post
title:  "Some of your tests did a full page reload 이슈 해결 방법"
date:   2017-02-09 01:00:00 +0900
categories: javascript
---

자바스크립트로 location.href 나 window.open 테스트를 해야 될 때가 있습니다.
이때 karma runner를 이용해 테스트를 하면 아래와 같은 에러가 나면서 테스트가 멈춥니다.

![screenshot]({{ site.url }}/images/attachment/2017-02-09-test-error.png)


아래와 같은 방법을 써도 잘 되지 않고,,

```javascript
window.open = () => {};
```

이를 우회하기 위해 아래와 같은 방안을 이용할 수 있습니다.
본인은 jasmine을 이용하고 있으므로 jasmine의 createSpy 를 이용했습니다.
테스트 코드안에 추가 해 주면 됩니다. :)

```javascript
window.onbeforeunload = jasmine.createSpy();
```

