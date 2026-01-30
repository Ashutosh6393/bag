## 2 files changed

## kanbanBoard.component.html

<div class="mt-20 layout-column justify-content-center align-items-center">
    <section class="mt-50 layout-row align-items-center justify-content-center">
      <input [(ngModel)]="newTask" id="create-task-input" type="text" class="large" placeholder="New task name" data-test-id="create-task-input"/>
      <button (click)="onAdd()" type="submit" class="ml-30" data-test-id="create-task-button">Create task</button>
    </section>

    <div class="mt-50 layout-row">
      <div class="card outlined ml-20 mt-0" *ngFor="let tasks of stagesTasks;index as i;">
        <div class="card-text">
          <h4>{{this.stagesNames[i]}}</h4>
          <ul class="styled mt-50" [attr.data-test-id]="'stage-'+ i">
            <li *ngFor="let task of tasks; index as index;">
              <div class="li-content layout-row justify-content-between align-items-center">
                  <span [attr.data-test-id]="generateTestId(task.name)+ '-name'">{{task.name}}</span>
                  <div class="icons">
                    <button [disabled]="task.stage === 0" (click)="moveLeft(task.name)" class="icon-only x-small mx-2"
                            [attr.data-test-id]="generateTestId(task.name)+ '-back'">
                      <i class="material-icons">arrow_back</i>
                    </button>

                    <button [disabled]="task.stage === 3" (click)="moveRight(task.name)" class="icon-only x-small mx-2"
                            [attr.data-test-id]="generateTestId(task.name)+ '-forward'">
                      <i class="material-icons">arrow_forward</i>
                    </button>

                    <button (click)="onDelete(task.name)" class="icon-only danger x-small mx-2"
                            [attr.data-test-id]="generateTestId(task.name)+ '-delete'">
                      <i class="material-icons">delete</i>
                    </button>
                  </div>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>


## kanbanBoard.component.ts

import {Component, OnInit} from '@angular/core';

@Component({
  selector: 'kanban-board',
  templateUrl: './kanbanBoard.component.html',
  styleUrls: ['./kanbanBoard.component.scss'],
  standalone: false
})
export class KanbanBoard implements OnInit {
  tasks: Task[];
  stagesNames: string[];
  stagesTasks: any[]; //Only used for rendering purpose

  newTask: string = "";

  onAdd(){
    if(this.newTask.trim()){
      this.tasks.push({name: this.newTask, stage: 0});
      this.configureTasksForRendering()
    }
    this.newTask = "";
  }

  ngOnInit() {
    // Each task is uniquely identified by its name. 
    // Therefore, when you perform any operation on tasks, make sure you pick tasks by names (primary key) instead of any kind of index or any other attribute.
    this.tasks = [
      { name: '0', stage: 0 },
      { name: '1', stage: 0 },
    ];
    this.stagesNames = ['Backlog', 'To Do', 'Ongoing', 'Done'];
    this.configureTasksForRendering();
  }
  onDelete(name){
    this.tasks = [...this.tasks].filter(e => e.name != name)
    this.configureTasksForRendering();

  }

  moveLeft(name){
    this.tasks = [...this.tasks].map(e => {
      if(e.stage != 0 && e.name === name){
        return {
          ...e,
          stage: e.stage-1
        }
      }
      return e;
    })
    console.log(this.tasks)

    this.configureTasksForRendering();

  }

  moveRight(name){
    this.tasks =  [...this.tasks].map(e => {
      if(e.stage != 3 && e.name === name){
        return {
          ...e,
          stage: e.stage+1
        }
      }
      return e;
    })

    console.log(this.tasks)
    this.configureTasksForRendering();
  }
  
  // this function has to be called whenever tasks array is changed to construct stagesTasks for rendering purpose
  configureTasksForRendering = () => {
    this.stagesTasks = [];
    for (let i = 0; i < this.stagesNames.length; ++i) {
      this.stagesTasks.push([]);
    }
    for (let task of this.tasks) {
      const stageId = task.stage;
      this.stagesTasks[stageId].push(task);
    }
  }

  generateTestId = (name) => {
    return name.split(' ').join('-');
  }
}

interface Task {
  name: string;
  stage: number;
}