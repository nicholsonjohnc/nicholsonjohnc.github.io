---
layout: default
title: Sign Up
description: "Custom Cognito sign up form using Angular and Bootstrap"
parent: Authentication
nav_order: 1
---

# Custom Cognito sign up form using Angular and Bootstrap
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Sign up template

```html
<div class="card">
  <div class="card-body">
    <form>
      <h1 class="card-title">Sign up</h1>
      <div *ngIf="errorMessage!=null" class="alert alert-danger">
        {{ errorMessage }}
      </div>
      <div class="form-group">
        <input [(ngModel)]="email" [ngModelOptions]="{standalone: true}" type="email" class="form-control" aria-label="Email" placeholder="Email">
      </div>
      <div class="form-group">
        <input [(ngModel)]="password" [ngModelOptions]="{standalone: true}" type="password" class="form-control" aria-label="Password (at least 8 characters)" placeholder="Password (at least 8 characters)">
      </div>
      <div class="form-group">
        <input [(ngModel)]="confirmPassword" [ngModelOptions]="{standalone: true}" type="password" class="form-control" aria-label="Confirm password" placeholder="Confirm password">
      </div>
      <button (click)="onSignup()" type="submit" class="btn btn-primary btn-block">Create account</button>
    </form>
    <hr>
    <p class="card-text text-muted">Already have an account? <a [routerLink]="[appPaths.signin]">Sign in</a>.</p>
  </div>
</div>
```

## Sign up styles

```css
.card {
    max-width: 400px;
    margin-left: auto;
    margin-right: auto; 
    margin-top: 75px;
}

.h1, h1 {
    margin-bottom: 30px;
}
```

## Sign up component

```js
import { Component, OnInit } from '@angular/core';
import { AppPaths } from '../app.paths';
import { AuthenticationService } from "../authentication/authentication.service";
import { Router } from "@angular/router";

@Component({
  selector: 'abc-signup',
  templateUrl: './signup.component.html',
  styleUrls: ['./signup.component.css']
})
export class SignupComponent implements OnInit {

  errorMessage: string = null;
  email: string = null;
  password: string = null;
  confirmPassword: string = null;

  constructor(public appPaths: AppPaths,
              public authenticationService: AuthenticationService,
              public router: Router) { }

  ngOnInit() {
  }

  onSignup(): void {

    this.errorMessage = null;
    this.authenticationService.registerUser(this.email, this.password, (error: any, result: any) => {
      if (error) {
        this.errorMessage = error;
      } else {
        this.router.navigate([this.appPaths.confirm, result.user.username]);
      }
    });

  }

}
```