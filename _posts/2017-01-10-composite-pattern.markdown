---
layout: post
title:  "컴포지트 패턴 (Composite Pattern)"
date:   2017-01-10 00:00:00 +0900
categories: design pattern
---

객체들을 트리(Tree) 구조로 구성하여 부분-전체 계층구조를 구현합니다.
이 패턴을 이용하면 Client 에서 개별 객체와 복합 객체(Composite)를 똑같은 방법으로 다루도록 할 수 있습니다.


![screenshot]({{ site.url }}/images/attachment/composite-pattern.png)


### 컴포지트 패턴의 특징

컴포지트 패턴을 이용하면 객체의 구성과 개별 객체를 노드로 가지는 트리 형태로 객체를 구축할 수 있습니다.
이런 복합 구조(composite structure)를 사용하면 복합 객체와 개별 객체에 대해 똑같은 작업을 적용할 수 있습니다.
즉, 대부분의 경우에 복합 객체와 개별 객체를 구분할 필요가 없어집니다.

### 사용 사례

Composite는 클라이언트 프로그램이 객체들의 구성(composition)과 개개의 객체 사이에 차이점을 무시해야할 때 사용될 수 있습니다.
만약 프로그래머가 여러개의 객체들을 같은 방법으로 사용하거나, 종종 그 객체 각각을 다루기 위해 거의 비슷한 코드를 사용하고 있는 자신의 모습을 발견한다면, 컴포짓이 좋은 선택이 될 수 있습니다.
이러한 상황에서 원시(primitive)객체와 구성 객체를 동일하게 다루는 것이 덜 복잡하게 됩니다.


### Composite Pattern 구성

#### Component
- Composite 하나를 포함하는 모든 component들의 추상화입니다.
- Composite에서 객체들을 위한 인터페이스를 정의합니다.

#### Leaf
- Composition에서 leaf 객체를 나타냅니다.
- Component의 모든 메소드를 구현합니다.

#### Composite
- Composite Component
- 자식들을 다루는 메소드를 제공합니다.
- 일반적으로 자식들에게 그 기능을 위임함으로써 Component의 모든 메소드를 구현합니다.


### Composite Pattern in JS

```javascript
var Node = function (name) {
    this.children = [];
    this.name = name;
}

Node.prototype = {
    add: function (child) {
        this.children.push(child);
    },

    remove: function (child) {
        var length = this.children.length;
        for (var i = 0; i < length; i++) {
            if (this.children[i] === child) {
                this.children.splice(i, 1);
                return;
            }
        }
    },

    getChild: function (i) {
        return this.children[i];
    },

    hasChildren: function () {
        return this.children.length > 0;
    }
}

// recursively traverse a (sub)tree

function traverse(indent, node) {
    log.add(Array(indent++).join("--") + node.name);

    for (var i = 0, len = node.children.length; i < len; i++) {
        traverse(indent, node.getChild(i));
    }
}

// logging helper

var log = (function () {
    var log = "";

    return {
        add: function (msg) { log += msg + "\n"; },
        show: function () { alert(log); log = ""; }
    }
})();

function run() {
    var tree = new Node("root");
    var left = new Node("left")
    var right = new Node("right");
    var leftleft = new Node("leftleft");
    var leftright = new Node("leftright");
    var rightleft = new Node("rightleft");
    var rightright = new Node("rightright");

    tree.add(left);
    tree.add(right);
    tree.remove(right);  // note: remove
    tree.add(right);

    left.add(leftleft);
    left.add(leftright);

    right.add(rightleft);
    right.add(rightright);

    traverse(1, tree);

    log.show();
}
```