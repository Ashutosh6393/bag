<div class="data layout-column align-items-center mt-50">
  <section class="layout-row align-items-center justify-content-center">
    <button data-test-id="submit-button" (click)="getData()">Get Date</button>
  </section>
 
  <section *ngIf="date" class="mt-20 layout-row align-items-center justify-content-center">
    <div class="card outlined">
      <div class="card-text pt-10 layout-column align-items-center justify-content-center">
        <div class="mt-10 outlined" data-test-id="date">
          Date: {{date}}
        </div>
        <div *ngIf="HolidayName" class="mt-10 outlined" data-test-id="holiday">
          <b>{{HolidayName}}</b>
        </div>
        <div *ngIf="!HolidayName" data-test-id="holiday">NOT A HOLIDAY</div>
        <div *ngIf="HolidayName" class="mt-10 outlined" data-test-id="day">
          Day: {{day}}
        </div>
      </div>
    </div>
  </section>
</div>
 



import {Component, OnInit, Input} from '@angular/core';
import {HttpClient} from "@angular/common/http";
import { Holiday } from '../app.component';
 
interface ApiResponse {
  date: string;
  time: string
}
 
@Component({
  selector: 'holiday-checker',
  templateUrl: './holidayChecker.component.html',
  styleUrls: ['./holidayChecker.component.scss'],
  standalone: false
})
export class HolidayChecker implements OnInit {
  @Input() holidayList: Holiday[];
  URL = `https://jsonmock.hackerrank.com/datetime`;
 
  constructor(private http: HttpClient) {
  }
date:string='';
day:string='';
HolidayName:string='';
  getData(){
    this.http.get<ApiResponse>(this.URL).subscribe(res=>{
      this.date = res.date
      let x = this.holidayList.find(U=>U.date === res.date)
      this.day = x.day
      this.HolidayName = x.name
    })
    console.log(this.date);
  }
  ngOnInit() {
 
  }
}
 
 