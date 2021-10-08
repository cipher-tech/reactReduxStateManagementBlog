
# React State Management Blog

 **Outline**:

-    Introduction. 
    
-   What is state?
    
-   state management
    
-   Building the App
    
-   Working with Redux
    
-   Adding and setting up Redux
    
-   Connecting our store to react components:
    
-   Summary
    

  

## Introduction:

  

In the world of single-page applications(SPAs) and ReactJs state management has always been a subject of discussion. Do I do it this way or that way, which state management library should be used and why? What's the performance impact of this method? Is this pattern still valid?  
  

Well, the questions keep coming and the truth is there's no perfect answer, so deciding how to handle state can be tricky at times, and having a good understanding of the popular state management libraries can help in deciding which of them to use.

  

This is a three-part tutorial that covers three popular react state management libraries and tools, namely: Redux, Recoil, and Context API. In this article, we’ll talk about popular react state management options, Redux. We Will cover Recoil and Context API in upcoming posts in this series.

We’ll build a simple to-do app to add, delete, and change the status of a task with each of the state management tools we’ll be looking at and see them in action.

This will help us know how to set up each of them and the boilerplate involved.

  

We’ll try to answer questions like:

-   pros and cons
    
-   When we should use each of them
    
-   When not to use each of them.
    

  

*Note: This article isn’t an introduction to React.
This tutorial is aimed at readers who are interested in developing React applications that require a state management library. A basic understanding of React, hooks and a bit of typescript is required. If you’re a beginner with React and state management in React, please go through the basics before reading this tutorial.*

  
  

What Is State?

Simply put, a state is a javascript object that is used by ReactJs to store mutable values of a component and can be updated based on events defined in the code. Another way of putting it is that a state is where information about a component is stored, something like a memory. This is what tells a component how and what to render.

  

In ReactJs only a change in state causes a component to rerender, when a state changes React decides whether to re-render the component, and in doing so all its child components re-renders as well and this might bring about performance issues. Also, remember that...

