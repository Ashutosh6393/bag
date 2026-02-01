------Caesar's cipher--------

//CipherText.html

<div class="card w-18 ml-20">
  <h3 class="card-text text-center my-0 py-20">Ciphertext</h3>
  <textarea
    name="ciphertext"
    rows={{6}}
    cols={{30}}
    placeholder="Enter ciphertext"
    class="mx-25 mb-25"
    data-test-id="cipher-text"
    [(ngModel)]="cipherText"
    (ngModelChange)="onText($event)"
  >
  </textarea>
</div>


//cipherText.ts

import { Component, Input, OnInit, Output, EventEmitter } from "@angular/core";

@Component({
  selector: "cipher-text",
  templateUrl: "./cipherText.component.html",
  styleUrls: ["./cipherText.component.scss"],
  standalone: false
})
export class CipherText implements OnInit {
  @Output() onCipherTextChange: EventEmitter<string> = new EventEmitter<string>();
  @Input() shift: number;
  @Input() cipherText: string;
  constructor() {
    
  }

  onText(value:string){
    this.onCipherTextChange.emit(value);
  }

  ngOnInit() {
  }
}


//PlainText.html

<div class="card mr-20 w-18">
  <h3 class="card-text text-center my-0 py-20">Plaintext</h3>
  <textarea
    name="plaintext"
    rows={{6}}
    cols={{30}}
    placeholder="Enter plaintext"
    class="mx-25 mb-25"
    data-test-id="plain-text"
    [(ngModel)]="plainText"
    (ngModelChange)="onText($event)"
  >
  </textarea>
</div>


//PlainText.ts

import { Component, Input, OnInit, Output, EventEmitter } from "@angular/core";

@Component({
  selector: "plain-text",
  templateUrl: "./plainText.component.html",
  styleUrls: ["./plainText.component.scss"],
  standalone: false
})
export class PlainText implements OnInit {
  @Output() onPlainTextChange: EventEmitter<string> = new EventEmitter<string>();
  @Input() shift: number;
  @Input() plainText: string;
  constructor() {
  
  }

  onText(value:string){
    this.onPlainTextChange.emit(value);
  }
  ngOnInit() {
  }
}


//Shift.html


<div class="layout-row justify-content-center mt-100">
  <select
    data-test-id="select"
    (ngModelChange)="onSelect($event)"
    [(ngModel)]="selected"
  >
    <option
      [value]="-1"
      disabled
    >Enter shift amount</option>
    <option *ngFor="let option of shiftOptions; let i=index" [value]="i">{{i}}</option>
  </select>
</div>


//Shift.ts

import { Component, Input, Output, EventEmitter, OnInit } from "@angular/core";

@Component({
  selector: "shift",
  templateUrl: "./shift.component.html",
  styleUrls: ["./shift.component.scss"],
  standalone: false
})
export class Shift implements OnInit {
  @Output() onShiftChange: EventEmitter<number> = new EventEmitter<number>();

  shiftOptions: number[] = [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0];
  selected:number=-1;
  constructor() {}

  ngOnInit() {
  }
  
onSelect(value: number | string) {
    const n = typeof value === 'string' ? parseInt(value, 10) : value;
    this.selected = n;
    this.onShiftChange.emit(n);
  }

}


///app.html

<h8k-navbar header="Caesar's Cipher"></h8k-navbar>

<div class="layout-row align-items-center justify-content-center mt-40">
  <shift (onShiftChange)="onShiftChange($event)"></shift>
</div>
<div class="layout-row justify-content-center mt-40">
  <plain-text [shift]="shift" [plainText]="plainText" (onPlainTextChange)="onPlainTextChange($event)"></plain-text>
  <cipher-text [shift]="shift" [cipherText]="cipherText" (onCipherTextChange)="onCipherTextChange($event)"></cipher-text>
</div>


//app.ts


import {Component} from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss'],
  standalone: false
})
export class AppComponent {
  title = "Caesar's Cipher";

  shift: number = -1;
  plainText: string = '';
  cipherText: string = '';

  private lastEdit:'plain' |'cipher' | null=null;
  onShiftChange(i) {
    this.shift=i;
    if(this.shift<0){
      return;
    }
    if(this.lastEdit==='plain'){
      this.cipherText=this.encrypt(this.plainText);
    }else if(this.lastEdit==='cipher'){
      this.plainText=this.decrypt(this.cipherText);
    }
  }

  onPlainTextChange(text) {
    this.plainText=text;
    this.lastEdit='plain';
    this.cipherText=this.shift>=0? this.encrypt(text):'';
  }

  onCipherTextChange(text) {
    this.cipherText=text;
    this.lastEdit='cipher';
    this.plainText=this.shift>=0?this.decrypt(text):'';
  }

  encrypt(plainText) {
    let cipherArr = []
    let cipherLetter;

    plainText.split("").map(letter => {
      let code = letter.charCodeAt(0);
      if(letter === " ") {
        return cipherArr.push(letter)
      }
      // Uppercase letters
      if (code >= 65 && code <= 90) {
        cipherLetter = String.fromCharCode(((code - 65 + this.shift) % 26) + 65)
      }
      // Lowercase letters
      else if (code >= 97 && code <= 122) {
        cipherLetter = String.fromCharCode(((code - 97 + this.shift) % 26) + 97)
      }
      return cipherArr.push(cipherLetter)
    })
    return cipherArr.join("");
  }

  decrypt(cipherText) {
    let plainArr = []
    let plainLetter

    this.cipherText.split("").map(cipher => {
      let code = cipher.charCodeAt(0);
      if(cipher === " ") {
        return plainArr.push(cipher)
      }
      // Uppercase letters
      if (code >= 65 && code <= 90) {
        let diff = code - 65 - this.shift
        if (diff >= 0) {
          plainLetter = String.fromCharCode((diff % 26) + 65);
        } else {
          plainLetter = String.fromCharCode(((26 + diff) % 26) + 65);
        }
      }
      // Lowercase letters
      else if (code >= 97 && code <= 122) {
        let diff = code - 97 - this.shift
        if (diff >= 0) {
          plainLetter = String.fromCharCode((diff % 26) + 97);
        } else {
          plainLetter = String.fromCharCode(((26 + diff) % 26) + 97);
        }
      }
      return plainArr.push(plainLetter)
    })
    return plainArr.join("");
  }
}


