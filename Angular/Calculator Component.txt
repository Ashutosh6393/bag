------------------CalculatorComponent-------------


//calculator.html

<div class="layout-column align-items-center">

  <h4 data-test-id="total-operations" class="pt-50">Total operations performed: {{count}} </h4>
  <div class="card">
    <section class="card-text">
      <div class="layout-row justify-content-around align-items-center mt-40">
        <input type="number" class="ml-3 mr-3" data-test-id="app-input1" autocomplete="off" placeholder="Eg: 1"
               name="input1" [(ngModel)]="inp1"/>

        <label class="ml-2 mr-2 symbol text-center" data-test-id="selected-operator">{{symbol}}</label>

        <input type="number" data-test-id="app-input2" autocomplete="off" class="ml-3 mr-3"
               placeholder="Eg: 2" [(ngModel)]="inp2"/>
      </div>

      <div class="layout-row justify-content-around mt-30">
        <button type="submit" data-test-id="add-button" (click)="add()"><span
          class="operationFont">+</span></button>
        <button type="submit" data-test-id="minus-button" (click)="minus()">
          <span class="operationFont">-</span></button>
        <button type="submit" data-test-id="multiply-button" (click)="multiply()"><span
          class="operationFont">*</span></button>
        <button type="submit" data-test-id="divide-button" (click)="substract()"><span
          class="operationFont">/</span>
        </button>
      </div>


      <div class="layout-row justify-content-between align-items-center mt-30">
        <button type="reset" class="outline danger" data-test-id="reset-button" (click)="reset()">
          Reset
        </button>
        <div class="layout-row justify-content-center align-items-center result-container">
          <h5 *ngIf="result" data-test-id="result" class="result-value ma-0 slide-up-fade-in">Result: {{result}}</h5>
        </div>

      </div>
    </section>

  </div>
</div>


//calculator.ts

import {Component, OnInit} from '@angular/core';


@Component({
  selector: 'app-Calculator',
  templateUrl: './calculator.component.html',
  styleUrls: ['./calculator.component.scss'],
  standalone: false
})
export class CalculatorComponent implements OnInit {
  inp1: number;
  inp2: number;
  result: number;
  symbol: string='+';
  count: number=0;

  ngOnInit() {
  }

  add(){
    this.result=this.inp1+this.inp2;
    this.count++;
    this.symbol='+';
  }
  minus(){
    this.result=this.inp1-this.inp2;
    this.count++;
    this.symbol='-';
  }
  multiply(){
    this.result=this.inp1*this.inp2;
    this.count++;
    this.symbol='*';
  }
  substract(){
    this.result=this.inp1/this.inp2;
    this.count++;
    this.symbol='/';
  }
  reset(){
    this.inp1=null;
    this.inp2=null;
    this.result=null;
    this.symbol='+';
  }
}
