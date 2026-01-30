## navigation.component.html

<div class="layout-row align-items-center justify-content-center navigation-screen">
  <div class="card layout-row flat map-card">
    <section class="card pb-16 pr-16 flex-auto layout-column justify-content-center">
      <ul class="pl-0" data-test-id="location-list">
<!--   Use this `li` for rendering each location item as it contains all the data-test-id attributes required for the tests to pass     -->

       <li *ngFor="let location of locations; let index=index" [attr.data-test-id]="'location-' + index"
           class="layout-row justify-content-between align-items-center mr-8 pl-40 relative">
         <div class="layout-column justify-content-start align-items-center handle">
           <i [ngClass]="this.getClasses('marker', index)">
             {{this.isLast(index) ? 'room' : 'radio_button_checked' }}
           </i>
           <i [ngClass]="this.getClasses('dots', index)">more_vert</i>
         </div>
         <div class="location-name">
           <p class="caption text-start mb-4" data-test-id="location">{{location}}</p>
         </div>
         <div>

           <button *ngIf="index!=0" (click)="moveUp(index)" class="icon-only small mx-0" data-test-id="up-button">
             <i class="material-icons">arrow_upward</i>
           </button>


           <button *ngIf="index!=locations.length-1" (click)="moveDown(index)" class="icon-only small mx-0" data-test-id="down-button">
             <i class="material-icons">arrow_downward</i>
           </button>
         </div>
       </li>
      </ul>
    </section>
    <section class="flex-auto">
      <img src="assets/images/map.svg" class="fill" alt="map"/>
    </section>
  </div>

</div>

## navigation.component.ts

import {Component, Input} from '@angular/core';

@Component({
  selector: 'app-navigation',
  templateUrl: './navigation.component.html',
  styleUrls: ['./navigation.component.scss'],
  standalone: false
})
export class NavigationComponent {
  @Input() locations: string[];

  moveDown(index){
    if(index<this.locations.length-1){
      let x = this.locations[index]
      let y = this.locations[index+1];
      this.locations[index]= y;
      this.locations[index+1]=x;
    }

  }

  moveUp(index){
    if(index>0){
      let x = this.locations[index]
      let y = this.locations[index-1];
      this.locations[index]= y;
      this.locations[index-1]=x;
    }

  }

  // Used for rendering
  getClasses(ctx, index) {
    let classes = `material-icons ${ctx}`;
    if (ctx === 'dots') {
      if (this.isLast(index)) {
        classes += ' hidden';
      }
    } else {
      classes += this.isLast(index) ? ' small' : ' x-small';
      if (index === 0) {
        classes += ' first';
      }
    }
    return classes;
  }

  // Used for rendering
  isLast(index) {
    return index === this.locations.length - 1;
  }
}
