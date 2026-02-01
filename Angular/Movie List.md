------Movie List------

 
import { Component, OnInit } from '@angular/core';
import { HttpClient } from "@angular/common/http";
 
@Component({
  selector: 'app-movie-list',
  templateUrl: './movie-list.component.html',
  styleUrls: ['./movie-list.component.scss'],
  standalone: false
})
 
export class MovieListComponent implements OnInit {
 
  yearInput: number = null;
 
  movieList: Movie[];
 
  constructor(private http: HttpClient) {
  }
 
  url: string = "https://jsonmock.hackerrank.com/api/moviesdata?"
 
  onSearch(){
   
 
    this.http.get<ApiResponse>(this.url+`Year=${this.yearInput}`).subscribe(
      response => this.movieList = response.data
    )
  }
 
  ngOnInit() {
  }
}
export interface Movie {
  Title: string;
  Year: number;
  imdbID: string;
}
 
interface ApiResponse {
  page: number;
  per_page: number;
  total: number;
  total_pages: number;
  data: Movie[];
}
 
 
<div class="layout-column align-items-center mt-50">
  <section class="layout-row align-items-center justify-content-center">
    <input type="number" class="large" [(ngModel)]="yearInput" placeholder="Enter Year eg 2015"
           data-test-id="app-input"/>
    <button class=""
    (click)="onSearch()"
    data-test-id="submit-button">Search</button>
  </section>
 
 
  <ul *ngIf="movieList && movieList.length>0" class="mt-50 styled" data-test-id="movieList">
    <li *ngFor="let movie of movieList" class="slide-up-fade-in py-10">
      {{movie.Title}}
    </li>
  </ul>
 
  <div *ngIf="movieList && movieList.length===0" class="mt-50 slide-up-fade-in"
       data-test-id="no-result">No Results Found</div>
</div>
 
 