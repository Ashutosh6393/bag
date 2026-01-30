## timer.component.html


<div class="mt-100 layout-column align-items-center justify-content-center">
  <h2 data-test-id="timer-value">{{timer}}</h2>
  <button class="large" data-test-id="stop-button" (click)="onStop()">Stop Timer</button>
</div>


## timer.component.ts

import {Component, Input, OnInit} from '@angular/core';

@Component({
  selector: 'app-timer',
  templateUrl: './timer.component.html',
  styleUrls: ['./timer.component.scss']
})
export class TimerComponent implements OnInit {
  @Input() initial: number;
  timer: number;
  interval;

  onStop(){
    clearInterval(this.interval);
  }

  ngOnInit() {
    this.timer = this.initial;

    this.interval = setInterval(() => {
      if(this.timer > 0){
        this.timer--;
      }


      
    }, 1000);
    

  }

}
