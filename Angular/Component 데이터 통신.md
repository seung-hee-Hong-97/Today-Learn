# Component 간 데이터 통신

Component란 Angular에서 화면을 구성하는 최소 단위입니다. 클라이언트에게 보이는 최종 화면은 하나의 html로 보여지지만, 내부적으로는 여러 개의 Component들로 이루어져 있을 수 있습니다.

따라서 Component 간에 데이터를 교환해야 원활한 개발을 할 수 있습니다.



## 부모 Component에서 자식 Component로 데이터 전달하기 (입력 바인딩)

### 공통 DA

```typescript
export interface Student {
  name: string;
}
```

### 부모 Component

```typescript
// Parent.component.ts

import { Student } from './student';

export class ParentComponent {
  student: Student[] = [name: 'hone', name: 'lee', name: 'kim'];
}
```

```html
<!-- Parent.component.html -->
<h2>
  Input Binding Test
</h2>
<app-child [student]="student"></app-child>
```

> testData를 자식 컴포넌트에 데이터 바인딩 합니다.

### 자식 Component

```typescript
// Child.component.ts

import { Input } from '@angular/core';
import { Student } from './student';

export class ChildComponent {
  @Input() student: Student;
}
```

``` html
<!-- Child.component.html -->

<h3>
  {{student.name}}
</h3>
```

> @Input 데코레이터를 통해 데이터를 입력 받습니다.

## 자식 Component에서 부모 Component로 데이터 전달하기

자식 Component에서 어떠한 이벤트가 발생할 경우 이 이벤트는 `EventEmitter` 타입으로 지정한 프로퍼티를 통해 부모 Component에게 보낼 수 있습니다. 부모 Component는 이 이벤트를 바인딩해서 원하는 로직을 실행할 수 있습니다.

자식 Component에서 외부로 이벤트를 보내려면 `EventEmitter` 타입으로 선언한 프로퍼티에 `@Output()` 데코레이터를 사용하여 출력 프로퍼티로 지정하면 됩니다.

### 부모 Component

``` html
<!-- Parent.component.html -->

<h2>
  Output Binding Test
</h2>

<app-child 
	*ngFor="let voter of voters" 
  [name]="voter"
  (voted)="onVoted($event)">
</app-child>
```

```typescript
// Parent.component.ts

import { Component } from '@angular/core';

@Component({
  selector: 'app-parent',
  'template': './parent.component.html'
})

export class ParentComponent {
  agreed = 0;
  disagreed = 0;
  voters = ['Kim', 'Lee', 'Park'];
  
  onVoted(agreed: boolean) {
    agreed ? this.agreed++ : this.disagreed++;
  }
}
```

> onVoted 함수를 자식 Component에 바인딩 합니다.

### 자식 Component

```html
<!-- Child.component.html -->

<h3>
  {{ name }}
</h3>

<button (click)="vote(true)" [disabled]="didVote">
   Agree
</button>

<button (click)="vote(false)" [disabled]="didVote">
  Disagree
</button>
```

```typescript
// Child.component.ts

import { Component, EventEmitter, Input, Output } from '@angular/core';

@Component({
  selector: 'app-child',
  template: './child.component.html'
})

export class ChildComponent {
  @Input() name: string;
  @Output() voted: EventEmitter<boolean> = new EventEmitter();
  didVote = false;
  
  vote(agreed: boolean) {
    this.voted.emit(agreed);
    this.didVote = true;
  }
}
```

> 버튼을 클릭하면, onVote( ) 함수가 이벤트 객체를 인자로 받아, `agree` 와 `disagree` 카운터를 갱신합니다.
>
> 이때 전달되는 이벤트 객체는 템플릿에서 `$event` 라는 이름으로 접근할 수 있으며, 템플릿에서 이벤트 핸들러 함수에 인자로 전달하기 때문에, 컴포넌트 클래스 코드에서 활용할 수 있습니다.