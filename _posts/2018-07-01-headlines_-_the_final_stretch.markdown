---
layout: post
title:      "headlines - the final stretch"
date:       2018-07-01 16:15:07 -0400
permalink:  headlines_-_the_final_stretch
---

I was so excited to learn React/Redux in spite of what I've heard of this section of Learn to be more difficult to digest. This enthusiasm definitely helped me learn the material well enough to feel comfortable with implementing a variety of solutions and seeing what works.

For my final project, I wanted to work with a well documented external API, and then, model its persistence to similarly constructed attributes in my own rails models. I chose to work with the News API, which is a conglomeration of online media resources bundled into one. I had fun playing around with the parameters, and I based the features of my applications around the clearest parameters available to me.

I'm fascinated by the performant nature of the React/Redux libraries. I have to admit that until now, I've never wanted to learn more about front-end development, but it seems that with React, the possibilities are endless. The reusability of the code is amazing. The single most important part of my process was the planning stage. I drew a layout of what I wanted my app to look like, and then, I based my components on the resulting image.

Because I had planned in advance which components would be stateful containers and which would be children, stateless function components, I found that once I had a good handle on my app architecture, I was ready to move on to my favorite part of this project:

## Redux

Redux as a tool for managing application state is, by far, my favorite thing I've learned during this course. It's incredibly powerful, and the ability to both modularize state management with the help of `rootReducer` and make external API calls with Redux thunk middleware allows us to create rich depth to our web applications.

```
const rootReducer = (state, action) => {
  if (action.type === 'RESET_USER_STATE') {
    state = undefined;
  }

  return appReducer(state, action);
}

const appReducer = combineReducers({
  user: userReducer,
  stories: storiesReducer,
  searchedStories: keywordReducer,
  savedStories: savedStoriesReducer,
  categoryStories: categoryStoriesReducer,
  sources: sourcesReducer,
  categories: categoriesReducer
})

export default rootReducer;
```

The Redux store holds the whole state of our application. Within the store lives an action dispatcher: the dispatch function. It accepts an argument of an action, which is a JS object that consists of `type` and `payload`. While I didn't structure my action objects with `payload` keys, instead opting to have each state as its own key in the action object, it's definitely something I'd consider refactoring in the future if that is best practice.

After an action is dispatched, the reducer function, also within the Redux store, sets a new state. This process is repeated as necessary as the user of our application interacts with the front-end.

For example, in my application, users simply have to enter a username in order to save top U.S. headlines to the database and refer to those articles later.

```
  handleSubmit = (event) => {
    event.preventDefault();
    let username = this.state.username;

    fetch('http://localhost:3001/users')
      .then(resp => resp.json())
      .then(users => users.find((user, idx) => user.username === username))
      .then(user => {
        if (!user) {
          return this.props.createUser(username);
        } else {
          return this.props.fetchUser(username);
        }
      });
  }
	```
	
	`createUser` and `fetchUser` are both action creators that user Redux thunk middlware in order to fetch to an API and then dispatch an action object that contains JSON data that I've extracted from the API call.
	
	```
	export function fetchUser(username) {
  return (dispatch) => {
    return fetch(`http://localhost:3001/users`)
      .then(resp => resp.json())
      .then(userArray => userArray.find((user) => user.username === username))
      .then(user => {
        // debugger;
        if (!!user) {
          dispatch({ type: 'SET_USER_STATE', user: user });
        } else {
          createUser(username);
        }
      });
  };
}

export function createUser(username) {
  const args = {
    method: 'POST',
    mode: 'cors',
    headers: {
    Accept: 'application/json',
    'Content-Type': 'application/json'
    },
    body: JSON.stringify({username: username})
  };

  return (dispatch) => {
    return fetch(`http://localhost:3001/users/`, args)
      .then(resp => resp.json())
      .catch(error => console.log(error))
      .then(user => dispatch({ type: 'SET_USER_STATE', user: user }));
  };
}
```

After the action is dispatched, my `userReducer` dictates how to set the `user` state.

```
export default function userReducer(state = {}, action) {
  switch (action.type) {
    case 'SET_USER_STATE':
      return action.user;
    case 'RESET_USER_STATE':
      return action.user;
    default:
      return state;
  }
}
```

Pretty cool! Feel free to check out the rest of the code. I'd be happy to collaborate on something, and contributions are always welcome. Here's the link to the [repo](https://github.com/rh24/headlines-api)!

### Challenges

I think the challenging part of this was convincing myself to allow some flexibility in what can be has to be a container and what has to be a component. I'd found along the lines that some of my components that I'd planned for to be stateless might be better off being stateful so that I can map state to props. It really depended on how I wanted to user to be able to interact with my app. This is only something I figured out the more I played around with what I'd built and the more I ran into bugs.

All in all, I'm really excited to keep building upon what the solutions I've come up with. I'm even more excited to see how others have been leveraging React/Redux in their own personal projects!

