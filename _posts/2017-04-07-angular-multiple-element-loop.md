---
layout: post
title:  "angular2/4 다중 엘리먼트의 loop 구현"
date:   2017-02-09 01:00:00 +0900
categories: javascript
---

- angular2/4 multiple element loop

앵귤러에서 template에선 보통 단일 엘리먼트를 기준으로 loop를 구현합니다.  
대부분의 상황에서는 아래와 같은 형태로 충분합니다.  

```html
<ul>
  <li *ngFor="item of items">{{item.name}}</li>
</ul>

```

하지만 조금 더 깊게 응용하려고 보면 이런 상황이 발생합니다.  
table 에 특정 요소가 여러개 필요하다면? (rowspan 적용 모델) 위와 같은 방식으로는 loop 구현이 어렵습니다.  
그럴 땐 아래와 같이 ng-container 문법을 이용하면 됩니다.  

```html
<table>
  <tbody>

  <ng-container *ngFor="let item of [1,2,3,4,5]">
  <tr>
     <td>...</td>
   </tr>
   <tr>
     <td>...</td>
   </tr>
   </ng-container>

   </tbody>
</table>
```

간단한 문법이지만 모르면 또 헤맬 수 있으므로 간단히 정리 해 보았습니다.^^  

### Ref
[https://angular.io/docs/ts/latest/guide/structural-directives.html#!#ng-container](https://angular.io/docs/ts/latest/guide/structural-directives.html#!#ng-container)
