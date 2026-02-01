<section class="layout-column align-items-center mt-50">
  <input (input)="onSearch()" [(ngModel)]="countryInput" type="text" class="large" placeholder="Enter Country Name"
           id="app-input" data-test-id="app-input" />
 
  <ul class="card country-list" data-test-id="countryList" *ngIf="filterList.length > 0">
    <li *ngFor="let country of filterList" class="pa-10 pl-20">{{country}}</li>
  </ul>
 
  <div class="mt-50 slide-up-fade-in" id="no-result" data-test-id="no-result" *ngIf="filterList.length === 0">No Results Found</div>
</section>
 
 

import {Component, Input, OnInit} from '@angular/core';
 
@Component({
  selector: 'country-filter',
  templateUrl: './countryFilter.component.html',
  styleUrls: ['./countryFilter.component.scss'],
  standalone: false
})
 
export class CountryFilter implements OnInit {
  @Input() countryList: string[];
   filterList = [];
   countryInput:string='';
  ngOnInit() {
   this.filterList = this.countryList
  }
  onSearch(){
  if(this.countryInput ===''){
   this.filterList;
  }
  this.filterList = this.countryList.filter(x=>x.toLowerCase().includes(this.countryInput.toLowerCase()));
  }
}
 
 