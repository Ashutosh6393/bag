###currencyConverter
 
app.component.ts
 
import {Component} from '@angular/core';
 
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss'],
  standalone: false
})
export class AppComponent {
  title = "Currency Converter";
 
  yenValue: number;
  usdValue: number;
 
  onUsdValueChange(value) {
    this.yenValue =  value ? value/0.0090 : null;
  }
 
  onYenValueChange(value) {
   
    this.usdValue = value ? value * 110 : null;
  }
}
 
###yenValue.component.html
 
 
<div class="card mr-20 w-18">
  <h3 class="card-text text-center my-0 py-20">Japanese Yen</h3>
  <textarea
    name="yenValue"
    rows={{6}}
    cols={{30}}
    placeholder="Enter value in Yen"
    class="mx-25 mb-25"
    data-test-id="yen-value"
    [(ngModel)]="yenValue"
    [(ngModel)]="yenInput"
    (input)="onYenInput()"
  >
  </textarea>
</div>
 
 
 
###yenValue.component.ts
 
import { Component, Input, OnInit, Output, EventEmitter } from "@angular/core";
 
@Component({
  selector: "yen-value",
  templateUrl: "./yenValue.component.html",
  styleUrls: ["./yenValue.component.scss"],
  standalone: false
})
export class YenValue implements OnInit {
  @Output() onYenValueChange: EventEmitter<string> = new EventEmitter<string>();
  @Input() yenValue: string;
  yenInput:string;
  constructor() {}
 
  ngOnInit() {
  }
  onYenInput(){
    this.onYenValueChange.emit(this.yenInput);
  }
}
 
 
 
 
###usdValue.component.html
 
<div class="card w-18 ml-20">
  <h3 class="card-text text-center my-0 py-20">US Dollar</h3>
  <textarea
    name="usdValue"
    rows={{6}}
    cols={{30}}
    placeholder="Enter value in USD"
    class="mx-25 mb-25"
    data-test-id="usd-value"
    [(ngModel)]="usdInput"
    (input)="onUsdSent()"
    [(ngModel)]="usdValue"
  >
  </textarea>
</div>
 
 
 
###usdValue.component.ts
 
 
import { Component, Input, OnInit, Output, EventEmitter } from "@angular/core";
 
@Component({
  selector: "usd-value",
  templateUrl: "./usdValue.component.html",
  styleUrls: ["./usdValue.component.scss"],
  standalone: false
})
export class UsdValue implements OnInit {
  @Output() onUsdValueChange: EventEmitter<string> = new EventEmitter<string>();
  @Input() usdValue: string;
  usdInput:string;
 
  constructor() {
  }
onUsdSent(){
  this.onUsdValueChange.emit(this.usdInput);
}
  ngOnInit() {
  }
}
 
 
 