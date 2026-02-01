------payment validation-----

validation.html

<div class="layout-column justify-contents-center align-items-center">
  <section class="card pa-50 card-container">
    <div class="credit-card-box">
      <div class="wave">

        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1440 320">
          <path fill="#1ba94c" fill-opacity="1"
                d="M0,224L48,208C96,192,192,160,288,144C384,128,480,128,576,149.3C672,171,768,213,864,229.3C960,245,1056,235,1152,192C1248,149,1344,75,1392,37.3L1440,0L1440,320L1392,320C1344,320,1248,320,1152,320C1056,320,960,320,864,320C768,320,672,320,576,320C480,320,384,320,288,320C192,320,96,320,48,320L0,320Z"></path>
        </svg>
      </div>
      <div class="front">
        <img src="assets/logo.png" class="logo">
        <div class="number"></div>
        <div class="card-holder">
          <label class="card-number">{{ cardNumber }}</label>
          <label>{{ cardHolderName }}</label>
        </div>
        <div class="card-expiration-date">
          <label>{{ cardExpiryMonth }}/ {{ cardExpiryYear }}</label>
          <div></div>
        </div>
      </div>
    </div>
    <form [formGroup]="cardForm" class="layout-column" novalidate>
      <label class="mt-20">
        <input
          type="text"
          data-test-id="number-input"
          autocomplete="off"
          formControlName="number"
          class="white large outlined"
          [class]="{'error': true}"
          placeholder="Card Number"
        />
        <p class="error-text form-hint" data-test-id="number-input-error" *ngIf="(number.touched || number.dirty)&& number.invalid">
            <span *ngIf="number.errors?.['pattern'] || number.errors?.['required']">
              Invalid Card Number
            </span>
        </p>
      </label>
      <label class="mt-20">
        <input
          type="text"
          formControlName="name"
          autocomplete="off"
          data-test-id="name-input"
          class="white large outlined"
          [class]="{'error': true}"
          placeholder="Name on card"
        />
        <p data-test-id="name-input-error"
            class="error-text form-hint" *ngIf="(name.touched || name.dirty)&& name.invalid">
              <span *ngIf="name.errors?.['pattern'] || name.errors?.['required']">
                Invalid Cardholder Name
              </span>
        </p>
      </label>
      <div class="layout-row card-info mt-20">
        <section class="layout-column">
          <div class="flex">
            <label class="expiry-month">
              <input
                type="text"
                formControlName="month"
                data-test-id="month-input"
                autocomplete="off"
                class="white large outlined"
                [class]="{'error': true}"
                placeholder="##"
              />
              <p class="form-hint">Expiry Month</p>
              <p data-test-id="month-input-error"
                 class="error-text form-hint" *ngIf="(month.touched || month.dirty)&& month.invalid">
                <span *ngIf="month.errors?.['pattern'] || month.errors?.['required'] ">
                  Invalid Month
                </span>
              </p>
            </label>
            <label class="expiry-year ml-8">
              <input
                type="number"
                formControlName="year"
                data-test-id="year-input"
                autocomplete="off"
                class="white large outlined"
                [class]="{'error': true}"
                placeholder="####"
              />
              <p class="form-hint">Expiry Year</p>
              <p data-test-id="year-input-error"
                 class="error-text form-hint" *ngIf="(year.touched || year.dirty)&& year.invalid">
                <span *ngIf="year.errors?.['pattern'] || year.errors?.['required']">
                  Invalid Year
                </span>
              </p>
            </label>
          </div>
        </section>

        <label class="cvv-number">
          <input
            type="number"
            formControlName="cvv"
            autocomplete="off"
            data-test-id="cvv-input"
            class="white large outlined"
            [class]="{'error': true}"
            placeholder="###"
          />
          <p class="form-hint">CVV/CVC</p>
          <p data-test-id="cvv-input-error"
             class="error-text form-hint" *ngIf="(cvv.touched || cvv.dirty)&& cvv.invalid">
                <span *ngIf="cvv.errors?.['pattern'] || cvv.errors?.['required']">
                  Invalid CVV/CVC
                </span>
          </p>
        </label>
      </div>


      <button class="mt-50" type="submit" data-test-id="submit-button" [disabled]="cardForm.invalid" >
        Submit
      </button>
    </form>
  </section>
</div>


//validation.ts

import { Component, OnInit } from '@angular/core';
import { FormGroup, FormBuilder, Validators, FormControl } from '@angular/forms';
import { formatCardNumber, formatExpiryDate } from "./utils";

@Component({
  selector: 'payment-validation',
  templateUrl: './paymentValidation.component.html',
  styleUrls: ['./paymentValidation.component.scss'],
  standalone: false
})


export class PaymentValidation {
  cardForm: FormGroup;

   yearReg=(()=> {
    const y= new Date().getFullYear();
    return `^(?:${y}|${y+1}|${y+2}|${y+3})$`;
  })();
  constructor(private fb: FormBuilder) {
    this.createForm();
  }

  createForm() {
    this.cardForm = this.fb.group({
      number: ['', [Validators.required,Validators.pattern(/^[0-9]{16}$/)]],
      name: ['', [Validators.required,Validators.pattern(/^[A-Za-z]+$/)]],
      month: ['', [Validators.required,Validators.pattern(/^(0[1-9]|1[0-2])$/)]],
      year: ['', [Validators.required,Validators.pattern(this.yearReg)]],
      cvv: ['', [Validators.required,Validators.pattern(/^\d{3}$/)]],
    });
  }

  get cardNumber() {
    return formatCardNumber(this.cardForm.controls['number'].value)
  }

  get cardExpiryMonth() {
    return formatExpiryDate(this.cardForm.controls['month'].value)
  }

  get cardExpiryYear() {
    return formatExpiryDate(this.cardForm.controls['year'].value)
  }

  get cardHolderName() {
    return this.cardForm.controls['name'].value || ''
  }
 
// Control getters (use these in *ngIf and [ngClass])
get number(): FormControl { return this.cardForm.get('number') as FormControl; }
get name(): FormControl   { return this.cardForm.get('name') as FormControl; }
get month(): FormControl  { return this.cardForm.get('month') as FormControl; }
get year(): FormControl   { return this.cardForm.get('year') as FormControl; }
get cvv(): FormControl    { return this.cardForm.get('cvv') as FormControl; }

}

