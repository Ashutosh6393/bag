## blog.component.html

<div class="layout-row wrap" data-test-id="blog-posts">
 <!-- Used for blog item rendering -->
 <div *ngFor="let blogPost of blogPosts; let index" class="flex-25">
   <article  class="card blog-post-item" [attr.data-test-id]="'blog-item-' + index">
     <img [src]="blogPost.image" class="card-image" alt=""/>
     <div class="blog-post-item-overlay"></div>
     <div class="blog-post-item-content pb-20">
       <p class="blog-title px-16" data-test-id="blog-title">{{blogPost.title}}</p>
       <section class="layout-row justify-content-between align-items-center blog-meta px-16">
         <div class="layout-row justify-content-between align-items-center author-section">
           <img src="/assets/images/logo.svg" class="author-avatar" alt=""/>
           <section class="layout-column justify-content-between align-items-start ml-12">
             <p class="author-name text-capitalize font-weight-medium">{{blogPost.author}}</p>
             <p class="post-comments font-weight-light my-4">Comments : {{blogPost.num_comments}}</p>
           </section>
         </div>
         <span class="blog-date font-weight-light">{{blogPost.created_at | date : 'mediumDate'}}</span>
       </section>
     </div>
   </article>
 </div>

</div>


## blog.component.ts

import {Component, Input, OnInit} from '@angular/core';
import {BlogPost} from "../../types";

@Component({
  selector: 'app-blog',
  templateUrl: './blog.component.html',
  styleUrls: ['./blog.component.scss'],
  standalone: false
})
export class BlogComponent implements OnInit {
  @Input() blogPosts: BlogPost[];
  ngOnInit() {
  }
}

## theme-switcher.component.html

<div class="layout-row justify-content-end align-items-center my-16">
  <button class="small text" data-test-id="switcher-button" (click)="onThemeSwitch()">
    <i class="material-icons">{{getIcon()}}</i>
    <span class="ml-10" data-test-id="current-theme">{{btnTheme? 'Dark Theme': 'Light Theme'}}</span>
  </button>
</div>


## theme-switcher.component.ts

import {Component, EventEmitter, Input, OnInit, Output} from '@angular/core';

@Component({
  selector: 'app-theme-switcher',
  templateUrl: './theme-switcher.component.html',
  styleUrls: ['./theme-switcher.component.scss'],
  standalone: false
})
export class ThemeSwitcherComponent implements OnInit {
  isDark: boolean = false;
  @Output() onThemeChanged: EventEmitter<any> = new EventEmitter<any>();
  btnTheme=false;
  dark: boolean = false;

  ngOnInit() {
  }
  onClick(){
   
  }

  onThemeSwitch(){

     if(this.btnTheme){
      this.btnTheme=false;
      this.onThemeChanged.emit('light')
    }else{
      this.btnTheme=true;
      this.onThemeChanged.emit('dark');
    }

    // console.log('x')
    // this.onThemeChanged.emit(this.isDark);
    // this.dark = !this.dark;
    // this.isDark = !this.isDark;

  }


  getIcon() {
    return this.isDark ? 'brightness_high' : 'wb_sunny'
  }

}


## app.component.html
<!--The content below is only a placeholder and can be replaced.-->

<h8k-navbar header="Theme Switcher"></h8k-navbar>

<div class="layout-column mt-75 pb-50 mx-16 theme--dark">
  <app-theme-switcher (onThemeChanged)="applyTheme($event)"></app-theme-switcher>
  <app-blog [blogPosts]="blogPosts"></app-blog>
</div>




## app.component.ts

import {Component, OnInit, Renderer2} from '@angular/core';
import {BlogPost} from "../types";

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss'],
  standalone: false
})
export class AppComponent implements OnInit {
  blogPosts: BlogPost[];

  ngOnInit() {
    this.blogPosts = [...BLOG_POSTS].map((blogPost, index) => {
      blogPost.id = index + 1;
      blogPost.image = `/assets/images/blog-posts/img-${index + 1}.png`;
      blogPost.created_at = blogPost.created_at * 1000;
      return blogPost;
    });
  }
  applyTheme(value: any) {
    document.body.classList.remove('theme--dark','theme--light');
    document.body.classList.add(value==='light'? 'theme--light':'theme--dark');
  }

}


export const BLOG_POSTS: BlogPost[] = [
  {
    "title": "A Message to Our Customers",
    "url": "http://www.apple.com/customer-letter/",
    "author": "epaga",
    "num_comments": 967,
    "created_at": 1455698317
  },
  {
    "title": "“Was isolated from 1999 to 2006 with a 486. Built my own late 80s OS”",
    "url": "http://imgur.com/gallery/hRf2trV",
    "author": "epaga",
    "num_comments": 265,
    "created_at": 1418517626
  },
  {
    "title": "Apple’s declining software quality",
    "url": "http://sudophilosophical.com/2016/02/04/apples-declining-software-quality/",
    "author": "epaga",
    "num_comments": 705,
    "created_at": 1454596037
  },
  {
    "title": "F.C.C. Repeals Net Neutrality Rules",
    "url": null,
    "author": "patricktomas",
    "num_comments": 376,
    "created_at": 1317858143
  },
  {
    "title": "Google Is Eating Our Mail",
    "url": "https://www.tablix.org/~avian/blog/archives/2019/04/google_is_eating_our_mail/",
    "author": "saintamh",
    "num_comments": 685,
    "created_at": 1556274921
  },
  {
    "title": "Why I’m Suing the US Government",
    "url": "https://www.bunniestudios.com/blog/?p=4782",
    "author": "saintamh",
    "num_comments": 305,
    "created_at": 1469106658
  },
  {
    "title": "F.C.C. Repeals Net Neutrality Rules",
    "url": "https://www.nytimes.com/2017/12/14/technology/net-neutrality-repeal-vote.html",
    "author": "panny",
    "num_comments": 1397,
    "created_at": 1513275215
  },
  {
    "title": "Show HN: This up votes itself",
    "url": "http://news.ycombinator.com/vote?for=3742902&dir=up&whence=%6e%65%77%65%73%74",
    "author": "olalonde",
    "num_comments": 83,
    "created_at": 1332463239
  },
];
