# redux

console.clear()
//People Dropping of a form (Action Creators)
const createPolicy = (name, amount) => {
  return { //Action (A form in our Analogy)
    type: 'CREATE_POLICY',
    payload: {
      name: name,
      amount: amount
    }
  };
};

const deletePolicy = (name) => {
  return { //Action (A form in our Analogy)
    type: 'DELETE_POLICY',
    payload: {
      name: name
    }
  };
};

const createClaim = (name, amountOfMoneyToCollect) => {
  return { //Action (A form in our Analogy)
    type: 'CREATE_CLAIM',
    payload: {
      name: name,
      amountOfMoneyToCollect: amountOfMoneyToCollect
    }
  };
};


//Reducers (Departments)
//oldListOfClaims Changed to oldListOfClaims=[]=>This will replace the undefined by empty array (For the first time)
const claimHistory = (oldListOfClaims=[], action) => {
  if(action.type === 'CREATE_CLAIM'){
    //We care about the action (Form)
    return [...oldListOfClaims, action.payload];
  }
  //Else we don't care about the action (Form)
  return oldListOfClaims;
};

const accounting = (bagOfMoney=100, action) => {
  if(action.type === 'CREATE_CLAIM'){
    return bagOfMoney - action.payload.amountOfMoneyToCollect;
  } else if(action.type === 'CREATE_POLICY'){
    return bagOfMoney + action.payload.amount;
  }
    return bagOfMoney;
}

const policies = (listOfPolicies=[], action) => {
  if(action.type === 'CREATE_POLICY'){
    return [...listOfPolicies, action.payload.name];
  } else if(action.type === 'DELETE_POLICY'){
    return listOfPolicies.filter(name => name !== action.payload.name);
  }
  return listOfPolicies;
}

//console.log(Redux);

//Stores=>Collection of different reducers and actions
const {createStore, combineReducers} = Redux;

//combineReducers => Wire together all of the reducers
const ourDepartments = combineReducers({
  claimHistory: claimHistory,
  accounting: accounting,
  policies: policies
});

//createStore => Combine together all of the reducers
const store = createStore(ourDepartments);

//console.log(store);

//Before dispatch we need to create an action
//const action = createPolicy('Pintu', 20);
//console.log(action);

//Dispacth
store.dispatch(createPolicy('Pintu', 20));
store.dispatch(createPolicy('Jhony', 30));
store.dispatch(createPolicy('Jimmy', 40));

store.dispatch(createClaim('Pintu', 120));
store.dispatch(createClaim('Jhony', 50));

store.dispatch(deletePolicy('Jimmy', 50));

//To show the states
console.log(store.getState());
