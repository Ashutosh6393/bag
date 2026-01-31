---------Stocks data-------

stocks.html

<div class="layout-column align-items-center mt-50">
  <section class="layout-row align-items-center justify-content-center">
    <input type="text" class="large" placeholder="5-January-2000"
           id="app-input" data-test-id="app-input" [(ngModel)]="dateInput"/>
    <button class="" id="submit-button" data-test-id="submit-button" (click)="onSearch()">Search</button>
  </section>

  <ul class="mt-50 styled slide-up-fade-in" id="stockData" data-test-id="stock-data" *ngIf="responseData.length>0">
    <li class="py-10">Open: {{responseData[0]?.open}}</li>
    <li class="py-10">Close: {{responseData[0]?.close}}</li>
    <li class="py-10">High: {{responseData[0]?.high}}</li>
    <li class="py-10">Low: {{responseData[0]?.low}}</li>
  </ul>

  <div class="mt-50 slide-up-fade-in"
       id="no-result" data-test-id="no-result" *ngIf="responseData.length==0 && hasSearched">No Results Found</div>
</div>


//ts

import {Component, OnInit} from '@angular/core';
import {HttpClient} from "@angular/common/http";
import { Observable } from 'rxjs';


@Component({
  selector: 'stock-data',
  templateUrl: './stockData.component.html',
  styleUrls: ['./stockData.component.scss'],
  standalone: false
})
export class StockData implements OnInit {
  dateInput='';
  responseData:Data[]=[];
  hasSearched=false;
  constructor(private http: HttpClient) {
  }

  ngOnInit() {

  }
  getDate(date):Observable<ApiResponse>{
    return this.http.get<ApiResponse>(`https://jsonmock.hackerrank.com/api/stocks?date=${this.dateInput}`);
  }
  onSearch(){
    var d=this.dateInput;
    this.hasSearched=true;
    this.getDate(d).subscribe({
      next:(resp)=>{
        this.responseData=resp?.data??[];
        console.log(this.responseData);
      }
    })
    
  }
}

interface Data {
  date: string;
  open: number;
  high: number;
  low: number;
  close: number;
}

interface ApiResponse {
  page: number;
  per_page: number;
  total: number;
  total_pages: number;
  data: Data[];
}
