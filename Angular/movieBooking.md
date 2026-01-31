
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-ticket-booking',
  templateUrl: './ticket-booking.component.html',
  styleUrls: ['./ticket-booking.component.scss'],
  standalone: false
})
export class TicketBookingComponent implements OnInit {
  rows: number[] = Array(10).fill(0).map((_, i) => i);
  cols: number[] = Array(10).fill(0).map((_, i) => i);
  boookedSeats = [];
  selectedSeats = [];
  seatSelected:boolean=false
  seatRowsTimeAuto: string = "";
  selectedSeatSet: Set<string> = new Set<string>();
  constructor() { }

  ngOnInit(): void {
    for (let i = 0; i <= this.cols.length; i++) {
      this.seatRowsTimeAuto += 'auto ';
    }
  }

  getSeatText(row: number, col: number): string {
    return `${String.fromCharCode(65 + col)}${row}`;
  }

  
  isSelected(row: number, col: number): boolean {
    return this.selectedSeatSet.has(this.getSeatText(row, col));
  }

  isBooked(row: number, col: number): boolean {
    return this.boookedSeats.includes(this.getSeatText(row, col));
  }

  onSelect(row: number, col: number){
    
    const seat = this.getSeatText(row, col);

    // Prevent selecting already booked seats
    if (this.isBooked(row, col)) {
      return;
    }

    if (this.selectedSeatSet.has(seat)) {
      this.selectedSeatSet.delete(seat);
    } else {
      this.selectedSeatSet.add(seat);
    }
  }

  onBooking(){      
    if (this.selectedSeatSet.size === 0) {
      alert('Please select at least one seat');
      return;
      }

    // Merge selections into booked seats, deduplicate
    const unique = new Set<string>([...this.boookedSeats, ...this.selectedSeatSet]);
    this.boookedSeats = Array.from(unique);

    // Clear current selection after booking (optional)
    this.selectedSeatSet.clear();

  }

  onReset(){
    this.selectedSeatSet.clear();
    this.boookedSeats=[];
  }

  onClear(){
    this.selectedSeatSet.clear();
  }

}


------------------------html------------------------------

<div class="mt-50 layout-column justify-content-center align-items-center">
    <div class="display-flex">
        <button (click)="onBooking()" data-testid="book-seats">Book Seats</button>
        <button (click)="onClear()" data-testid="clear-selection" class="danger">Clear</button>
    </div>
    <div class="test" [ngStyle]="{'display': 'grid', 'gridTemplateColumns': seatRowsTimeAuto }">
        <div *ngFor="let row of rows">
            <div [ngStyle]="{'marginRight': row === 4 ? '40px' : '0px' }">
                <div  (click)="onSelect(row,col)" *ngFor="let col of cols" 
                    [ngClass]="{
                        'seat': true,
                        'selected': isSelected(row, col),
                        'disabled': isBooked(row, col)
                    }"
                    [attr.data-testid]="'_'+ col + '' + row + ''">
                    {{ getSeatText(row, col) }}
                </div>
            </div>
        </div>
    </div>
    <br />
    <button (click)="onReset()" data-testid="reset">Reset Bookings</button>
</div>


-----------------------Csss------------------------

.layout-column {
  .seat {
    border: solid red 1px;
    text-align: center;
    padding: 10px 20px;
    margin: 5px;
    color: red;

    &:hover {
      background-color: green;
    }
  }

  .disabled {
    color: gray;
    border: solid grey 1px;
    background-color: lightgrey;

    &:hover {
      background-color: lightgrey;
      cursor: not-allowed;
    }
  }

  .selected {
    background-color: green;
    color: white;

    &:hover {
      background-color: lightgreen;
      color: red;
    }
  }
}
