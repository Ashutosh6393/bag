-------Catalog viewer--------

thumbs.html

<div class="layout-row">
  <span *ngFor="let catalog of items; let idx=index"
  >
    <span class="inline-flex w-90 pa-4 image-thumb"
      [ngClass]="{'thumb-selected': idx === currentIndex}">
      <span
        class='mx-5 thumb'
        [attr.data-test-id]="'thumb-button-' + idx"
        [ngStyle]="{'background-image': 'url('+ catalog.thumb + ')'}" (click)="onSelect(idx)">
      </span>
    </span>
  </span>
</div>


thumbs.ts

import { Component, Input, OnInit, Output, EventEmitter, } from "@angular/core";

@Component({
  selector: "thumbs",
  templateUrl: "./thumbs.component.html",
  styleUrls: ["./thumbs.component.scss"],
  standalone: false
})
export class Thumbs implements OnInit {
  @Input() items;
  @Input() currentIndex;
  @Output() selectedCatalog: EventEmitter<number> = new EventEmitter<number>();
  ngOnInit() {
  }

  onSelect(idx:number){
    this.selectedCatalog.emit(idx);
  }
}



app.html

<h8k-navbar header="Catalog Viewer"></h8k-navbar>
<div class="layout-column justify-content-center mt-75">
  <div class="layout-row justify-content-center">
    <div class="card pt-25">
      <viewer [catalogImage]="catalogsList[activeIndex].image"></viewer>
      <div class="layout-row justify-content-center align-items-center mt-20">
        <button
          class="icon-only outlined"
          data-test-id="prev-slide-btn"
          (click)="onPrev()"
        >
          <i class="material-icons">arrow_back</i>
        </button>
        <thumbs
          [items]="catalogsList"
          [currentIndex]="activeIndex"
          (selectedCatalog)="selectedCatalog($event)"
        >
        </thumbs>
        <button
          class="icon-only outlined"
          data-test-id="next-slide-btn"
          (click)="onNext()"
        >
          <i class="material-icons">arrow_forward</i>
        </button>
      </div>
    </div>
  </div>
  <div class="layout-row justify-content-center mt-25">
    <input
      type="checkbox"
      data-test-id="toggle-slide-show-button"
      (change)="toggleSlide($event)"
    />
    <label class="ml-6">Start Slide Show</label>
  </div>
</div>


----app.ts

import {Component} from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss'],
  standalone: false
})

export class AppComponent {
  title = 'Catalog Viewer';

  catalogsList = [
    {
      thumb: '/assets/images/tea-light-candle.jpg',
      image: '/assets/images/tea-light-candle.jpg'
    },
    {
      thumb: '/assets/images/white-light-candle.jpg',
      image: '/assets/images/white-light-candle.jpg'
    },
    {
      thumb: '/assets/images/pink-light-candle.jpg',
      image: '/assets/images/pink-light-candle.jpg'
    },
    {
      thumb: '/assets/images/green-light-candle.jpg',
      image: '/assets/images/green-light-candle.jpg'
    }
  ]

  activeIndex: number = 0;
  slideDuration = 3000;
  private slideTimer:any=null;

  selectedCatalog(index: number) {
    this.activeIndex=index;

  }
  onPrev(){
    const n=this.catalogsList.length;
    this.activeIndex=(this.activeIndex-1+n)%n;
  }
  onNext(){
    const n=this.catalogsList.length;
    this.activeIndex=(this.activeIndex+1+n)%n;
  }
  toggleSlide(event:Event){
    const input=event.target as HTMLInputElement;
    if(input.checked){
      this.startSlideShow();
    }else{
      this.stopSlideShow();
    }
  }
  startSlideShow(){
    this.stopSlideShow();
    this.slideTimer=setInterval(()=>{
      this.onNext();
    },this.slideDuration);
  }
  stopSlideShow(){
    clearInterval(this.slideTimer);
    this.slideTimer=null;
  }
  
}
