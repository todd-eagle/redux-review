# React Redux Review

## Description

This project is intended to provide another example of an implementation of React Redux. The master branch contains a project ready to implement React Redux. For a working version of the project, checkout to the `finished` branch.

## Instructions

Many people struggle to implement the redux pattern their first few times. The best way to become comfortable with it will be to repeat it several times. This repo will provide instructions walking through the process. For your information we have installed the following libraries from npm:

- redux
- react-redux
- redux-devtools-extension
- react-router-dom (already implemented)

### Step 1

Our first step will be to set up our reducer.

1. Inside of your `src` folder, create a `ducks` folder. Inside of this folder create a file called `moviesReducer.js`.
2. Let's set up our reducer, it will contain 4 parts
   1. Our initial state which will need the following properties and default values: `title: ''`, `poster: ''`, `rating: null`, `movies: []`.
   2. An action constant called `"SET_MOVIE_INFO"`
   3. An action creator, a function called `setMovieInfo` that does the following:
      - Takes in title, poster, and rating as parameters
      - Returns an object containing two properties: type and payload
      - type will be our `SET_MOVIE_INFO` constant
      - payload will be an object containing our title, poster, and rating
      - Make sure to export this function
   4. Our reducer function which will take in state (this should default to initialState)and action. It will contain:
      - A switch statement that will switch on our action.type.
      - In that switch statement, set up a case for `SET_MOVIE_INFO` which will return an updated redux state with our title, poster, and rating properties updated (HINT: Use the spread operator to do this easily)
      - Don't forget to include a default case that will just return our state object
      - This function needs to be our default export from this file.

<details>
  <summary>moviesReducer.js solution</summary>

```js
const initialState = {
  title: '',
  poster: '',
  rating: null,
  movies: [],
}

const SET_MOVIE_INFO = 'SET_MOVIE_INFO'

export const setMovieInfo = (title, poster, rating) => {
  return {
    type: SET_MOVIE_INFO,
    payload: { title, poster, rating },
  }
}

function moviesReducer(state = initialState, action) {
  switch (action.type) {
    case SET_MOVIE_INFO:
      return { ...state, ...action.payload }
    default:
      return state
  }
}

export default moviesReducer
```

</details>

### Step 2

Our next step will be to set up our store and connect provide it to our application

1. Create a file inside your `src` folder called `store.js` this will hold our redux store. This will also make it possible for us to use the redux devtools. If you do not have them installed you can find them [here](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd?hl=en). They will make your life infinitely easier when dealing with redux because you will be able to dynamically check your redux state values.
2. Import `createStore` from `redux`, `moviesReducer` from our `moviesReducer.js` file, and `devToolsEnhancer` from `redux-devtools-extension`. Only our `moviesReducer` is the default export so don't forget to put curly braces around the other two imports.
3. Create a variable called `store` and set it equal to `createStore` invoked. Pass it our movies reducer as its first argument, its second argument should be `devToolsEnhancer` invoked. This will connect our app to the redux devtools.
4. Inside of your `index.js` file, import `Provider` from `react-redux` and `store` from our `store.js` file. Wrap your app component in our Provider and pass it `store` as a prop called store. This will make our store available to our app.

<details>
<summary>store.js solution</summary>

```js
import { createStore } from 'redux'
import moviesReducer from './ducks/moviesReducer'
import { devToolsEnhancer } from 'redux-devtools-extension'

export default createStore(moviesReducer, devToolsEnhancer())
```

</details>

<details>
<summary>index.js solution</summary>

```js
import React from 'react'
import ReactDOM from 'react-dom'
import './index.css'
import App from './App'
import { HashRouter as Router } from 'react-router-dom'
import {Provider} from 'react-redux
import store from './store'

ReactDOM.render(
  <Router>
    <Provider store={store}>
      <App />
    </Provider>
  </Router>,
  document.getElementById('root')
)
```

</details>

### Step 3

Our next step will be to connect our `MovieForm` component to our reducer and make it update the values on our redux state.

1. In our `MovieForm` component, import `connect` from `react-redux` and `setMovieInfo` from our `moviesReducer.js` file.
2. Wrap your export of `MovieForm` in the second invocation of connect (HINT: `connect` will be invoked twice, once to recieve `mapStateToProps` and a second time to recieve any action creators that we want to pass in).
3. Because we don't need to display any redux state values in this component, just set them, instead of passing `mapStateToProps` to the first invocation of `connect`, we will pass `null` and an object containing our `setMovieInfo` function as the second argument.
4. Finally, we want to modify our `handleSubmit` method to set those values on our redux state. Inside of this method, invoke the props version of `setMovieInfo`, passing it title, poster, and rating from our local state.

> We can use our redux devtools to check that these values are setting properly. Open up your developer tools and find the redux devtools. They will show the current value of your redux state as well as any changes that happen.

<details>
<summary>MovieForm.js solution</summary>

```js
import React, { Component } from 'react'
import { connect } from 'react-redux'
import { setMovieInfo } from '../ducks/moviesReducer'
import styles from './styles'

class MovieForm extends Component {
  //Constructor and handle change methods.  These do not need to be changed.

  handleSubmit = e => {
    e.preventDefault()
    const { title, poster, rating } = this.state

    this.props.setMovieInfo(title, poster, rating)

    this.props.history.push('/confirm')
  }

  //Render method this does not need to be changed.
}

export default connect(
  null,
  { setMovieInfo }
)(MovieForm)
```

</details>

### Step 4

Once we can get our form to properly update the values in our redux state, we need to edit `MovieConfirm` to display them.

1. Import `connect` into your `MovieConfirm` component from `'react-redux'`.
2. Wrap your export of `MovieConfirm` in the second invocation of connect.
3. Outside of your component create a function called `mapStateToProps` which accepts a single argument, our redux state. You can call this whatever you want.
4. Destructure `title`, `poster`, and `rating` from our redux state and return an object containing those three values from our `mapStateToProps` function. When provided to the first invocation of `connect` this will take those values from our redux state and put them on the props of our `MovieConfirm` component.
5. Pass `mapStateToProps` to the first invocation of `connect` and console log `props` in our `MovieConfirm` component to test that these props are being applied properly.
6. The `MovieConfirm` component currently has placehoders of TITLE, RATING, and URL. Change those strings to references to our values on props to make them display properly.

<details>
<summary>MovieConfirm.js solution</summary>

</details>
