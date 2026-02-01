

<div class="pt-75">
  <div class="layout-row align-items-center justify-content-center">
    <input type="date" class="large outlined" data-test-id="app-input"
           placeholder="Filter Date" [(ngModel)]="filterDates"/>
    <button (click)="onFilter()" class="" data-test-id="submit-button">Filter</button>
  </div>
  <div class="card mx-auto table-card mt-50 pb-10">
    <table>
      <thead>
      <tr>
        <th>Date</th>
        <th>Description</th>
        <th>Type</th>
        <th (click)="onOrder()" data-test-id="amount" class="sortable">Amount ($)</th>
        <th >Available Balance</th>
      </tr>
      </thead>
 
    <tbody data-test-id="records-body">
   <tr *ngFor="let txn of filteredTxns" >
    <td>{{txn.date}}</td>
      <td>{{txn.description}}</td>
      <td>{{txn.type === 1 ? "Debit" : "Credit"}}</td>
        <td>{{txn.amount}}</td>
     <td>{{txn.balance}}</td>
  </tr>
      </tbody>
    </table>
  </div>
</div>
 
 



import {Component, OnInit} from '@angular/core';
 
@Component({
  selector: 'app-record-table',
  templateUrl: './record-table.component.html',
  styleUrls: ['./record-table.component.scss'],
  standalone: false
})
export class RecordTableComponent implements OnInit {
  filterDate: string;
  txns: Transaction[];
  filteredTxns: Transaction[];
  filterDates:string='';
 
  ngOnInit() {
    this.txns = getTransactions();
    this.filteredTxns = this.txns;
  }
  onFilter(){
    if(this.filterDates.trim()===''){
      this.filteredTxns = this.txns;
      return;
    }
    this.filteredTxns = this.txns.filter(x=>x.date === this.filterDates);
  }
  onOrder(){
    this.filteredTxns = this.filteredTxns.sort((a,b)=>a.amount - b.amount);
  }
}
 
export interface Transaction {
  date: string;
  description: string;
  type: number;
  amount: number;
  balance: string;
}
 
export const getTransactions: () => Transaction[] = () => [
 
  {
    date: '2019-12-01',
    description: 'THE HACKERUNIVERSITY DES: CCD+ ID:0000232343',
    type: 0,
    amount: 1000,
    balance: '$12,234.45'
  },
  {
    date: '2019-11-25',
    description: 'HACKERBANK DES:DEBIT O ID: 0000987945787897987987',
    type: 1,
    amount: 2450.45,
    balance: '$12,234.45'
  },
  {
    date: '2019-11-29',
    description: 'HACKERBANK DES: CREDIT O ID:1223232323',
    type: 1,
    amount: 999,
    balance: '$10,928'
  },
  {
    date: '2019-12-03',
    description: 'HACKERBANK INC. DES:CCD+ ID: 33375894749',
    type: 0,
    amount: 1985.4,
    balance: '$12,234.45'
  },
  {
    date: '2019-11-29',
    description: 'HACKERBANK1 BP DES: MERCH PMT ID:1358570',
    type: 0,
    amount: 1520.34,
    balance: '$12,234.45'
  },
  {
    date: '2019-11-29',
    description: 'HACKERBANK DES: DEBIT O ID:00097494729',
    type: 0,
    amount: 564,
    balance: '$12,234.45'
  },
  {
    date: '2019-11-30',
    description: 'CREDIT CARD PAYMENT ID: 222349083',
    type: 1,
    amount: 1987,
    balance: '$12,234.45'
  }
];
 
 