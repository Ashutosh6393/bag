CryptoRank Exchange
 
 
###app.component.html
 
<h1>CryptoRank Exchange</h1>
<section role="search">
  <div class="form-group">
    <label>
      I want to exchange $
      <input type="number" step=".01" placeholder="USD" [(ngModel)]="amountExchange" #inputctrl="ngModel" min="0.01" max="{{availableAmout}}" required/>
      of my $<span>{{availableAmout}}</span>:
    </label>
    <ul *ngIf="inputctrl.invalid && (inputctrl.touched || inputctrl.dirty )" class="errors">
      <li *ngIf="inputctrl.errors?.['required']">Cannot be empty</li>
      <li *ngIf="inputctrl.errors?.['min']">Cannot be less than $0.01</li>
      <li *ngIf="inputctrl.errors?.['max']">Cannot exceed the available balance</li>
    </ul>
  </div>
</section>
<section role="main">
  <article>
    <table>
      <thead>
        <tr>
          <th>Cryptocurrency</th>
          <th>Exchange Rate</th>
          <th>Number of Coins</th>
        </tr>
      </thead>
      <tbody>
         <tr *ngFor="let crypto of filterList">
          <td>{{crypto.name}}</td>
          <td>1 USD = {{crypto.rate}} {{crypto.code}}</td>
          <td>{{inputctrl.valid && amountExchange ? (+amountExchange * crypto.rate).toFixed(8) : 'n/a'}}</td>
        </tr>
      </tbody>
    </table>
  </article>
</section>
 
 
 
###app.component.ts
 
import { Component, OnInit } from '@angular/core';
import { Cryptocurrency ,cryptocurrencyList } from 'src/shared/mock-api-response/cryptocurrency-list';
import { profileDetails } from 'src/shared/mock-api-response/profile-details';
 
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: [
    './app.component.scss'
  ],
  standalone: false
})
 
export class AppComponent implements OnInit{
filterList :Cryptocurrency[] = [];
amountExchange:number = profileDetails.availableFiatBalanceAmount;
availableAmout:number = profileDetails.availableFiatBalanceAmount;
 
ngOnInit(): void {
  this.filterList = cryptocurrencyList
}
}
 
 
 
###app.module.ts
 
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { FormsModule } from '@angular/forms';
 
import { AppComponent } from './app.component';
import { CommonModule } from '@angular/common';
 
@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    FormsModule,
    CommonModule
  ],
  bootstrap: [AppComponent]
})
 
export class AppModule {
}
 
 