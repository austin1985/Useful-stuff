## Implementing Redux with React ##

- **Step install dependencies**
  1. react-redux
  2. redux
  3. redux-thunk 
    
- **Create action types and actions**
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
  Actions are exported functions, these functions are called by the react components. The functions can call the dispatch     function which will update the state in the store. 
  
  
