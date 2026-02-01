

<section class="flex">
  <!-- Each grouping should be rendered in the below class="card" -->
  <div *ngFor="let task of tasks" class="card" data-test-id="task-card">
      <section class="card-text">
        <div class="layout-row justify-content-around align-items-center">

          <!-- Use below h3 to display the user name or status -->
          <h3 class="mt-0 mb-12">{{task.x}}</h3>
        </div>
        <div *ngFor="let x of task.task" class="layout-row align-items-center">
          <!-- Use below div to display the tasks -->
          <div class="task-name mb-8">{{x}}</div>
        </div>
      </section>
  </div>
</section>


import {Component, Input} from '@angular/core';
import {ITasks} from "../../app.service";

@Component({
  selector: 'task-card-list',
  templateUrl: './taskCard.component.html',
  styleUrls: ['./taskCard.component.scss'],
  standalone: false
})

export class TaskCard {
  @Input() tasks;
}




<div class="layout-row align-items-center justify-content-center mt-30">
  <section class="layout-column">
    <input [(ngModel)]="name" type="text" data-test-id="app-input-userName" placeholder="User Name" />
    <input [(ngModel)]="task" type="text" data-test-id="app-input-task" placeholder="Task" />
    <input [(ngModel)]="status" type="text" data-test-id="app-input-status" placeholder="Status" />
    <div class="layout-row justify-content-center mt-20">
      <button [disabled]="name.length===0 || task.length===0 || status.length===0" (click)="onSubmit()" data-test-id="submit-button">Submit</button>
    </div>
  </section>
</div>



import { Component, OnInit } from "@angular/core";
import { AppService, ITasks } from "../app.service";
import { FormControl, FormGroup, Validators } from "@angular/forms";

@Component({
  selector: "create-tasks",
  templateUrl: "./createTasks.component.html",
  styleUrls: ["./createTasks.component.scss"],
  standalone: false,
})
export class CreateTasks implements OnInit {
  name: string= "";
  task: string= "";
  status: string= "";

  onSubmit() {
    this.appService.tasksList.push({
      status: this.status,
      task: this.task,
      userName: this.name,
    });

    let existsInUser = this.appService.userList.find(e => e.x === this.name)
    if(!existsInUser){
      this.appService.userList.push({x: this.name, task: [`${this.task}`]})
    }else{
      existsInUser.task.push(this.task);
    }

    let existsInStatus = this.appService.statusList.find(e => e.x === this.status)
    if(!existsInStatus){
      this.appService.statusList.push({x: this.status, task: [`${this.task}`]})
    }else{
      existsInStatus.task.push(this.task);
    }


    this.name = "";
    this.task = "";
    this.status = "";
  }

  constructor(private appService: AppService) {}

  ngOnInit() {}
}




<div class="layout-row justify-content-center mt-30">
  <task-card-list [tasks]="tasks"></task-card-list>
</div>



import {Component, OnInit} from '@angular/core';
import {AppService, ITasks} from '../app.service';

@Component({
  selector: 'status-tasks',
  templateUrl: './statusTasks.component.html',
  styleUrls: ['./statusTasks.component.scss'],
  standalone: false
})

export class StatusTasks implements OnInit {
  tasks;
  constructor(private appService: AppService) {
  }

  ngOnInit() {
    this.tasks = this.appService.statusList;
  }

}


<div class="layout-row justify-content-center mt-30">
  <task-card-list [tasks]="tasks"></task-card-list>
</div>


import {Component, OnInit} from '@angular/core';
import {AppService, ITasks} from '../app.service';

@Component({
  selector: 'user-tasks',
  templateUrl: './userTasks.component.html',
  styleUrls: ['./userTasks.component.scss'],
  standalone: false
})

export class UserTasks implements OnInit {
  tasks;
  constructor(private appService: AppService) {
    this.tasks = this.appService.userList;
  }

  ngOnInit() {
    
  }

}



<h8k-navbar header="Task Logger"></h8k-navbar>

<nav class="layout-row justify-content-center mt-30" data-test-id="app-nav">
    <a routerLinkActive="active" [routerLink]="['/']" data-test-id="create-task">Create Task</a>
    <a routerLinkActive="active" [routerLink]="['/tasks/user']" data-test-id="view-tasks-by-user-route-link">View Tasks by User</a>
    <a routerLinkActive="active" [routerLink]="['/tasks/status']" data-test-id="view-tasks-by-status-route-link">View Tasks by Status</a>
</nav>

<router-outlet></router-outlet>
