# Keira3 Internals

This is a collection of notes aiming to explain the internals of Keira3 for development purposes.

If you just want to use Keira3, you don't need any of the following. If you want to modify Keira3, for example to improve it or to add new features, you may find this page useful.

## Main technologies

Keira3 is built upon the following open source web technologies:

- [**TypeScript**](http://www.typescriptlang.org/) is the main language of Keira3. It is a superset of JavaScript. 
If you know JavaScript and have some basic knowledge of OOP languages like Java and C#, you will feel pretty familiar with TypeScript already.
Otherwise you might find [this course](https://www.udemy.com/course/understanding-typescript/) helpful.
If you don't know JavaScript at all, it would be better to get some basic knowledge first.

- [**Angular**](https://angular.io/). This is the main framework behind Keira3. 
We absolutely recommend to get familiar with it before getting your hands inside Keira3's code.
If you are looking for a complete Angular course, we can recommend [this one](https://www.udemy.com/course/the-complete-guide-to-angular-2/).

- We use [**SCSS**](https://sass-lang.com/) to style our UI. It's an extension of CSS.
Knowing the CSS fundamentals is required in order to be able to change the Keira3's interface.
SCSS should be quite intuitive for anyone who can understand CSS.

- [**Bootstrap**](https://getbootstrap.com/) is the CSS framework used as a base for Keira3's style.
You don't have to be a Bootstrap expert, however we recommend to be at least familiar with its [Grid system](https://getbootstrap.com/docs/4.5/layout/grid/) and Utilities like [spacing](https://getbootstrap.com/docs/4.5/utilities/spacing/).

- [**Electron**](https://electronjs.org/) is the software framework that allows building Desktop apps using web technologies.
We don't use many native Electron features so usually you don't have to worry about it when developing Keira3.

## Testing

We use [test automation](https://en.wikipedia.org/wiki/Test_automation) in Keira3 in our development cycle. For every PR/commit, our CI automatically runs a lot of automated tests.

More specifically, we have:

- **Unit tests**. It's all `*.spec.ts` file, they run with `ng test`. We keep **100% coverage**.
  This means that if you try to submit untested code, the CI build of your PR will fail. We use the [Angular testing framework](https://angular.io/guide/testing) for it.

- **Integration tests**. It's all the `*.integration.spec.ts` file, they also run with `ng test`, together with the unit tests.
 You can see the integration tests of Keira3 almost like a set of e2e tests, the main difference is that all the DB interactions are mocked.
 The difference between unit tests and integration test is: in unit tests we test units by mocking all their dependencies, while in integration tests we test "big pieces" of Keira3 together (mocking only the DB). Mostly used to test the editors.
 
 - **E2E tests**. We have a tiny set of e2e tests based on [Spectron](https://www.electronjs.org/spectron). For example, to check the sqlite integration. 
 The command `ng e2e` will automatically serve the app and run the e2e tests.
 
 ### Why test automation?
 
 Because every time you modify your app, you never know if you are breaking any existing functionality unless you manually test everything again and again.
 
 When you have automated tests in place, you are still not 100% sure about not breaking existing stuff, but at least they can give you some assurance.

## Files structure

Relevant directories:

- **e2e**: all e2e test cases, isolated from everything else;

- **src/assets**: images and **global** style files (the components' style files are located in `src/app`);

- **src/app**: source code of the application as well as the unit and integration tests;

- **src/app/config**: app configuration files, like routing and library-specific configurations;

- **src/app/features**: source code of the main Keira3 features (each one isolated from the others). **RULE**: a feature can NOT import anything from another feature.
If something is meant to be shared across features, then it must be placed under `src/app/shared`;

- **src/app/main**: components that don't belong to a specific feature, yet they are isolated. For example, the root component `AppComponent`, the Sidebar, the Login window, etc...

- **src/app/shared**: all kinds of utilities, modules, components, services, abstract classes, testing utilities, etc... that **are meant to be used by more than 1 feature**;

## Architecture design

Keira3 code is structured using [OOP](https://en.wikipedia.org/wiki/Object-oriented_programming) with techniques like [inheritance](https://www.typescriptlang.org/docs/handbook/classes.html#inheritance) and [generic types](https://www.typescriptlang.org/docs/handbook/generics.html) to maximise code reuse.

Inside the directory `src/app/shared/abstract` there is a collection of abstract classes that are meant to be extended by the concrete Angular [Components](https://angular.io/guide/architecture-components) and [Services](https://angular.io/guide/architecture-services) which will implement the Keira3 features.

*If you are not familiar with the terminology used so far, please check the above hyperlinks before proceeding.*

TODO