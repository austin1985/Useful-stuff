# Implementing Redux with React #

## Install dependencies ##
  1. react-redux
  2. redux
  3. redux-thunk 
    
## Create action types and actions ##
  1. create folder "actions"
  2. in "actions" folder create files "types.js" 
  3. in "actions" folder create all action files needed (auth,profile...etc) 

  Example:
  
  **types.js**
  
  ```code 
  export const GET_ERRORS = 'GET_ERRORS'
  export const SET_CURRENT_USER = 'SET_CURRENT_USER'
  ```
  
  **authActions.js**
  ```code 
  import { GET_ERRORS } from './types'
  import { SET_CURRENT_USER } from './types'
  
  //Login get user token
  export const loginUser = (userData) => dispatch => {
      axios.post('/api/users/login', userData)
          .then(res => {
              //Save to local storage
              const { token } = res.data;
              //Set token to localstorage
              localStorage.setItem('jwtToken', token)
              //Set token to Auth header
              setAuthToken(token)
              //Decode token to get user data
              const decoded = jwt_decode(token)
              //Set current user
              dispatch(setCurrentUser(decoded))
          })
          .catch(err => {
              dispatch({
                  type: GET_ERRORS,
                  payload: err.response.data
              })
          })
  }
  ```

  Types.js defines the name of the actions. It is a convention to use string constants to describe action names.
  Actions are exported functions, these functions are called by the react components. The functions can call the dispatch     function which will update the state based on the reducers in the store. 
  
## Creating the reducers ##
  
  1. create folder "reducers"
  2. in "reducers" folder create files for individual state elements (user,auth,...etc) 
  3. in "reducers" folder create index.js file (combining reducers together)
  
  Example:
  
  **authReducer.js**
  
  ```code
  
  import { SET_CURRENT_USER } from '../actions/types'

  const initialState = {
      isAuthenticated: false,
      user: {}
  }

  export default function (state = initialState, action) {
      switch (action.type) {
          case SET_CURRENT_USER:
              return {
                  ...state,
                  isAuthenticated: !isEmpty(action.payload),
                  user: action.payload
              }
          default:
              return state;
      }
  }
  
  ```
  
  **index.js**
  
  ```code
  
    import {combineReducers} from 'redux'
    import authReducer from './authReducer'
    import errorReducer from './errorReducer'

    export default combineReducers({

        auth: authReducer,
        errors: errorReducer

    })
  
  ```

  Reducers specify how the application's state changes in response to actions sent to the store. Remember that actions only describe what happened, but don't describe how the application's state changes.
In the example we see that every reducer has an initial state with the relevant information. The reducer also contains a function with the switch. In the function you must describe wich actions should be picked up by the reducer and how the state should be altered. In this case the store state should be extended with the user and the isAuthenticated flag should be set to true. 

  Index.js is used to combine reducers together. This way you only need to import this one js if you want to add the reducers to redux. 

## Create store ##

  1. create folder "store"
  2. in "store" folder create store.js

  Example:
  
  **store.js**
  
  ```code
  
  import { createStore, applyMiddleware, compose } from 'redux'
  import thunk from 'redux-thunk'
  import rootReducer from './reducers'

  const initialState = {}

  const middleware = [thunk]

  const store = createStore(
      rootReducer,
      initialState,
      compose(applyMiddleware(...middleware),
      window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
      )
  )

  export default store
  
  ```
  
  For the store we need an initial state , define which middleware we want to use (optional) thunk in this case, and a route reducer. The index js file in the "reducers" folder is used as the root reducer. The last thing we need is a middleware handler. These three objects are needed for creating the store. We use compose so we can add one extra component which is what enables the redux dev tool in chrome and firefox.
  For middleware we are using **thunk** which makes possible that we use async functions inside our actions.
  
  
