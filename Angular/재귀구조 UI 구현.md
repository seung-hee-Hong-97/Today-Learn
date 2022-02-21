## Ionic + Angular를 통한 재귀구조 UI 구현

Tree 형태의 데이터, 즉 계층 구조의 데이터를 UI로 구현 하는 방법으로,

Angular의 `*ngTempleteOutlet` 을 활용한 재귀적 방법을 통해 구현할 수 있습니다.



### 계층 구조 데이터

```javascript
nodes = [
  {
    title: "Section 1",
    children: []
  },
  {
    title: "Section 2",
    children: [
      {
        title: "Section 2.1",
        children: []
      },
      {
        title: "Section 2.2",
        children: []
      },
      {
        title: "Section 2.3",
        children: []
      }
    ]
  },
  {
    title: "Section 3",
    children: [
      { title: "Section 3.1", children: [] },
      {
        title: "Section 3.2",
        children: [
          {
            title: "Section 3.2.1",
            children: []
          },
          {
            title: "Section 3.2.2",
            children: []
          },
          {
            title: "Section 3.2.3",
            children: [
              {
                title: "Section 3.2.3.1",
                children: []
              },
              {
                title: "Section 3.2.3.2",
                children: []
              }
            ]
          }
        ]
      },
      {
        title: "Section 3.3",
        children: [
          {
            title: "Section 3.3.1",
            children: []
          },
          {
            title: "Section 3.3.2",
            children: []
          }
        ]
      }
    ]
  }
];
```

> 재귀 구조를 구현하기 위해 필요한 테스트용 데이터 입니다.
>
> 자식을 가지는 노드도 있고, 가지고 있지 않는 노드도 있습니다.



### ngTemplateOutlet은 무엇인가?

우선 ngTemplateOutlet을 알아보면, 템플릿 참조 및 컨텍스트 개체를 매개 변수로 사용하여 템플릿을 동적으로 인스턴스화 시켜주는 역할을 합니다.

쉽게 설명하면 DOM의 다양한 섹션에 템플릿을 삽입하는 데 사용할 수 있습니다.

몇 가지 템플릿을 정의하여 항목을 표시하고 이를 View의 여러 위치에 표시하고 사용자의 선택에 따라 해당 템플릿을 교체할 수 있습니다.



### ngTemplateOutlet 사용방법

```html
<ul>
  <ng-template #recursiveList let-nodes>
    <li *ngFor="let node of nodes">
      {{node.title}}
      
      <ul *ngIf="node.children.length > 0">
        <ng-container *ngTemplateOutlet="recursiveList; context: { $implicit: node.children }"></ng-container>
      </ul>
    </li>
  </ng-template>
  
  <ng-container *ngTemplateOutlet="recursiveList; context: { $implicit: nodes }"></ng-container>
</ul>
```

> 1. children 배열에 요소가 있을 경우
>
>    자식 노드를 가지고 있는 경우 node.children의 ngTemplateOutlet에 데이터가 전달되어 자식 노드를 렌더링합니다.
>
> 2. children 배열에 요소가 없는 경우
>
>    자식 노드를 가지고 있지 않는 경우 nodes의 ngTemplateOutlet에 상위 노드를 렌더링합니다.
>
> 위에 작성된 nodes 배열의 Test 데이터를 Angular *ngTemplateOutlet을 활용하여 UI에 나타내면 아래와 같은 결과가 나옵니다.



<img src="http://192.168.1.157/asdfg/icoopintern/-/raw/master/images/tree-data.png" width="350">



### $implicit 사용

Context 개체에서 `$implicit` 키를 사용하면, 모든 지역 변수에 대한 기본값으로 해당 값을 설정합니다.

위의 코드를 기준으로 하면 node.children, nodes 가 값에 해당됩니다.



