Flight Validation

Reactive-Form.ts

import { CommonModule } from '@angular/common';
import { Component } from '@angular/core';
import { FormControl, FormGroup, ReactiveFormsModule, Validators } from '@angular/forms';

@Component({
  selector: 'app-reactive-form-validation',
  imports: [ReactiveFormsModule,CommonModule],
  templateUrl: './reactive-form-validation.html',
  styleUrl: './reactive-form-validation.css',
})
export class ReactiveFormValidation {
    flightsForm= new FormGroup({
    number: new FormControl('',[Validators.required,Validators.pattern(/^\d{3}$/)]),
    source: new FormControl('',[Validators.required,Validators.minLength(3),Validators.pattern(/^[A-Za-z]+$/)]),
    destination: new FormControl('',[Validators.required,Validators.minLength(3),Validators.pattern(/^[A-Za-z]+$/)])
  })
get number() {
  return this.flightsForm.get('number') as FormControl;
}
 
get source() {
  return this.flightsForm.get('source') as FormControl;
}
get destination() {
  return this.flightsForm.get('destination') as FormControl;
}
}

html

<h1>Reactive</h1>
<div>
  <form [formGroup]="flightsForm">
    <label>
      <input type="text" formControlName="number" placeholder="Flight Number" />
      <div *ngIf="(number.touched || number.dirty) && number.invalid">
        <div *ngIf="number.errors?.['required']">Flight Number is required</div>
        <div *ngIf=" number.errors?.['pattern'] ">Flight Number should be a 3 digit number</div>
      </div>
    </label>
    <label>
      <input type="text" formControlName="source" placeholder="Source" />
      <div *ngIf="(source.touched || source.dirty) && source.invalid">
        <div *ngIf="source.errors?.['required']">Source is required</div>
        <div *ngIf="source.errors?.['pattern']">Source should only contain letters</div>
        <div *ngIf="source.errors?.['minlength']">
          Source should contain a minimum of 3 characters
        </div>
      </div>
    </label>
    <label>
      <input type="text" formControlName="destination" placeholder="Destination" />
      <div *ngIf="(destination.touched || destination.dirty) && destination.invalid">
        <div *ngIf="destination.errors?.['required']">Destination is required</div>
        <div *ngIf="destination.errors?.['pattern']">Destination should only contain letters</div>
        <div *ngIf="destination.errors?.['minlength']">
          Destination should contain a minimum of 3 characters
        </div>
      </div>
    </label>
    <button type="submit" [disabled]="flightsForm.invalid">Submit</button>
  </form>
</div>


Template-form
import { CommonModule } from '@angular/common';
import { Component } from '@angular/core';
import { FormsModule, NgForm } from '@angular/forms';

@Component({
  selector: 'app-template-driven-form-validation',
  imports: [FormsModule,CommonModule],
  templateUrl: './template-driven-form-validation.html',
  styleUrl: './template-driven-form-validation.css',
})
export class TemplateDrivenFormValidation {
   flightNumber: string = '';
  source: string = '';
  destination: string = '';
  submitted: boolean = false;
 
  onSubmit(form: NgForm) {
    this.submitted = true;
    if (form.valid) {
      console.log('Form Submitted!', form.value);
    }
  }
}


html

<div>
  <h2>Flight Validation</h2>
 
  <form #flightForm="ngForm" (ngSubmit)="onSubmit(flightForm)">
 
    <div>
      <label>Flight Number</label>
      <input
        name="flightNumber"
        placeholder="Enter Name"
        [(ngModel)]="flightNumber"
        #flightNumberCtrl="ngModel"
        required
        pattern="^\d{3}$"
      />
      <div *ngIf="(flightNumberCtrl.touched || flightNumberCtrl.dirty) && flightNumberCtrl.invalid">
        <div *ngIf="flightNumberCtrl.errors?.['required']">Flight Number is required</div>
        <div *ngIf="flightNumberCtrl.errors?.['pattern']">Flight Number should be a 3 digit number</div>
      </div>
    </div>
 
    <div>
      <label>Source</label>
      <input
        name="source"
        placeholder="Enter Source"
        [(ngModel)]="source"
        #sourceCtrl="ngModel"
        required
        minlength="3"
        pattern="^[A-Za-z]+$"
      />
      <div *ngIf="(sourceCtrl.touched || sourceCtrl.dirty) && sourceCtrl.invalid">
        <div *ngIf="sourceCtrl.errors?.['required']">Source is required</div>
        <div *ngIf="sourceCtrl.errors?.['minlength']">Source should contain a minimum of 3 characters</div>
        <div *ngIf="sourceCtrl.errors?.['pattern']">Source should only contain letters</div>
      </div>
    </div>
 
    <div>
      <label>Destination</label>
      <input
        name="destination"
        placeholder="Enter destination"
        [(ngModel)]="destination"
        #destinationCtrl="ngModel"
        required
        minlength="3"
        pattern="^[A-Za-z]+$"
      />
      <div *ngIf="(destinationCtrl.touched || destinationCtrl.dirty) && destinationCtrl.invalid">
        <div *ngIf="destinationCtrl.errors?.['required']">Destination is required</div>
        <div *ngIf="destinationCtrl.errors?.['minlength']">Destination should contain a minimum of 3 characters</div>
        <div *ngIf="destinationCtrl.errors?.['pattern']">Destination should only contain letters</div>
      </div>
    </div>
 
    <button type="submit" data-test-id="submit-button" [disabled]="flightForm.invalid">Submit</button>
  </form>
</div>

