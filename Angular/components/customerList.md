###customer list
 
 
###customer-list.component.html
 
<div class="mt-75 layout-column justify-content-center align-items-center">
  <section class="layout-row align-items-center justify-content-center">
    <input type="text" class="large" placeholder="Name" data-test-id="app-input" [(ngModel)]="title"/>
    <button type="submit" class="ml-30" data-test-id="submit-button" (click)="addCustomer()">Add Customer</button>
  </section>
 
  <ul class="styled mt-50" data-test-id="customer-list" *ngIf="customers.length > 0">
    <li class="slide-up-fade-in"
        [attr.data-test-id]="'list-item'+ index"
        *ngFor="let customer of customers; let index = index;">{{customer}}</li>
  </ul>
 
</div>
 
 
 
###customer-list.component.ts
 
import {Component, OnInit} from '@angular/core';
 
@Component({
  selector: 'app-customer-list',
  templateUrl: './customer-list.component.html',
  styleUrls: ['./customer-list.component.scss'],
  standalone: false
})
export class CustomerListComponent implements OnInit {
  customers: string[] = [];
  title: string = '';
  ngOnInit() {
  }
  addCustomer(){
    if(this.title.trim()) {
      this.customers.push(this.title);
      this.title = '';
    }
  }
 
}
 
 
 
 
 