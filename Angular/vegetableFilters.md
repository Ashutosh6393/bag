## vegetableFilter.component.ts

import {Component, Input, OnInit} from '@angular/core';

@Component({
  selector: 'vegetable-filter',
  templateUrl: './vegetableFilter.component.html',
  styleUrls: ['./vegetableFilter.component.scss'],
  standalone: false
})

export class VegetableFilter implements OnInit {
  @Input() vegetables: string[];
  searchText: string = "";
  filteredVegetables: string[];



  onInput(){
    if(this.searchText.length === 0){
      this.filteredVegetables = [...this.vegetables]
    }
    this.filteredVegetables = this.vegetables.filter( e => e.toLowerCase().startsWith(this.searchText.toLowerCase()));
  }

  ngOnInit() {
    
      this.filteredVegetables = [...this.vegetables];

  }
}

## vegetableFilter.component.html

<div class="layout-column align-items-center mt-50">
  <section class="layout-row align-items-center justify-content-center">
    <input type="text" class="large" placeholder="Enter veg eg carrot"
          data-test-id="app-input" [(ngModel)]="searchText" (input)="onInput()" />
  </section>
​
  <ul *ngIf="filteredVegetables.length>0" class="mt-50 slide-up-fade-in styled" data-test-id="filtered-vegetables" >
    <li class="py-10" *ngFor="let vege of filteredVegetables">{{vege}}</li>
  </ul>
​
  <div *ngIf="filteredVegetables.length===0" class="mt-50 slide-up-fade-in" data-test-id="no-result">No Results Found</div>
</div>
