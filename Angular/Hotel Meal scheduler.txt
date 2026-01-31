-----------Hotel Meal scheduler------------


///Guest form.html

<div class="layout-column">
  <div class="layout-row align-items-center justify-content-center mt-50">
    <input class="mr-30" type="text" data-test-id="name-input" required placeholder="Guest Name" [(ngModel)]="name"/>
    <input class="mr-30" type="date" data-test-id="start-date" required placeholder="Start Date" [(ngModel)]="startDate" />
    <input class="mr-30" type="date" data-test-id="end-date" required placeholder="End Date" [(ngModel)]="endDate" />
  </div>
  <div class="layout-row align-items-center justify-content-center mt-50">
    <button data-test-id="submit-button" (click)="AddButton()" class="w-30" >Add to Menu</button>
  </div>
</div>


///guest.ts


import { Component, Input, OnInit, Output, EventEmitter } from "@angular/core";
import { Guest } from '../app.component';

@Component({
  selector: "guest-form",
  templateUrl: "./guestForm.component.html",
  styleUrls: ["./guestForm.component.scss"],
  standalone: false
})
export class GuestForm implements OnInit {
  @Output() onGuestAdded: EventEmitter<Guest> = new EventEmitter<Guest>();
  name:string='';
  startDate:string='';
  endDate:string='';
  
  constructor() {}

  ngOnInit() {

  }
  AddButton(){
    const guest:Guest={
      name:this.name,
      startDate:this.startDate,
      endDate:this.endDate
    }
      this.onGuestAdded.emit(guest);
    this.name='';
    this.startDate='';
    this.endDate='';

  }
  
}



////MealScheduler.html


<div class="layout-column align-items-center justify-content-center">
    <section class="content layout-row align-items-center justify-content-center mt-50">
      <table class="card content">
        <thead>
          <tr>
            <th>Date</th>
            <th>Breakfast</th>
            <th>Lunch</th>
            <th>Dinner</th>
          </tr>
        </thead>
        <tbody data-test-id="meal-schedule"  >
          <tr class="slide-up-fade-in" id="schedule-{{idx}}" *ngFor="let guest of guestList; let idx=index">
            <!-- Use below to render row for each object-->
            <td data-test-id="date">{{guest.date}}</td>


              <td>
                <ul data-test-id="breakfast-list">
                  <li *ngFor="let g of guest.guests">{{g.name}}</li>
                </ul>
              </td>
              <td>
                <ul data-test-id="lunch-list">
                  <li *ngFor="let g of guest.guests">{{g.name}}</li>
                </ul>
              </td>
              <td>
                <ul data-test-id="dinner-list">
                  <li *ngFor="let g of guest.guests">{{g.name}}</li>
                </ul>
              </td>
    
          </tr>
        </tbody>
      </table>
    </section>
</div>


///MealScheduler.ts

import { Component, Input, OnInit,OnChanges } from "@angular/core";
import { Guest, MealScheduleList } from '../app.component';

@Component({
  selector: "meal-schedule",
  templateUrl: "./mealSchedule.component.html",
  styleUrls: ["./mealSchedule.component.scss"],
  standalone: false
})
export class MealSchedule implements OnInit {
  @Input() guestList:MealScheduleList[];

  newList:any[]=[];
  
  constructor() {
    

  }

ngOnChanges(){

  console.log(this.guestList);
}
  ngOnInit() {
  }
}


///app.html

<h8k-navbar header="Hotel Meal Schedule"></h8k-navbar>
<guest-form (onGuestAdded)="onGuestAdded($event)"></guest-form>
<meal-schedule [guestList]="mealListApp"></meal-schedule>


//app.ts

import {Component} from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss'],
  standalone: false
})

export class AppComponent {
  title = 'Hotel Meal Schedule';
  mealListApp:MealScheduleList[]=[];
  onGuestAdded(guest) {
      const s=new Date(guest.startDate);
      const e=new Date(guest.endDate);

      for(let d=new Date(s);d<=e;d.setDate(d.getDate()+1)){
        const dateStr=d.toISOString().split("T")[0];
        let dayEntry=this.mealListApp.find(m=>m.date===dateStr);
        if(!dayEntry){
          dayEntry={date:dateStr,guests:[]};
          this.mealListApp.push(dayEntry);
        }
        dayEntry.guests.push({
          name:guest.name,
          startDate:guest.startDate,
          endDate:guest.endDate,
        });
      }
      this.mealListApp.sort((a,b)=>a.date.localeCompare(b.date));

  }
}
export interface MealScheduleList{
  date:string;
  guests:Guest[];
}
export interface Guest {
  name: string;
  startDate: string;
  endDate: string;
}
