import {Component, Input, OnInit} from '@angular/core';
 
@Component({
  selector: 'app-color-picker',
  templateUrl: './color-picker.component.html',
  styleUrls: ['./color-picker.component.scss'],
  standalone: false
})
export class ColorPickerComponent implements OnInit {
  @Input() colorOptions: string[];
  @Input() initialColor: string;
  selectedColor : string;
 
  ngOnInit(): void {
    this.selectedColor = this.initialColor;
  }
onColor(color){
  this.selectedColor = color;
}
 
}
 
 

 <div class="layout-row justify-content-center">
  <div class="card mt-75">
    <div class="canvas" data-test-id="selectedColor" [ngStyle]="{'background-color': selectedColor}">
      <p class="text-center mx-auto px-5">{{selectedColor}}</p>
    </div>
    <div class="card-actions">
      <div class="layout-row justify-content-center align-items-center" data-test-id="colorPickerOptions">
        <div class="color-box mx-8 my-15"
             *ngFor="let color of colorOptions"
             [ngStyle]="{'background-color' : color}"
             (click)="onColor(color)"
             [ngClass]="{'selected' : selectedColor === color}">{{color}}</div>
      </div>
    </div>
  </div>
</div>
 
 