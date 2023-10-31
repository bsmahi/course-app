# CourseApp

This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 16.2.8.

# Install  NodeJs and Angular 16/latest using CLI

## Install Node.js

1. **Download Node.js:** Visit the official Node.js website (https://nodejs.org/), and download the LTS (Long-Term Support) version, which is recommended for most users.
   Ensure you download the version compatible with your operating system (Windows, macOS, or Linux).
2. **Install Node.js:** Run the downloaded installer and follow the installation prompts. On Windows, you can accept the default settings in most cases.
   On macOS or Linux, you may need to use a package manager like Homebrew or apt to install Node.js.
3. **Verify Installation:** Open your terminal or command prompt and run the following commands to verify that Node.js and npm (Node Package Manager) were installed successfully:

```shell
node -v
npm -v
```
These commands should display the installed Node.js and npm versions.

OR 
```shell
npm install -g n 
n stable 
n latest 
n lts 
node -v
```
## Install Angular CLI
1. **Open a Terminal/Command Prompt:** Make sure you have a terminal or command prompt open.
2. **Install Angular CLI:** Run the following command to install the Angular Command Line Interface (CLI) globally on your system:
- npm install -g @angular/cli@latest
- npm install --save-dev @angular/cli@latest
3. **Verify Angular CLI Installation:** After the installation is complete, you can verify it by running the following command:

- ng --version

This command should display the installed Angular CLI version, as well as information about Angular.

# Angular 16 Course CRUD Application

- The `App` component is a container with `router-outlet`. It has navbar that links to routes paths via routerLink.
- `CoursesListComponent` - gets and displays courses
- `CoursesDetailsComponent` - Component for editing the course details
- `AddCourse` - Component for creating new Course
- These Components call `CourseService` methods which use Angular `HTTPClient` to make HTTP requests and receive responses.

# Tech Stack Used

- Angular 16
- Angular HttpClient
- Angular Router
- Bootstrap 4
- Node Js 18v

# Project Layout

![Image](/src/assets/courseappps.png "Project Structure")

- The main class model, `Course`, is exported by the `course.model.ts` file.
- There are three components in total, namely `courses-list, course-details, and add-course` 
- The `course.service` file contains methods that facilitate the sending of HTTP requests to the backend Apis
- The `app-routing.module.ts` file is responsible for defining routes for each component
- The `app` component consists of a router view and a navigation bar
- The `app.module.ts` file declares Angular components and imports the necessary modules

# Setup Angular Course App

Let’s open cmd and use Angular CLI to create a new Angular Project by using executing the following command:

```shell
ng new course-app
? Would you like to add Angular routing? Yes
? Which stylesheet format would you like to use? CSS
```

We also need to generate some Components and Services:

```shell
ng g s services/course

ng g c components/add-course
ng g c components/course-details
ng g c components/courses-list

ng g class models/course --type=model
```

# Set up App Module

Open `app.module.ts` and import `FormsModule, HttpClientModule`

```java
...
import { FormsModule } from '@angular/forms';
import { HttpClientModule } from '@angular/common/http';

@NgModule({
  declarations: [ ... ],
  imports: [
    ...
    FormsModule,
    HttpClientModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```
# Define Routes for Angular AppRoutingModule

There are 3 main routes:
- /courses for courses-list component
- /courses/:id for course-details component
- /add for add-course component

**_app-routing.module.ts_**

```typescript
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { CoursesListComponent } from './components/courses-list/courses-list.component';
import { CourseDetailsComponent } from './components/course-details/course-details.component';
import { AddCourseComponent } from './components/add-course/add-course.component';

const routes: Routes = [
  { path: '', redirectTo: 'courses', pathMatch: 'full' },
  { path: 'courses', component: TutorialsListComponent },
  { path: 'courses/:id', component: TutorialDetailsComponent },
  { path: 'add', component: AddTutorialComponent }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```
# Define Model Class

Our main model class `Course` will be exported in `course.model.ts` with 4 fields:

- id
- title
- description
- published

**_models/course.model.ts_**

```typescript
export class Course {
  id?: any;
  title?: string;
  description?: string;
  published?: boolean;
}
```
# Data Service Creation

This service utilizes Angular `HttpClient` for the purpose of sending HTTP requests.

Its functionalities encompass CRUD operations as well as a finder method.

**_services/course.service.ts_**

```typescript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';
import { Course } from '../models/course.model';

const baseUrl = 'http://localhost:8080/api/courses';

@Injectable({
  providedIn: 'root'
})
export class CourseService {

  constructor(private http: HttpClient) { }

  getAllCourses(): Observable<Course[]> {
    return this.http.get<Course[]>(baseUrl);
  }

  get(id: any): Observable<Course> {
    return this.http.get<Course>(`${baseUrl}/${id}`);
  }

  create(data: any): Observable<any> {
    return this.http.post(baseUrl, data);
  }

  update(id: any, data: any): Observable<any> {
    return this.http.put(`${baseUrl}/${id}`, data);
  }

  delete(id: any): Observable<any> {
    return this.http.delete(`${baseUrl}/${id}`);
  }

  deleteAll(): Observable<any> {
    return this.http.delete(baseUrl);
  }

  findByTitle(title: any): Observable<Course[]> {
    return this.http.get<Course[]>(`${baseUrl}?title=${title}`);
  }

}
```

# Create Angular 16 Components

As previously mentioned, the AppRoutingModule consists of three components that correspond to three defined routes.

- Add new Item Component
- List of items Component
- Item details Component

# Run the Course Application

**NOTE: PLEASE RUN BACKEND COURSE APP BEFORE RUNNING FRONTEND APPLICATION**

To launch this application, execute the command 
`ng serve --port 8081`.
Upon successful execution, access the application by opening a web browser and navigating to the URL `http://localhost:8081/`.

# Dockerize the Application

## Create an image using the below command
> docker build -t course-app .

## Run the container using the below command
> docker run -d -p 8081:80 course-app

## Development server

Run `ng serve` for a dev server. Navigate to `http://localhost:8081/`. The application will automatically reload if you change any of the source files.

## Code scaffolding

Run `ng generate component component-name` to generate a new component. You can also use `ng generate directive|pipe|service|class|guard|interface|enum|module`.

## Build

Run `ng build` to build the project. The build artifacts will be stored in the `dist/` directory.

## Running unit tests

Run `ng test` to execute the unit tests via [Karma](https://karma-runner.github.io).

## Running end-to-end tests

Run `ng e2e` to execute the end-to-end tests via a platform of your choice. To use this command, you need to first add a package that implements end-to-end testing capabilities.

## Further help

To get more help on the Angular CLI use `ng help` or go check out the [Angular CLI Overview and Command Reference](https://angular.io/cli) page.
