## hotelsearch.html

<div class="layout-column">
  <div class="layout-row align-items-center justify-content-center mt-50">
    <section class="layout-row justify-content-center align-items-center">
        <input class="mr-30" type="text" data-test-id="city-input" required placeholder="City" [(ngModel)]="City"/>
 
        <input class="mr-30" type="text" data-test-id="room-type-input" required placeholder="Room Type"
        [(ngModel)]="RoomType" />
        <button data-test-id="search-button" class="w-30" (click)="onSubmit()">Search</button>
    </section>
  </div>
 
  <section  class="layout-row align-items-center justify-content-center mt-50" >
    <table *ngIf="searched && filterData.length>0" class="card content">
      <thead>
        <tr>
          <th>NAME</th>
          <th>PRICE</th>
          <th>DISCOUNT</th>
        </tr>
      </thead>
      <tbody data-test-id="hotel-results">
        <tr class="slide-up-fade-in" *ngFor="let hotel of filterData">
          <!--Use below to render list item for each hotel object-->
            <td>{{hotel.name}}</td>
            <td>{{hotel.price}}</td>
            <td>{{hotel.discount}}</td>
          </tr>
      </tbody>
    </table>
  </section>
 
  <section class="layout-row align-items-center justify-content-center mt-50" *ngIf="searched && filterData.length==0">
    <div data-test-id="no-hotels" class="no-hotels slide-up-fade-in">
        No hotels found
    </div>
  </section>
</div>

## hotelsearch.ts

import { Component, Input, OnInit } from "@angular/core";
import { HotelDetail } from '../app.component';
import { h } from "h8k-components/dist/types/stencil-public-runtime";
 
@Component({
  selector: "hotel-search",
  templateUrl: "./hotelSearch.component.html",
  styleUrls: ["./hotelSearch.component.scss"]
})
export class HotelSearch implements OnInit {
  @Input() hotelOptions: HotelDetail[];
  City=""
  RoomType=""
 
  filterData:HotelDetail[]=[];
  searched:boolean=false
  ngOnInit() {
  }
  onSubmit(){
    this.searched=true;
    this.City=this.City.toLowerCase();
    this.RoomType=this.RoomType.toLowerCase();
    this.filterData=this.hotelOptions.filter(h=>h.city.toLowerCase()==this.City &&
  h.roomType.toLowerCase()==this.RoomType);
   
  }
}
