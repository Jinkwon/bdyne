---
layout: post
title:  "js에서 모든 파일을 쉽게 import 하기"
date:   2017-05-20 11:57:00 +0900
categories: javascript
---

js에서 모든 파일을 쉽게 import 하는 방법이 있습니다.  
간단한 팁이지만 혼자 알기 아까워 간단히 글을 작성하였습니다.  
js에서 파일을 임포트 해서 사용하려면 아래와 같이 파일을 구성합니다.    
하지만 파일이 많아지면 선언부가 복잡해 집니다.

```javascript
import { getData, setData } from './modules/module1';
import { getData, setData2 } from './modules/module2';

```

쉬운 방법으로는,
modules 디렉토리 밑에 index.js 를 만들면 폴더 기준으로 파일을 import 할 수 있습니다.

```javascript
export * from './module1';
export * from './module2';
```

## 사용법

```javascript
import { getData, getData2, setData, setData2 } from './modules';

```


조금 더 깊게 응용하는 방법이 있습니다.

```javascript
const files = require.context('.', false, /\.js$/)
const modules = {}

files.keys().forEach((key) => {
  if (key === './index.js') return
  modules[key.replace(/(\.\/|\.js)/g, '')] = files(key).default
})

export default modules
```

이를 이용하면 index.js 에 매번 추가해 줄 필요가 없어집니다.
index.js 만 복붙하면 됩니다.
