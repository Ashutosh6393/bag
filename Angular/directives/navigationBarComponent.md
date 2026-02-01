## navigationBar.component.html

<div class="layout-column justify-content-center align-items-center">

  <div class="layout-row justify-content-around align-items-center mt-20 links">
      <a (click)="onClick(0)"  data-test-id="home-navigation-tab">Home</a>
      <a (click)="onClick(1)" data-test-id="news-navigation-tab">News</a>
      <a (click)="onClick(2)" data-test-id="contact-navigation-tab">Contact</a>
      <a (click)="onClick(3)" data-test-id="about-navigation-tab">About</a>
  </div>

  <div class="card w-20 ma-0">
    <section class="card-text">
        <span data-test-id="tab-content">{{test[selected]}}</span>
    </section>
  </div>
</div>


## navigationBar.component.ts


import {Component, OnInit} from '@angular/core';

@Component({
  selector: 'navigation-bar',
  templateUrl: './navigationBar.component.html',
  styleUrls: ['./navigationBar.component.scss']
})
export class NavigationBar implements OnInit {

  test: string[] = ["HOME PAGE", "NEWS PAGE", "CONTACT PAGE", "ABOUT PAGE"]

  selected: number = 0;

  onClick(i){
    this.selected = i;
  }
  ngOnInit() {
  }

}