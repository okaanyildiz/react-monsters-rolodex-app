react-monsters-rolodex-app

steps to take

*That app uses class components. 


FETCHING AND DISPLAYING THE MONSTER NAMES

Create the App class. 
Create an empty monsters array to track the state of monsters. 
To fetch the data use the componentDidMount() method inside the App.
Inside the componentDidMount(), fetch monsters from the Api: 
https://jsonplaceholder.typicode.com/users

Then setState() the “monsters” state with users. 
Console.log the state to display the coming data.
Map the monsters in return statement in a div and an h1. 

App: 
```
import { Component } from 'react'
import './App.css'
 
class App extends Component {
 constructor() {
   super();
 
   this.state = {
     monsters: []
   }
 }
 
 componentDidMount() {
   fetch('https://jsonplaceholder.typicode.com/users')
     .then(response => response.json())
     .then(users => this.setState(
       () => { return { monsters: users } },
       () => console.log(this.state)
     ))
 }
 
 render() {
   return (
     <div className='App'>
       {
         this.state.monsters.map(monster => {
           return (
             <div key={monster.id}>
               <h1>{monster.name}</h1>
             </div>
           )
         })
       }
     </div>
   )
 }
}
 
export default App

```
  
MAKING THE INPUT SEARCH TO FILTER MONSTERS

Make an input element inside the return statement of the App for the search box. 
Add onChange to the input element.
Inside the onChange pass onSearchChange function.
Make the onSearchChange(event) function beneath the componentDidMount()
Inside the onSearchChange(event) make the searchField const, which gets the event.target.value. And SetState the state with searchField. 
We need to pass onSearchChange, monsters and searchField as destructured const into render() not to refer “this” everywhere inside the render(). 
Add searchField as an empty string to the state of the App.
Inside the render() above the return() statement make the filteredMonsters const, which filters() the monsters according to the value in the searchField.
Inside the return statement we have to map filteredMonsters from now on.
Now we can filter the monsters according to the input value.

App: 

```
import { Component } from 'react'
import './App.css'
 
class App extends Component {
 constructor() {
   super();
 
   this.state = {
     monsters: [],
     searchField: ''
   }
 }
 
 componentDidMount() {
   fetch('https://jsonplaceholder.typicode.com/users')
     .then(response => response.json())
     .then(users => this.setState(
       () => { return { monsters: users } },
       () => console.log(this.state)
     ))
 }
 
 onSearchChange = event => {
   const searchField = event.target.value.toLocaleLowerCase()
   this.setState(() => {
     return { searchField }
   })
 }
 
 render() {
 
   const { monsters, searchField } = this.state
   const { onSearchChange } = this
 
   const filteredMonsters = monsters.filter(monster => {
     return monster.name.toLocaleLowerCase().includes(searchField)
   })
 
   return (
     <div className='App'>
       <input
         className='search-box'
         type='search'
         placeholder='search monsters'
         onChange={onSearchChange}
       />
       {
         filteredMonsters.map(monster => {
           return (
             <div key={monster.id}>
               <h1>{monster.name}</h1>
             </div>
           )
 
 
         })
       }
     </div>
   )
 }
}
 
export default App
```


MAKING THE CARD LIST COMPONENT

Create the CardList component. 
Import CardList into the App.
Stop mapping the monsters inside the App. 
Pass the filteredMonsters as a prop of CardList. 
Map the monsters inside the CardList component.
 
App: 
```
import { Component } from 'react'
import './App.css'
import CardList from './components/card-list.component';
 
class App extends Component {
 constructor() {
   super();
 
   this.state = {
     monsters: [],
     searchField: ''
   }
 }
 
 componentDidMount() {
   fetch('https://jsonplaceholder.typicode.com/users')
     .then(response => response.json())
     .then(users => this.setState(
       () => { return { monsters: users } },
     ))
 }
 
 onSearchChange = event => {
   const searchField = event.target.value.toLocaleLowerCase()
   this.setState(() => {
     return { searchField }
   })
 }
 
 render() {
 
   const { monsters, searchField } = this.state
   const { onSearchChange } = this
 
   const filteredMonsters = monsters.filter(monster => {
     return monster.name.toLocaleLowerCase().includes(searchField)
   })
 
   return (
     <div className='App'>
       <input
         className='search-box'
         type='search'
         placeholder='search monsters'
         onChange={onSearchChange}
       />
       <CardList monsters={filteredMonsters} />
     </div>
   )
 }
}
 
export default App
```
CardList: 
```
import { Component } from 'react'
 
class CardList extends Component {
   render() {
       const { monsters } = this.props
       return (
           <div>
               {monsters.map(monster => (
                   <h1 key={monster.id}>{monster.name}</h1>
               ))}
           </div>
       )
   }
}
 
export default CardList
```
MAKING THE SEARCH BOX COMPONENT

Create the SearchBox component. 
Cut the input element inside the App and paste it into SearchBox. 
Import SearchBox into the App. Pass the necessary props into the component. 
Go to the SearchBox, update the props of the input element. 

