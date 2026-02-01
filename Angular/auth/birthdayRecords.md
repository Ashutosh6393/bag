## birthdayRecords.component.html

<div class="layout-column align-items-center mt-20">

  <div class="layout-row align-items-center justify-content-center mt-50">
    <label class="pr-10">Sort By</label>
    <form>
        <input type="radio" name="sort" (click)="sortByName()" data-test-id="name"/>
        <label class="px-4">Name</label>
        <input type="radio" name="sort" (click)="sortByDate()" data-test-id="age"/>
        <label class="px-4">Age</label>
    </form>
  </div>

  <div class="layout-row align-items-center justify-content-center mt-30 w-50">
    <div class="card w-50 mx-auto mt-20 pb-30">
      <table>
        <thead>
          <tr>
            <th>Person Name</th>
            <th>Date of Birth</th>
          </tr>
        </thead>
        <tbody data-test-id="table">
          <tr *ngFor="let x of peopleList">
            <td>{{x.name}}</td>
            <td>{{x.birth}}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</div>


## birthdayRecords.component.ts

import { Component, Input, OnInit } from "@angular/core";
import { People } from "../app.component";

@Component({
  selector: "birthday-records",
  templateUrl: "./birthdayRecords.component.html",
  styleUrls: ["./birthdayRecords.component.scss"],
  standalone: false,
})
export class BirthdayRecords implements OnInit {
  @Input() peopleList: People[];

  sortByName() {
    this.peopleList.sort((a, b) => a.name.localeCompare(b.name));
  }

  sortByDate() {
    this.peopleList.sort(
      (a, b) => new Date(a.birth).getTime() - new Date(b.birth).getTime(),
    );
  }
  ngOnInit() {}
}
