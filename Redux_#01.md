1.the most widely used state management tool for Reactjs

2.predictable

3.current data within a single store 

4.state - read only ; action dispatched -> via a reducer, state updated 

5.reducer - changes updated via pure functon only not to make changes to the state; take a new state and action -> a new state; centralized data , a new state occurs

<Redux develop dev tool>

data mgt through redux

6.trace a new state 

7.data in a global component can be handed down to children components

8.core concepts:

- actions
- reducer

9.createstore()

store.dispatch( action...)

10. Data flow

dispathcer which is attached to Store -> reducer - > a new state -> if a new action is taken, the action goes thru a dispatcher

11. React-redux

.connect() ; f()

Component, <provider> takes in either of the follwoing functions

-mapStateToProps : connect state with props - each time the store changes, the function executes

-mapDispatchToProps :

In Summary:

-use onely one store 

-cannot alter the state of the store : to change the store,an action needs to dispatch

-reducer, a function that processes the action object ; thd function defines update of data

-reducer : pure function, not asynchronously perforrm, cannot access to the db, or network, cannot change the parameters

Data flow

- view requests an action and action creator creates an action
- view hands the action over to Store
- Store sends current status tree and action to root reducer
- the root reducer sorts out the current status and sent it to the sub reducer in charge
- Root reducer compiles them all and 

   











