## universities-by-rank.component.html

<div class="layout-column align-items-center mt-50">
  <section class="layout-row align-items-center justify-content-center">
    <input [(ngModel)]="rank" type="number" class="large" placeholder="Enter Rank eg 5"
           data-test-id="app-input"/>
    <button (click)="onSearch()" data-test-id="submit-button">Search</button>
  </section>

  <ul *ngIf="universityList && universityList.length > 0" class="mt-50 styled" data-test-id="universities-list">
    <li *ngFor="let l of universityList" class="slide-up-fade-in py-10">{{l.university}}</li>
  </ul>

  <div *ngIf="universityList && universityList.length===0" class="mt-50 slide-up-fade-in"
       data-test-id="no-result">No Results Found</div>
</div>


## universities-by-rank.component.ts


import {Component, OnInit} from '@angular/core';
import {HttpClient} from "@angular/common/http";

@Component({
  selector: 'app-universities-by-rank',
  templateUrl: './universities-by-rank.component.html',
  styleUrls: ['./universities-by-rank.component.scss'],
  standalone: false
})

export class UniversitiesByRankComponent implements OnInit {
  constructor(private http: HttpClient) {
  }

  universityList: University[];

  rank: number;

  onSearch(){
    this.http.get<ApiResponse>(`https://jsonmock.hackerrank.com/api/universities?rank_display=${this.rank}`).subscribe(response =>{
      
        this.universityList = response.data
      
    })
  }

  ngOnInit() {
  }
}

export interface University {
  university: string;
  rank_display: string;
  score: number;
  type: string;
  student_faculty_ratio: number;
  international_students: string;
  faculty_count: string;
  location: {
    city: string;
    country: string;
    region: string
  }
}

interface ApiResponse {
  page: number;
  per_page: number;
  total: number;
  total_pages: number;
  data: University[];
}
