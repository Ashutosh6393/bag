<h1>
  {{domain.name}}
</h1>
<div class="form">
  <div class="form-group">
    <label>ID:</label>
    {{id}}
  </div>
  <div class="form-group">
    <label>Name:</label>
    {{domain.name}}
  </div>
  <div class="form-group">
    <label>Expiration:</label>
    {{domain.expiration}}
  </div>
  <div class="form-group">
    <label>Auto Renew:</label>
    {{domain.auto_renew? "Yes": "No"}}
  </div>
  <div class="form-group">
    <label>Domain Privacy:</label>
    {{domain.domain_privacy? "Yes" : "No"}}
  </div>
  <div class="form-group">
    <label>Renewal Price:</label>
    ${{domain.renewal_price}}/yr
  </div>
  <div class="form-actions">
    <button (click)="onBack()"  type="button">Back to the list</button>
  </div>
</div>




import { Component, Inject, OnInit } from '@angular/core';
import { domainList, Domain } from 'src/shared/mock-api-response/domain-list';
import { Router, ActivatedRoute, UrlSegment } from '@angular/router';


@Component({
  selector: 'app-domain-details',
  templateUrl: './domain-details.component.html',
  styleUrls: ['./domain-details.component.scss']
})
export class DomainDetailsComponent implements OnInit{

  id: string | null = "" ;
  domain: any;
 

  constructor(private route: ActivatedRoute, private router: Router ){}

  ngOnInit(): void {
    this.id = this.route.snapshot.paramMap.get('id');
    console.log(this.id)

    const x = domainList.find( e=> e.id === this.id);
    console.log(x)
    this.domain = x;
  }

  onBack(){
    this.router.navigate(['/']);
  }














  
}





<h1>My Domains</h1>
<section role="main">
  <table>
    <thead>
      <tr>
        <th>Name</th>
        <th>Expiration</th>
        <th>Renewal Price</th>
        <th></th>
      </tr>
    </thead>
    <tbody>
      <tr *ngFor="let domain of dList">
        <td>
          {{domain.name}}
        </td>
        <td>
         {{domain.expiration}}
        </td>
        <td>
        ${{domain.renewal_price}}/yr
        </td>
        <td>
          <a [routerLink]= "['domain', domain.id]" >details</a>
        </td>
      </tr>
  
    </tbody>
  </table>
</section>




import { Component, OnInit } from '@angular/core';

import { domainList, Domain } from 'src/shared/mock-api-response/domain-list';

@Component({
  selector: 'app-domain-list',
  templateUrl: './domain-list.component.html'
})
export class DomainListComponent implements OnInit{

  dList: Domain[] = [];

  ngOnInit(): void {
    this.dList = domainList
  }
}



// app.component.html
<!-- <app-domain-list></app-domain-list>
<app-domain-details></app-domain-details> -->

<router-outlet></router-outlet>




// app.module.ts

import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import {
  RouterModule,
  Routes
} from '@angular/router';

import { AppComponent } from './app.component';
import { DomainDetailsComponent } from './domain-details/domain-details.component';
import { DomainListComponent } from './domain-list/domain-list.component';

export const routes: Routes = [{
  path: '', component: DomainListComponent, pathMatch: 'full'
}, 
{
  path: 'domain/:id', component: DomainDetailsComponent
},
{
  path: '**', redirectTo: ''
}];

@NgModule({
  declarations: [
    AppComponent,
    DomainListComponent,
    DomainDetailsComponent
  ],
  imports: [
    BrowserModule,
    RouterModule.forRoot(routes)
  ],
  bootstrap: [AppComponent]
})

export class AppModule {
}
