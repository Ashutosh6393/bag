--------FootBall Match component---------

 
<div class="layout-row">
  <ul class="sidebar" data-test-id="year-list">
    <span class="section-title">Select Year</span>
    <li *ngFor="let year of years;" class="sidebar-item"
        [ngClass]="{'active': selectedYear === year}" (click)="getData(year)
        " >
      <a>{{year}}</a>
    </li>
  </ul>
 
  <section class="content">
    <section>
      <h3 data-test-id="total-matches" *ngIf="matchList.length>0 && hasSearched">Total Matches: {{matchList.length}}</h3>
      <ul class="mr-20 matches styled slide-up-fade-in" data-test-id="match-list" *ngIf="matchList.length>0 && hasSearched">
        <li class="slide-up-fade-in" *ngFor="let m of matchList">Match {{m.name}} won by {{m.winner}}</li>
 
      </ul>
    </section>
    <p data-test-id="no-result" class="slide-up-fade-in" *ngIf="matchList.length===0 && hasSearched">No Matches Found</p>
  </section>
</div>
 
 
 
import {Component, OnInit} from '@angular/core';
import {HttpClient} from "@angular/common/http";
import {tap} from "rxjs/operators";
import { Observable } from 'rxjs';
@Component({
  selector: 'app-football-matches',
  templateUrl: './football-matches.component.html',
  styleUrls: ['./football-matches.component.scss']
})
export class FootballMatchesComponent implements OnInit {
  years: number[] = [2011, 2012, 2013, 2014, 2015, 2016, 2017];
  selectedYear: number;
  hasSearched=false;
  constructor(private http: HttpClient) {
  }
 
  matchList:Competition[]=[];
  ngOnInit(): void {
 
  }
 
  getData(year:number){
    this.hasSearched=true;
    this.http.get<ApiResponse>(`https://jsonmock.hackerrank.com/api/football_competitions?year=${year}`).subscribe(
      response=>this.matchList=response.data
    )
  }
 
}
export interface Match {
  name: string;
  winner: string;
}
 
interface Competition {
  name: string;
  country: string;
  year: number;
  winner: string;
  runnerup: string;
}
interface ApiResponse {
  page: number;
  per_page: number;
  total: number;
  total_pages: number;
  data: Competition[];
}
 