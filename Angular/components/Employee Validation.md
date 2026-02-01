Employee Validation
<div class="layout-column justify-contents-center align-items-center">
  <form #empForm="ngForm" (ngSubmit)="onSubmit(empForm)"  class="layout-column" novalidate>
        <label>
            <input
              type="text"
              name="name"
              [(ngModel)]="name"
              #nameCtrl="ngModel"           
              data-test-id="name-input"
              class="white large outlined"
              placeholder="Name"
              required
              minlength="2"
            />
            <div *ngIf="(nameCtrl.touched || nameCtrl.dirty)&& nameCtrl.invalid" data-test-id="name-input-error" class="error-text">
                <div *ngIf="nameCtrl.errors?.['required']">Name is required</div>
                <div *ngIf="nameCtrl.errors?.['minlength']">Name should contain a minimum of 2 characters</div>
            </div>
        </label>
        <label>
            <input
              type="email"
              name="email"
              [(ngModel)]="email"
              #emailCtrl="ngModel"
              data-test-id="email-input"
              
              class="white large outlined"
            
              placeholder="Email"
              required
              pattern="^[A-Za-z][A-Za-z0-9.]{1,9}@[A-Za-z]{2,10}\.[A-Za-z]{2,10}$"
              
            />
            <div *ngIf="(emailCtrl.touched || emailCtrl.dirty) && emailCtrl.invalid" class="error-text" data-test-id="email-input-error">
              <div *ngIf="emailCtrl.errors?.['required']">Email is required</div>
              <div *ngIf="emailCtrl.errors?.['pattern']">Email has invalid pattern</div>
            </div>
        </label>
        <label>
            <input
              type="text"
              name="department"
              [(ngModel)]="department"
              #deptCtrl="ngModel"
           
              data-test-id="department-input"
              class="white large outlined"
              placeholder="Department"
              required
              pattern="^(?!.*[Ee][Xx][Aa][Mm][Pp][Ll][Ee]).*$"
            />
            <div *ngIf="(deptCtrl.touched || deptCtrl.dirty)&& deptCtrl.invalid" data-test-id="department-input-error" class="error-text">
              <div *ngIf="deptCtrl.errors?.['required']">Department is required</div>
              <div *ngIf="deptCtrl.errors?.['pattern']">Department cannot contain Example</div>
            </div>
        </label>
        <button [disabled]="empForm.invalid"  class="w-30 mt-20" type="submit" data-test-id="submit-button">
            Submit
        </button>
  </form>
</div>




-------------------------Ts File-----------------------------------------------------------------------------------------------
import {Component, OnInit} from '@angular/core';
import { FormGroup,  FormBuilder,  Validators } from '@angular/forms';

@Component({
  selector: 'employee-validation',
  templateUrl: './employeeValidation.component.html',
  styleUrls: ['./employeeValidation.component.scss'],
  standalone: false
})

export class EmployeeValidation{

  name:string='';
  email:string='';
  department:string='';
  submitted: boolean = false;
  constructor() {
    
  }

  onSubmit(form:any){
    this.submitted=true;
  }
}




