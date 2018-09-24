# react-testing-library-course

_Course material for testing React components using react-testing-library_

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [My Comments](#my-comments)
- [Pre-requisites:](#pre-requisites)
- [System Requirements:](#system-requirements)
- [Setup:](#setup)
- [License](#license)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

<small>I'm still working on the README... sorry...</small>

We're going to be learning how to write tests with React and
[react-testing-library](https://github.com/kentcdodds/react-testing-library).
It's going to be fantastic.

## My Comments

Following along the youtube [video](https://www.youtube.com/watch?v=w6KCDFssHFA). I'll comment in the order of the exercises as in the linked video. The main premise is that UI tests should be written the same way a user would use the application: by scanning for text content.

- `react-dom`: render a component to DOM in memory and then use DOM API to find and query the elements for required values. Things I learned: Element is a specialized Node, which is actually a global interface for all possible elements in a DOM tree

- `jest-dom`: makes DOM testing simpler, adds helper methods used to search for certain attribute value or text content of an element

- `dom-testing-library`: more utils for DOM testing. Point of this is to focus the testing on the features instead of implementation details. That makes the tests more resilient to implementation changes, while still being useful to determine if the component is behaving correctly. `getQueriesForElement` creates function references for the given element, so I've used a helper called `getByLabelText` which finds an `input` element connected to a label with the given text. Good outtake: maybe I18N library can help you selecting a text value by i18n key instead.

- `react-testing-library`: looks much faster since we're running Virtual DOM instead of real DOM. Provides similar utilities as `dom-testing-library`, but specific for React. After each test we should do a cleanup, to make sure that the rendered component does not stay in DOM (test isolation). We can do it by manually calling `unmount` (returned from `render` call) at the end of each test (stupid), or just calling the function `cleanup` (imported from `react-testing-library`) in the `afterEach` hook. That is nice and explicit, but Kent then shows some magic by doing that kind of stuff automagically which is not cool since you have more to read & learn & keep in head while working with tests. Adding some helpers in each file as needed is not too troublesome.

- `state`: Kent prefers using `fireEvent` instead of `Simulate`. Point is to use real DOM events instead of synthetic ones. Helps to get through mumbo jumbo surrounding the way React handles the events. Using `debug` function returned from `render` call can help by printing out the current state of the whole container or just the element passed as argument. Label with the error message has to be searched for only after using the `change` event, since it does not exist before and `getByTestId` call will fail.

- `prop-updates`: used if there is a need to compare between the render before and after a prop changes. `rerender` is returned from the first render.

- `portals`: nothing special, seems easier than with Enzyme (Kent is gloating)

- `unmounting`: learned about timer mocks in JS, which give you deterministic control over time in tests, instead of relying on real time. Also learned about spies, which watch over object method calls

- `mock-component`: example uses an external component which in turn renders the provided children. Problem is that the used component waits a second before removing the element from DOM, so we'll mock it to make tests run fast and not depend on implementation details. Used `jest.mock`, takes import name + implementation. You just return the mock object, with the implementation you provided yourself.

- `http-jest-mocks`: a component makes an API call by using an imported module. To test it, we need to mock that module. This actually shows how to mock the user implemented modules, instead of the ones from `node_modules`. One thing I failed to understand at first: there is no need to keep variables to track spies, we can just import the same module we're mocking, you'll always get the mocked one.

- `dependency-injection`: what to do outside of Jest context? We use DI to inject dependencies instead of mocking them. This is really trivial and dissapointing, since I've expected more than just passing the `loadGreeting` through props.

- `a11ly`: there is a module called `jest-axe` which can help us in testing apps for a11ly problems. You cannot automate it completely, but it's better than nothing - someone should test this with an actual device.

## Pre-requisites:

You should be familiar with modern JavaScript and writing React applications.
Prior testing experience is not necessary.

## System Requirements:

You'll need git and node@>=8 installed.

## Setup:

Run these commands in your terminal to get yourself setup:

```
git clone https://github.com/kentcdodds/react-testing-library-course.git --branch workshop-2018-09
cd react-testing-library-course/
npm run setup
```

Please answer the prompt about your email. That should get you all setup.
If you have issues I can help you work through them while everyone else is
working on exercises.

## License

This material is available for private, non-commercial use under the
[GPL version 3](http://www.gnu.org/licenses/gpl-3.0-standalone.html). If you
would like to use this material to conduct your own workshop, please contact me
at kent@doddsfamily.us
