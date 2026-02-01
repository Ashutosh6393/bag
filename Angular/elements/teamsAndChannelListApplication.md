## team.component.html

<div *ngIf="team">
  <h4 class='mt-0 mb-6'>{{team.name}}</h4>
  <div class='layout-row justify-content-end mb-6'>
    <input
      placeholder='Enter Channel Name'
      class="channel-name-input px-13"
      [attr.data-test-id]="'channel-name-input-' + teamIndex"
      [(ngModel)]="channelName"
    />
    <button
      class='channel-name-btn x-small w-35 h-30 pa-6 ma-0 ml-6'
      [attr.data-test-id]="'add-channel-btn-' + teamIndex"
      (click)="onAddChannel()"
      [disabled]="onDisabled()"
    >
      Add Channel
    </button>
  </div>
  <ul class='styled mb-20 pl-25' [attr.data-test-id]="'channel-list-' + teamIndex">
    <li *ngFor="let channel of team && team.channels; let i = index"
      class='flex slide-up-fade-in justify-content-between align-items-center pl-10 pr-5 py-6 mt-0 mb-6'
    >
      <span>{{channel.name}}</span>
      <button (click)="onDeleteChanel(i)" [attr.data-test-id]="'remove-channel-button-' + teamIndex + i" class='icon-only x-small danger ma-0 pa-0'>
          <i class="material-icons">delete</i>
      </button>
    </li>
  </ul>
</div>


## team.component.html

import {Component, OnInit, Input, Output, EventEmitter} from '@angular/core';
import { ITeam } from '../app.component';

@Component({
  selector: 'team',
  templateUrl: './team.component.html',
  styleUrls: ['./team.component.scss'],
  standalone: false
})
export class Team implements OnInit {
  @Input() team: ITeam;
  @Input() teamIndex: number;
  @Output() newChannel: EventEmitter<string> = new EventEmitter<string>();
  @Output() cancelChannel:  EventEmitter<number> = new EventEmitter<number>();
  constructor() {}


  channelName: string = "";

  onAddChannel(){
    this.newChannel.emit(this.channelName);
    this.channelName ="";
  }

  onDeleteChanel(index){
    this.cancelChannel.emit(index)
  }

  onDisabled(){
    return this.channelName === '' || this.team.channels.some(e => e.name === this.channelName)
  }

  ngOnInit() {

  }
}


## teamList.component.html

<div class='w-50 mx-auto'>
    <div class='card w-40 mt-50 mx-auto px-10 py-15'>

      <div class='layout-column' data-test-id='team-list'>
        <team *ngFor="let team of teams; let i = index" [team]="team" [teamIndex]="i" (cancelChannel)="removeChannel($event, i)"
          (newChannel)="addChannel($event, i)">
        </team>
      </div>

      <div class='layout-row'>
        <input
          placeholder='Enter Team Name'
          class='team-list-input w-75'
          data-test-id='team-name-input'
          [(ngModel)]="teamName"

        />
        <button
          class='team-list-btn x-small w-35 h-30 pa-6 ma-0 ml-6'
          data-test-id='add-team-btn'
          (click)="addTeam()"
          [disabled]="onInput()"
        >
          Add Team
        </button>
      </div>

    </div>
  </div>


## teamList.component.ts

import { Component, OnInit } from '@angular/core';
import { ITeam } from '../app.component';

@Component({
  selector: 'team-list',
  templateUrl: './teamList.component.html',
  styleUrls: ['./teamList.component.scss'],
  standalone: false
})
export class TeamList implements OnInit {

  teams: ITeam[] = [];
  component: ITeam;
  teamName: string = "";

  isTeamNameUnique: boolean = false;

  constructor() {
    this.teams.push({
      name: 'Team1',
      channels: [{
        name: 'Channel1',
        index: 1
      },
      {
        name: 'Channel2',
        index: 2
      }]
    });
    this.teams.push({
      name: 'Team2',
      channels: [{
        name: 'Channel1',
        index: 1
      },
      {
        name: 'Channel2',
        index: 2
      }]
    });
  }

  ngOnInit() {
  }

  onInput(){
    return this.teamName === '' || this.teams.some(e => (e.name == this.teamName))
  }


  addTeam(){
    this.teams.push({name: this.teamName, channels: []});
    this.teamName = "";
  }

  addChannel(channelName, teamIndex) {
    this.teams[teamIndex].channels.push({
      name: channelName,
      index: this.teams[teamIndex].channels.length + 1
    })

  }

  removeChannel(channelIndex, teamIndex) {
    this.teams[teamIndex].channels.splice(channelIndex,1);
  }
}


## app.component.ts

import {Component} from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss'],
  standalone: false
})

export class AppComponent {
  title = 'Teams and Channels List';
}

export interface ITeam {
  channels: IChannel[];
  name: String
}

export interface IChannel {
  name: string;
  index: number;
}



