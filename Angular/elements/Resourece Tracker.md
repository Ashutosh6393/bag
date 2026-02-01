----------------------Resourece Tracker-----------------


///ResourceList.html


<section class="content">
  <ul class="mr-20 matches styled">
    <li class="slide-up-fade-in survey-details layout-row">
        <span><b>RESOURCE</b></span>
        <span><b>LOCATION</b></span>
        <span><b>TOTAL CAPACITY</b></span>
        <span><b>AVAILABLE CAPACITY</b></span>
    </li>
    <li class="survey-details layout-row slide-up-fade-in" data-test-id="resource-item" *ngFor="let resource of resourceList">
        <!-- Use below to render list item for each resource object-->
        <span>{{resource.resource}}</span>
        <span>{{resource.location}}</span>
        <span>{{resource.totalCapacity}}</span>
        <span>{{resource.availableCapacity}}</span> 
    </li>
  </ul>
</section>


///ResourceList.ts

import {Component, Input} from '@angular/core';
import {IResources} from "../../app.service";

@Component({
  selector: 'resource-list',
  templateUrl: './resourceList.component.html',
  styleUrls: ['./resourceList.component.scss'],
  standalone: false
})

export class ResourceList {
  @Input() resourceList: IResources[];
}



///CreateResource.html

<div class="layout-row align-items-center justify-content-center mt-30">
  <section class="layout-column">
    <input type="text" data-test-id="app-input-resource" placeholder="Resource Name" [(ngModel)]="name"/>
    <input type="text" data-test-id="app-input-location" placeholder="Location" [(ngModel)]="location" />
    <input type="number" data-test-id="app-input-totalCapacity" placeholder="Total Capacity" [(ngModel)]="tCapacity" />
    <input type="number" data-test-id="app-input-availableCapacity" placeholder="Available Capacity" [(ngModel)]="aCapacity" />
    <div class="layout-row justify-content-center mt-20">
      <button data-test-id="submit-button" [disabled]="name.length===0 || location.length===0 || tCapacity===null || aCapacity===null " (click)="onSubmit()">Submit</button>
    </div>
  </section>
</div>



///CreateResource.ts

import {Component, OnInit} from '@angular/core';
import {AppService} from '../app.service';
import {FormControl, FormGroup, Validators} from "@angular/forms";

@Component({
  selector: 'createResource',
  templateUrl: './createResource.component.html',
  styleUrls: ['./createResource.component.scss'],
  standalone: false
})

export class CreateResource implements OnInit {

  name:string='';
  location:string='';
  tCapacity:number=null;
  aCapacity:number=null;
  constructor(private appService: AppService) {}

  ngOnInit() {
  }

  onSubmit(){
    this.appService.resources.push({resource:this.name,location:this.location,totalCapacity:this.tCapacity,availableCapacity:this.aCapacity});
    this.name='';
    this.location=''
    this.tCapacity=null
    this.aCapacity=null
  }
}


//HotResource.html


<div class="layout-row justify-content-center mt-30">
  <resource-list [resourceList]="resources"></resource-list>
</div>



//hotResource.ts


import {Component, OnInit} from '@angular/core';
import {AppService, IResources} from '../app.service';

@Component({
  selector: 'hot-resources',
  templateUrl: './hotResources.component.html',
  styleUrls: ['./hotResources.component.scss'],
  standalone: false
})

export class HotResources implements OnInit {
  resources: IResources[];
  constructor(private appService: AppService) {
    this.resources=appService.resources.filter(r=>(r.availableCapacity/r.totalCapacity)*100<20)
  }

  ngOnInit() {

  }

}


//ViewResources.html


<div class="layout-row justify-content-center mt-30">
  <resource-list [resourceList]="resources"></resource-list>
</div>


//ViewResources.ts


import {Component, OnInit} from '@angular/core';
import {AppService, IResources} from '../app.service';

@Component({
  selector: 'view-resources',
  templateUrl: './viewResources.component.html',
  styleUrls: ['./viewResources.component.scss'],
  standalone: false
})

export class ViewResources implements OnInit {
  resources: IResources[];
  constructor(private appService: AppService) {
    this.resources=appService.resources;
  }

  ngOnInit() {
  }

}




