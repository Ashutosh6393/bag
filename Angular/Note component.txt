------Note component-------

Notes.html

<div class="layout-column align-items-center justify-content-start">
  <section class="layout-row align-items-center justify-content-center mt-30">
    <input data-test-id="input-note-title" type="text" class="large mx-8"
           placeholder="Note Title" #title="noteTitle"/>
    <input data-test-id="input-note-status" type="text" class="large mx-8"
           placeholder="Note Status" #status="noteStatus"/>
    <button class="" data-test-id="submit-button" (click)="onAdd(title,status)">Add Note</button>
  </section>

  <div class="mt-50">
    <ul class="tabs">
      <li class="tab-item slide-up-fade-in"
          [attr.data-test-id]="status.toLowerCase() + 'Button'"
          [ngClass]="{'active' : selectedTab.toLowerCase() === status.toLowerCase()}"
          *ngFor="let status of statusList;">
        {{status}}
      </li>
    </ul>
  </div>
  <div class="card w-40 pt-30 pb-8">
    <table>
      <thead>
      <tr>
        <th>Title</th>
        <th>Status</th>
      </tr>
      </thead>
      <tbody data-test-id="notesList">
      <tr>

      </tr>
      </tbody>
    </table>
  </div>
</div>


---notes.ts

import {Component, OnInit} from '@angular/core';

@Component({
  selector: 'app-notes',
  templateUrl: './notes.component.html',
  styleUrls: ['./notes.component.scss'],
  standalone: false
})
export class NotesComponent implements OnInit {
  statusList: string[] = ['All', 'Active', 'Completed'];
  selectedTab: string = 'All';
  NoteList:Note[]=[];
  noteTitle:string="";
  noteStatus:string="";

  ngOnInit(): void {
  }
  onAdd(title:string,status:string){
    this.NoteList.push({title,status});
  }

}

export interface Note {
  title: string;
  status: string
}


