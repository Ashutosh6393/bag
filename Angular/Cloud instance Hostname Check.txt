----------------Cloud instance Hostname check-------------

//html

<section role="alert" class="info">
  We are currently investigating the open issue <strong>Subsea Fiber Faults in the Pacific#2 region</strong>. <a
  href="#">Learn more</a>.
</section>
<h1>Create Instance</h1>
<section role="main">
  <article>
    <form class="form"
          [formGroup]="instanceCreationForm">
      <div class="form-group">
        <label class="required">Location:</label>
        <div class="form-group available">
          <label>
            <i class="material-icons">cloud</i> DC-1, default
            <input type="radio" name="location" checked/>
          </label>
          <ul>
            <li>Location: Pacific#1</li>
            <li>Status: <span class="status">available</span></li>
          </ul>
        </div>
        <div class="form-group not-available">
          <label>
            <i class="material-icons">cloud_off</i> DC-2
            <input type="radio" name="location" disabled/>
          </label>
          <ul>
            <li>Location: Pacific#2</li>
            <li>Status: <span class="status">not available</span></li>
          </ul>
        </div>
        <p>
          <i class="material-icons">notification_important</i> Important: "DS-2" data center is temporarily unavailable.
        </p>
      </div>
      <div class="form-group">
        <label for="configuration">Configuration:</label>
        <select id="configuration" required>
          <option>1 GB / 1 CPU / 25 GB SSD Disk</option>
          <option>2 GB / 1 CPU / 50 GB SSD Disk</option>
          <option>4 GB / 2 CPU / 80 GB SSD Disk</option>
        </select>
      </div>
      <div class="form-group">
        <label for="hostname">Hostname:</label>
        <input type="text"
               id="hostname"
               formControlName="hostname" [(ngModel)]="inputHost"/>
        <ul class="errors" *ngIf="(hostname.touched || hostname.dirty) && hostname.invalid">
          <li *ngIf="hostname.errors?.['required']">Cannot be empty</li>
          <li *ngIf="hostname.errors?.['pattern']">Should be a valid host name</li>
          <li *ngIf="hostname.errors?.['hostnameExistsValidator']">An instance with hostname "[[hostname]]" already exists</li>
        </ul>
      </div>
      <!-- password -->
       <div>
        <label for="password">Password:</label>
        <input type="text"
               id="password"
               formControlName="password" [(ngModel)]="inputPassword"/>
       </div>
        <!-- Confirm password -->
       <div>
        <label for="confirm-password">Confirm Password:</label>
        <input type="text"
               id="confirm-password"
               formControlName="confirmPassword" [(ngModel)]="inputConfirmPassword"/>
       </div>
      <div class="form-actions">
        <button type="submit"
                [disabled]="instanceCreationForm.invalid">Create
        </button>
      </div>
    </form>
  </article>
</section>


//ts

import { Component, signal } from '@angular/core';
import { RouterOutlet } from '@angular/router';
import { Instance, instanceList } from './instanceList';
import { FormControl, FormGroup, FormsModule, NgModel, ReactiveFormsModule, Validators } from '@angular/forms';
import { CommonModule } from '@angular/common';

@Component({
  selector: 'app-root',
  imports: [RouterOutlet,CommonModule,FormsModule,ReactiveFormsModule],
  templateUrl: './app.html',
  styleUrl: './app.css'
})
export class App {
  protected readonly title = signal('CryptoExchangeApp');
  hostArr:Instance[]=instanceList;
  inputHost:string='';
  inputPassword:string='';
  inputConfirmPassword:string='';
 
  instanceCreationForm: FormGroup = new FormGroup({
    hostname: new FormControl<string>('',[Validators.required,Validators.pattern(/^[A-Za-z0-9]+(\.[A-Za-z0-9]+)*$/)]),
    password:new FormControl('',Validators.required),
    confirmPassword:new FormControl('',Validators.required)
  });
 
  get hostname(){
    return this.instanceCreationForm.get('hostname') as FormControl;
  }

  get password(){
    return this.instanceCreationForm.get('password') as FormControl;
  }

  get confirmPassword(){
    return this.instanceCreationForm.get('confirmPassword') as FormControl;
  }
}

