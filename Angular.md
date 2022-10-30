#### button component

button.component.ts
```js
import { Component, OnInit, Input, Output, EventEmitter } from '@angular/core';

@Component({
  selector: 'app-button',
  templateUrl: './button.component.html',
  styleUrls: ['./button.component.css'],
})
export class ButtonComponent implements OnInit {
  @Input() text?: string;      <--------- * 
  @Input() color?: string;     <--------- *
  @Output() btnClick = new EventEmitter();   <--------- *

  constructor() {}

  ngOnInit(): void {}

  onClick() {
    this.btnClick.emit();      <--------- *
  }
}
```

button.component.html
```js
<button [ngStyle]="{ background: color }" (click)="onClick()">
  {{ text }}
</button>
```
header.component.html
```js
<app-button color="green" text="click" (btnClick)="toggle()"></app-button>   <--------- *
```

#### *nfFor
```js
<tr *ngFor="let hero of heroes">
  <td>{{hero.name}}</td>
</tr>
```
