-------Search and sort Books----


//Books.html

<div class="layout-row wrap w-100 justify-content-center" data-testid="book-list">
    <div *ngFor="let book of booksList" class="card layout-column w-20 ma-10 px-8">
        <h5 class="my-2">Name: {{book.book_name}}</h5>
        <h5 class="my-2">Author: {{book.author}}</h5>
        <h5 class="my-2">Genre: {{book.genre}}</h5>
        <h5 class="my-2">
            Rating: {{book.rating}} <span>⭐️</span>
        </h5>
    </div>

    <div [ngStyle]="{'color': 'red'}" data-testid="no-results" *ngIf="booksList.length===0">
        No Results Found!
    </div>
</div>


///books.ts

import { Component, Input } from '@angular/core';

export interface IBook {
  id: number,
  author: string,
  book_name: string,
  genre: string,
  rating: number,
}

@Component({
  selector: 'app-books',
  templateUrl: './books.component.html',
  styleUrls: ['./books.component.scss'],
  standalone: false
})
export class BooksComponent {
  @Input() booksList: IBook[] = [];
  
  
}



//search-sort.html

<div class="w-100 layout-row justify-content-center align-items-end pa-20">
    <input type="text" class="large mt-10 w-50" placeholder="Search for a book genre"
         data-testid="search" [(ngModel)]="bookGenre" (input)="onFilter()"/>
    <button class="my-0 h-4 mr-0" data-testid="sort-asc" #sortButton (click)="onAscending()">
        Sort A to Z
    </button>
    <button class="my-0 h-4" data-testid="sort-desc" #sortButton (click)="onDescending()">
        Sort Z to A
    </button>
</div>
<app-books [booksList]="filteredBooks"></app-books>


//search-sort.ts

import { Component, Input, OnInit } from '@angular/core';
import { IBook } from '../books/books.component';
import { startWith } from 'rxjs';

@Component({
  selector: 'app-search-sort',
  templateUrl: './search-sort.component.html',
  styleUrls: ['./search-sort.component.scss'],
  standalone: false
})
export class SearchSortComponent implements OnInit {
  @Input() booksList: IBook[] = [];
  bookGenre:string='';
  filteredBooks:IBook[]=[];
  ngOnInit(): void {
    this.filteredBooks=[...this.booksList];
  }

  onFilter(){
    const book=this.bookGenre.toLowerCase().trim();
    if(!book){
      this.filteredBooks=[...this.booksList];
      return;
    }
    if(book.length%2===0){
      this.filteredBooks=this.booksList.filter(b=>b.genre.toLowerCase().includes(book));
    }
  }

  onAscending(){
    this.filteredBooks.sort((a,b)=>
        a.book_name.localeCompare(b.book_name)
    );
  }
  onDescending(){
    this.filteredBooks.sort((a,b)=>
      b.book_name.localeCompare(a.book_name)
    );
  }
}



