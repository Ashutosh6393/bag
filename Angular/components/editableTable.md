
## editableTable.component.html


<div class="card w-45 mx-auto mt-75 pb-5">
  <table data-test-id='table'>
    <thead>
      <tr>
        <th>Name</th>
        <th>Position</th>
        <th>Salary</th>
        <th>Action</th>
      </tr>
    </thead>
    <tbody>
      <tr *ngFor="let employee of employeesList; let idx=index" [attr.data-test-id]="'row' + idx">
        <td>{{employee.name}}</td>
        <td class='pl-20'>{{employee.position}}</td>
        <td class='pl-20'>
        <div *ngIf="selected != idx" (click)="changeSalary(idx, employee.salary)" [attr.data-test-id]="'employee-salary-div-' + idx">{{employee.salary}}</div>
        <input
          *ngIf="selected === idx"
          [(ngModel)]="newSalary"
          [attr.data-test-id]="'employee-salary-input-' + idx" type='number'
        />
        </td>
        <td class='pl-20'>
          <button
          [disabled]="(!(idx === selected) || newSalary===employee.salary)"
            class="x-small w-75 ma-0 px-25"
            [attr.data-test-id]="'employee-save-button-' + idx"
            (click)="onSave()"
          > Save
          </button>
        </td>
      </tr>
      <tr>
        <td class='pl-30'>
          <input
          [(ngModel)]="name"
            data-test-id='new-employee-name-input'
            placeholder='Enter Name'
          />
        </td>
        <td class='pl-20'>
          <input
           [(ngModel)]="position"
            data-test-id='new-employee-position-input'
            placeholder='Enter Position'
          />
        </td>
        <td class='pl-20'>
          <input
           [(ngModel)]="salary"
            data-test-id='new-employee-salary-input'
            type='number'
            placeholder='Enter Salary'
          />
        </td>
        <td class='pl-20'>
          <button
            data-test-id='add-new-employee-button'
            class='x-small w-75 ma-0 px-25'
            [disabled]="!(name.length>0 && position.length>0 && salary>0)"
            (click)="onAdd()"
          >
            Add
          </button>
        </td>
      </tr>
    </tbody>
  </table>
</div>


## editableTable.component.ts

  import { Component, OnInit } from "@angular/core";
  import { IEmployee } from '../app.component';

  @Component({
    selector: "editable-table",
    templateUrl: "./editableTable.component.html",
    styleUrls: ["./editableTable.component.scss"],
    standalone: false
  })
  export class EditableTable implements OnInit {
    employeesList: IEmployee[];

    name: string = "";
    position: string = "";
    salary: number = null;

    newSalary: number = null;
    selected: number = null;
    
    constructor() {
      this.employeesList = [
        { id: 0, name: 'Chris Hatch', position: 'Software Developer', salary: 130000 },
        { id: 1, name: 'Elizabeth Montgomery', position: 'Lead Research Engineer', salary: 70000 },
        { id: 2, name: 'Aiden Shaw', position: 'Machine Learning Engineer', salary: 80000 },
      ]
    }

    onAdd(){
      this.employeesList.push({id: this.employeesList.length + 1, name: this.name, position: this.position, salary: this.salary})
      this.name = "";
      this.position = "";
      this.salary = null;
    }

    onSave(){
      this.employeesList = this.employeesList.map(e => {
        if(e.id === this.selected){
          return {
            ...e,
            salary: this.newSalary
          }
        }
        return e;
      })

      this.selected = null;
      this.newSalary = null;
    }


    changeSalary(id, sal){
      this.newSalary  = sal;
      this.selected = id;
    }

    ngOnInit() {
    }
  }
