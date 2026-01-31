import { Component } from '@angular/core';
import { HttpClient } from '@angular/common/http';
@Component({
  selector: 'app-books-by-genre',
  templateUrl: './books-by-genre.component.html',
  styleUrls: ['./books-by-genre.component.css'],
  standalone: false,
})
export class BooksByGenreComponent {
  matchedBooks: Book[] = [];
  genre: string = '';
  searched: boolean = false;
  private readonly BASE_URL = 'https://jsonmock.hackerrank.com/api/books';

  constructor(private http: HttpClient) {}

  onSubmit(value: string) {
    const query = (value || '').trim();

    // If query is empty, the test suite expects no request to be made
    if (!query) {
      return;
    }

    // Reset results for a clean state
    this.matchedBooks = [];

    // Construct the exact URL string as expected by the test spec
    const url = `${this.BASE_URL}?genre=${query}`;

    this.http.get<ApiResponse>(url).subscribe({
      next: (response) => {
        this.matchedBooks = response?.data || [];
        this.searched = true; // Set to true only after response
      },
      error: () => {
        this.matchedBooks = [];
        this.searched = true;
      },
    });
  }
}

export interface Book {
  author: string;
  book_name: string;
  genre: string;
  isbn13: string;
  no_of_pages: number;
  rating: number;
  votes: number;
}

interface ApiResponse {
  page: number;
  per_page: number;
  total: number;
  total_pages: number;
  data: Book[];
}



--------------------html-----------------------------------
<div class="layout-column align-items-center mt-50">
  <section class="layout-row align-items-center justify-content-center">
    <form (submit)="$event.preventDefault()">
      <input
        type="text"
        name="genre"
        [(ngModel)]="genre"
        #genreInput
        class="large"
        placeholder="Enter Genre eg Fiction"
        data-test-id="app-input"
      />
      <button type="button" (click)="onSubmit(genreInput.value)" data-test-id="submit-button">Search</button>
    </form>
  </section>

  <ul
    *ngIf="searched && matchedBooks.length > 0"
    class="mt-50 styled"
    data-test-id="booksList"
  >
    <li *ngFor="let b of matchedBooks" class="slide-up-fade-in py-10">
      {{ b.book_name }}
    </li>
  </ul>

  <div
    *ngIf="searched && matchedBooks.length === 0"
    class="mt-50 slide-up-fade-in"
    data-test-id="no-result"
  >
    No Results Found
  </div>
</div>