![](https://lh3.googleusercontent.com/ZNHe30qHUOKph31G5OB1vjeQV21c6zKE9EiBX4kCDEaysmo3RtmUhDUzLeEGlyj20g7P01aofuZJOKJs-9-2sjgmZzNzaM9YNgQpUYY3_6FPGhdW95l4bzuMa3GuPmaPYFOx570h=s0)

  

In our todo app every action we make triggers a state change. Adding, deleting, marking a task as completed in our app all trigger a change in state and that is why our component can re-render and display accurate information.

A state can only be changed by the component that owns it, if you need a child component to trigger a change in its parent state you’ll have to pass the setState function as props to the child.

  

Now imagine we are building a large app like an eCommerce website and the component that needs to update the state is nested five(5) components deep, which means you’ll have to pass the props through four(4) different components that don't need it (prop Drilling). Or do we keep creating states for every component? If so how do you sync the information 🤷🏼‍♂️? Wahala.

### State Management

Managing State gets messy as the app grows bigger. That's why we use state management tools like Redux, Recoil, and Context to help make the app cleaner. They make it possible for us to have a global state and only components that need to read or update the state can subscribe, no prop drilling.

In the coming sections, we will look at the state management library Redux and in the subsequent posts, we’ll cover Recoil and Context API, how to set them up, and what to consider before going for any of them.

  
  

## Building the App:

Navigate into any folder of your choice and run the following commands to clone the repo, install dependencies and start the project:

  

    $ git clone https://github.com/cipher-tech/react-state-management-tutorial.git
    
    $ cd react-state-management-tutorial
    
    $ npm install
    
    $ npm start

  
  

The last command will start the development server on port 3000 and open up a new page on our web browser. It should look like this:

![](https://lh3.googleusercontent.com/mBYrqU79JYF95P9hJZFmXM6zeOBq3ot7csSWQkRu-ezBYfGvHgcWzO720kSS4txSAC7JjWJhPD69W8bzrsNQfJwep2Ma7TJWsJYP4Ov1iE9M_mwohi15j2mJcpLfgDCGU2NTN_A4=s0)

## Working with Redux:

According to the official redux website, it’s defined as 

> “A Predictable State Container for JS Apps”.

With Redux you can develop applications that are consistent, predictable, and run in different environments. Although Redux and React are commonly used together, it is important to know that they are independent of each other. Redux is a standalone library and can be used with different UI libraries/frameworks like React, Vue, Angular. Etc.

`

> “Redux serves as a centralized store for state that needs to be used
> across your entire application, with rules ensuring that the state can
> only be updated predictably.”

[redux.org](https://redux.js.org/tutorials/essentials/part-1-overview-concepts#what-is-redux)

  

![](https://lh3.googleusercontent.com/RcXJHzi0Xz3J_uYorgbq9fD9_Skim0Bn0tzSkaUOq0SBvqSUJdgjmauakXxDQlqGCBynyDRpzwyKhk39yXuYkwGBfdTC38h_W7uVa-LovNXHFaI2Z7a8w9h5inf7qjXGMiZMHrcK=s0)

## Adding and setting up Redux:

    $ npm install react-redux @reduxjs/toolkit

To install Typescript types for the redux library run this command:

    $ npm install @types/react-redux

  

Open the `src` folder and make a directory called `/store`, then in the store folder create another directory called `/redux`.

Now in `src/index.tsx`, edit the ReactDOM.render method to look like this:

    ReactDOM.render(
    <Provider store={store}>
    <React.StrictMode>
    <App />
    </React.StrictMode>
    </Provider>,
    document.getElementById("root")
    );

  

First for our app to use Redux we have to wrap it with the Provider component and pass our store as props.

Don’t forget to import Provider and our store at the top of the file:

    import  {  Provider  }  from  "react-redux";
    import  {  store  }  from  "./store/redux";

  

Inside `store/redux` lets create two files `index.tsx` and `TodoReduce.tsx`.

We use the `.tsx` extension because we’re working with typescript.

  

Next, we define the types for our state, we expect our state to be an array of objects with the `id`, `title`, and `isCompleted` property. Let us add a `types` folder in our `src` directory and create an `/index.ts` file and add the code below:

    export  type ITaskProperty =  {
    title: string;
    completed: boolean;
    id: number;
    };

Here we export a type `ITaskProperty` and define the types for our state object. Next we use it in our `TodoReducer.tsx` file in the `/store` folder:

    import  {  ITaskProperty  }  from  "../../types";
    export  interface TaskState {
    allItems:  ITaskProperty[];
    }  

Here we say our state will be a single property “`allItems`” which will be an object with properties that match our exported `ITaskProperty`.

Next, we define our state with initial values in `TodoReducer.tsx` file:

    // we create our state with initial values
    const initialState:  TaskState  =  {
	    allItems:  [
		    {
			    id:  8838,
			    title:  "Write code",
			    completed:  true,
		    },
	    ],
    };

Next is the heavy lifting, we create our `actions` and `reducers`. There are many ways to create actions and reducers but I'll be following the implementation from the official Redux repo. To create our actions and reducers we’ll call a redux method `createSlice` with a slice name, an initial state, and an object full of `reducer` functions, and it automatically generates action creators and action types that correspond to the reducers and state. We then assign the result to a variable which we’ll export, this will be what we’ll create our store from.

        export const todoSlice  =  createSlice({
        name:  "todo",
        initialState,
        // The `reducers` field lets us define reducers and generate associated actions
        
        reducers:  {
    	    addItem:  (state, action:  PayloadAction<string>)  =>  {
    	    // Redux Toolkit allows us to write "mutating" logic in reducers. It
    	    // doesn't actually mutate the state because it uses the Immer library,
    	    
    	    // which detects changes to a "draft state" and produces a brand new
    	    // immutable state based off those changes
    		    state.allItems =  [
    			    ...state.allItems,
    			    {
    			    id: Math.floor(Math.random()  *  10000),
    			    title: action.payload,
    			    completed:  false,
    			    },
    		    ];
    	    },
        
    	    deleteItem:  (state, action:  PayloadAction<number>)  =>  {
    		    state.allItems = state.allItems.filter(
    			    (item)  => item.id !== action.payload
    		    );
    	    },
        
        // Use the PayloadAction type to declare the contents of `action.payload`
        
    	    completed:  (state, action:  PayloadAction<number>)  =>  {
    		    state.allItems = state.allItems.map((item)  =>  {
    			    item.completed =
    			    item.id === action.payload ?  true  : item.completed;
    			    return item;
    			   });
    		    },
    	    },
    });

From the code you can see we have three reducers `addItem`, `deleteItem`, `completed`. These are the operations we’ll be able to perform in our state. The `addItem` reducer adds an item to our state by first copying all of the existing state, then adding the new item to the state.

*Note: reducers always return a new state every time so to make changes you have to create a new state and copy the old one to it, else it will be replaced.*

The `deleteItem` and `completed` reducers follow the same pattern, `deleteItem` creates a new array excluding the one we want to delete, `completed` reducer sets the completed property of an item to true.

The `PayloadAction` is a type from the redux toolkit that tells the typescript compiler the type of augment we’ll be passing to the action, for addItem it must be a string, for deleteItem and completed reducers it must be a number.

    Import the methods from the redux toolkit at the top of the file.
    import  {  createSlice,  PayloadAction  }  from  "@reduxjs/toolkit";

Next we destructure and export our actions automatically generated by `createSlice` from the `todoSlice` variable like so:

    export const { addItem,  deleteItem,  completed } =  todoSlice.actions;

Remember we can not call our reducers or modify our state directly, we must dispatch these actions to trigger a reducer to modify our state.

    Next lest head over to the `store/redux/index.tsx` file and add this code:
    
    import  {  configureStore  }  from  '@reduxjs/toolkit';
    import  todoReducer  from  './TodoReduce';
    
    export const store  =  configureStore({
	    reducer:  {
		    todo: todoReducer,
		},
    });
    
    export  type AppDispatch =  typeof  store.dispatch;
    export  type RootState =  ReturnType<typeof  store.getState>;

First, we `import` our modules, next we use the configureStore function from the redux toolkit to create our store by passing it to our reducer. The configureStore store function takes an object as an argument, then in the reducer property we pass our reducer, we can have more than one. The last two lines are to add types for our state and dispatch function.

The last step, let's go back to `TodoReducer.tsx` and add these lines to the bottom of the file:

    // The function below is called a selector and allows us to select a value from
    // the state. Selectors can also be defined inline where they're used instead of
    // in the slice file. For example: `useSelector((state: RootState) => state.counter.value)`
    
    export const getStore  =  (state:  RootState) => state.todo;
    export  default  todoSlice.reducer;

The first export is our selector, to select values from our state, if you are used to redux you can see it as the `mapStateToProps` function for class components. The last line exports our reducer. The final `TodoReducer.tsx` will look like this:

    import  {  createSlice,  PayloadAction  }  from  "@reduxjs/toolkit";
    import  {  ITaskProperty  }  from  "../../types";
    import  {  RootState  }  from  "./index";
    
    export  interface ITaskState {
	    allItems:  ITaskProperty[];
	}
    // we create our state with initial values
    const initialState:  ITaskState  =  {
	    allItems:  [
		    {
			    id:  8838,
			    title:  "Write code",
			    completed:  true,
		    },
		    {
			    id:  8844,
			    title:  "Get some sleep",
			    completed:  true,
		    },
		],
    };
    export const todoSlice  =  createSlice({
    name:  "todo",
    initialState,
    
    // The `reducers` field lets us define reducers and generate associated actions
    
    reducers:  {
	    addItem:  (state, action:  PayloadAction<string>)  =>  {
	    // Redux Toolkit allows us to write "mutating" logic in reducers. It
	    // doesn't actually mutate the state because it uses the Immer library,
	    // which detects changes to a "draft state" and produces a brand new
	    // immutable state based off those changes
	    
		    state.allItems =  [
			    ...state.allItems,
			    {
				    id: Math.floor(Math.random()  *  10000),
				    title: action.payload,
				    completed:  false,
				},
			];
	    },
	    deleteItem:  (state, action:  PayloadAction<number>)  =>  {
		    state.allItems = state.allItems.filter(
			    (item)  => item.id !== action.payload
		    );
	    },
    
    // Use the PayloadAction type to declare the contents of `action.payload`
    
	    completed:  (state, action:  PayloadAction<number>)  =>  {
		    state.allItems = state.allItems.map((item)  =>  {
			    item.completed =
			    item.id === action.payload ?  true  : item.completed;
			    return item;
				});
			},
	    },
    });
    
    export const { addItem,  deleteItem,  completed } =  todoSlice.actions;
    
    // The function below is called a selector and allows us to select a value from
    // the state. Selectors can also be defined inline where they're used instead of
    // in the slice file. For example: `useSelector((state: RootState) => state.counter.value)`
    
    export const getStore  =  (state:  RootState) => state.todo;
    export  default  todoSlice.reducer;

  

Phewww… welcome to the world of Redux, we’ve set up our store but we are not done just yet, we have to consume it in our app, so let's do that.

## Connecting our store to react components:

First let’s add a folder in the `/src` directory called `/components` in components let’s create another folder called `/todo`, now inside `/todo` lets create `/Todo.tsx` and `/todo.css`, `Todo.tsx` will be our main file so let’s import it in `src/index,tsx`. The final `index.tsx` should look like this:

    import  React  from  "react";
    import  ReactDOM  from  "react-dom";
    import  "./index.css";
    import  reportWebVitals  from  "./reportWebVitals";
    import  Todo  from  "./components/todo/Todo";
    import  {  Provider  }  from  "react-redux";
    import  {  store  }  from  "./store/redux";
    
    ReactDOM.render(
    <Provider store={store}>
    <React.StrictMode>
    <Todo />
    </React.StrictMode>
    </Provider>,
    document.getElementById("root")
    );
    
    // If you want to start measuring performance in your app, pass a function
    // to log results (for example: reportWebVitals(console.log))
    // or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
    
    reportWebVitals();

  
Let’s build from the smaller components. In the components folder create a folder called `/createTask` and inside it a file called `CreateTask.tsx`. CreateTask is just a form with one controlled input that sends the value of the input to the parent with an addTask prop on submit.

    import  React,  {  useState  }  from  'react';
    
    interface ICreateTaskProps {
	    addTask:  (title:  string)  =>  void
    }
    
    export  default  function  CreateTask({ addTask }:  ICreateTaskProps)  {
	    const [value,  setValue] =  useState("");
	    const handleSubmit  =  (e:  React.FormEvent) => {
		    e.preventDefault();
		    if (!value) return;
		    addTask(value);
		    setValue("");
	    }
    
	    return  (
		    <form onSubmit={handleSubmit}>
			    <input
				    type="text"
				    className="input"
				    value={value}
				    placeholder="Add a new task"
				    onChange={e  =>  setValue(e.target.value)}
			    />
		    </form>
	    );
    }

In the same way, we create a `/taskCard` folder inside the components folder and in it a `TaskCard.tsx` file. This file is responsible for displaying one task with options to delete, and also mark the task as completed by passing the task id to the parent.

    type ITaskProperty =  {
	    title: string,
	    completed: boolean,
	    id: number
    }
    
    interface IProps {
	    task:  ITaskProperty,
	    index?:  any
	    completeTask:  (task:  number)  =>  void
	    removeTask:  (task:  number)  =>  void
    }
    export  default  function  Task({ task,  index,  completeTask,  removeTask }:  IProps)  {
	    return  (
		    <div
		    className="task"
		    style={{ textDecoration: task.completed ?  "line-through"  :  ""  }}
		    >
			    {task.title}
			    <button style={{ background:  "red"  }} onClick={() =>  removeTask(task.id)}>x</button>
			    <button onClick={() =>  completeTask(task.id)}>Complete</button>
		    </div>
	    );
    }

Next to our main file `Todo.tsx` where everything happens, first, we import our modules.

import  React,  {  useState,  useEffect  }  from  "react";
import  {  useDispatch,  useSelector  }  from  "react-redux";
import  {  addItem,  completed,  deleteItem,  getStore  }  from  "../../store/redux/TodoReduce";
import  {  ITaskProperty  }  from  "../../types";
import  CreateTask  from  "../createTask/CreateTask";
import  Task  from  "../taskCard/TaskCard";
import  "./todo.css";
  
`useDispatch` is used to `dispatch` actions to our store for our `reducers` to change the store accordingly, while `useSelector` is used to get information from the store, in the next line we import our actions, they are what we’ll be passing to `useDispatch` in order to update our store. The last function `getStore` is the one we use to select a part of our store. You can get the CSS file ([here](https://github.com/cipher-tech/react-state-management-tutorial/blob/master/src/components/todo/todo.css)).

Next we implement our app logic:

    const {allItems} =  useSelector(getStore)
    const dispatch  =  useDispatch()
    const [tasksRemaining,  setTasksRemaining] =  useState(0);
    useEffect(()  =>  {
	    setTasksRemaining(allItems.filter((task)  =>  !task.completed).length);
	},  [allItems]);
	const addTask:  (title:  string) => void  =  (title) => {
	    dispatch(addItem(title));
    };
    
    const completeTask  =  (index:  number) => {
	    dispatch(completed(index));
    };
    
    const removeTask  =  (index:  number) => {
	    dispatch(deleteItem(index));
    };

The first line is used to select a part of our `store(allItems)`, we pass our `getStore` to the `useSelector` hook and destructure `allItems` from the returned value, if you remember in our initial state was named `allItems`.

Next, we assign `useDispatch` to a variable, `dispatch`, in our `useEffect` function we get the incomplete task. `addTask` is a function that is called with the value of our `input(title)` whenever we submit the form, we then invoke our action by calling it with the title inside our dispatch function, this is how we update our state. The same thing happens in the `completeTask` and `removeTask` functions. The final `Todo.tsx` looks like this:

    import  React,  {  useState,  useEffect  }  from  "react";
    import  {  useDispatch,  useSelector  }  from  "react-redux";
    import  {  addItem,  completed,  deleteItem,  getStore  }  from  "../../store/redux/TodoReduce";
    import  {  ITaskProperty  }  from  "../../types";
    import  CreateTask  from  "../createTask/CreateTask";
    import  Task  from  "../taskCard/TaskCard";
    import  "./todo.css";
    
    function  Todo()  {
	    const {allItems} =  useSelector(getStore)
	    const dispatch  =  useDispatch()
	    const [tasksRemaining,  setTasksRemaining] =  useState(0);
	    useEffect(()  =>  {
	    setTasksRemaining(allItems.filter((task)  =>  !task.completed).length);
	    },  [allItems]);
	    
	    const addTask:  (title:  string) => void  =  (title) => {
		    dispatch(addItem(title));
	    };
	    
	    const completeTask  =  (index:  number) => {
		    dispatch(completed(index));
	    };
	    const removeTask  =  (index:  number) => {
		    dispatch(deleteItem(index));
	    };
	    
	    return  (
			    <div className="todo-container">
			    <div className="header">TODO - ITEMS</div>
			    <div className="header">Pending allItems ({tasksRemaining})</div>
			    <div className="tasks">
				    {allItems.map((task:  ITaskProperty, index)  =>  (
					    <Task
					    task={task}
					    index={index}
					    key={index}
					    completeTask={completeTask}
					    removeTask={removeTask}
					    />
				    ))}
				    </div>
				    <div className="create-task">
				    <CreateTask addTask={addTask} />
			    </div>
		    </div>
	    );
    }
    export  default  Todo;

And our project is complete.

For the complete project, you can check the using_redux_store branch on the repo here(​​https://github.com/cipher-tech/react-state-management-tutorial).

  

Summary

We’ve seen how to implement redux in our project, we can agree that there’s a lot of boilerplate setups to be done before we can use redux, perhaps that's the only downside. Apart from set up, it is effective and predictable which is important for big projects, and it lets us divide our state into different reducers as well. Is redux a good choice? Definitely, but let’s see how it compares to the others(Recoil and context).  

Watch out for my next post as I build the same app this time with recoil and we’ll see the difference, till then Peace.