App: 
```
import { Component } from 'react'
import './App.css'
import CardList from './components/card-list.component';
import SearchBox from './components/search-box.component';
 
class App extends Component {
 constructor() {
   super();
 
   this.state = {
     monsters: [],
     searchField: ''
   }
 }
 
 componentDidMount() {
   fetch('https://jsonplaceholder.typicode.com/users')
     .then(response => response.json())
     .then(users => this.setState(
       () => { return { monsters: users } },
     ))
 }
 
 onSearchChange = event => {
   const searchField = event.target.value.toLocaleLowerCase()
   this.setState(() => {
     return { searchField }
   })
 }
 
 render() {
 
   const { monsters, searchField } = this.state
   const { onSearchChange } = this
 
   const filteredMonsters = monsters.filter(monster => {
     return monster.name.toLocaleLowerCase().includes(searchField)
   })
 
   return (
     <div className='App'>
       <SearchBox
         className='search-box'
         onChangeHandler={onSearchChange}
         placeholder='search monsters'
       />
       <CardList monsters={filteredMonsters} />
     </div>
   )
 }
}
 
export default App
```
SearchBox: 
```
import { Component } from 'react'
 
class SearchBox extends Component {
   render() {
 
       return (
           <input
               className={this.props.className}
               type='search'
               placeholder={this.props.placeholder}
               onChange={this.props.onChangeHandler}
           />
       )
   }
}
 
export default SearchBox
```
STYLING THE APP

Paste the code below inside the App.css: 

```
body {
 margin: 0;
 padding: 0;
 font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Oxygen',
   'Ubuntu', 'Cantarell', 'Fira Sans', 'Droid Sans', 'Helvetica Neue',
   sans-serif;
 -webkit-font-smoothing: antialiased;
 -moz-osx-font-smoothing: grayscale;
 background: linear-gradient(
   to left,
   rgba(7, 27, 82, 1) 0%,
   rgba(0, 128, 128, 1) 100%
 );
 text-align: center;
}
 
```

Create the search-box.styles.css file and paste the code below inside: 
```
.search-box {
   -webkit-appearance: none;
   border: none;
   outline: none;
   padding: 10px;
   width: 150px;
   line-height: 30px;
   margin-bottom: 30px;
}
```
Go to the CardList component. 
To map the monsters, instead of h1 we will use a container div. 
We will get the monster photos from robohash.org
Update the CardList component:  

```
import { Component } from 'react'
 
class CardList extends Component {
   render() {
       const { monsters } = this.props
       return (
           <div className='card-list'>
               {monsters.map(monster => {
                   const { id, name, email } = monster
                   return (
                       <div className='card-container' key={id}>
                           <img src={`https://robohash.org/${id}?set=set2&size=180x180`} alt={`monster ${name}`} />
                           <h2>{name}</h2>
                           <p>{email}</p>
                       </div>
                   )
               })}
           </div>
       )
   }
}
 
export default CardList
```
Create the card-list.styles.css file.
Paste the code below: 

```
.card-list {
   width: 85vw;
   margin: 0 auto;
   display: grid;
   grid-template-columns: 1fr 1fr 1fr 1fr;
   grid-gap: 20px;
}

```

MAKING THE CARD COMPONENT

Create card.component.jsx and card.styles.css
Make the Card component. 
Inside the Card component move the card-container div. 
Import Card into Cardlist instead of the card-container. Remember to pass the Monsters prop.
Go to the App. Add “Monster Rolodex” with an h1 as the title of the app. Then style it in App.css:

```
.app-title {
 margin-top: 75px;
 margin-bottom: 50px;
 font-size: 76px;
 font-family: 'Bigelow Rules';
 color: #0ccac4;
}
```

Card: 

```
import { Component } from 'react'
import './card.styles.css'
 
class Card extends Component {
   render() {
       const { id, name, email } = this.props.monster
       return (
           <div className='card-container' key={id}>
               <img src={`https://robohash.org/${id}?set=set2&size=180x180`} alt={`monster ${name}`} />
               <h2>{name}</h2>
               <p>{email}</p>
           </div>
       )
   }
}
 
export default Card
 
 
```
 
Card.styles.css: 

```
.card-container {
   display: flex;
   flex-direction: column;
   background-color: #95dada;
   border: 1px solid grey;
   border-radius: 5px;
   padding: 25px;
   cursor: pointer;
   -moz-osx-font-smoothing: grayscale;
   backface-visibility: hidden;
   transform: translateZ(0);
   transition: transform 0.25s ease-out;
}
 
.card-container:hover {
   transform: scale(1.05);
}

```

CardList: 
```
import { Component } from 'react'
import './card-list.styles.css'
import Card from '../../card/card.component'
 
class CardList extends Component {
   render() {
       const { monsters } = this.props
       return (
           <div className='card-list'>
               {monsters.map(monster => {
                   return (
                       <Card monster={monster} />
                   )
               })}
           </div>
       )
   }
}
 
export default CardList
```