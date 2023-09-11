# Pokemon-js
## Hosted Link :-
Creating a basic Pokemon finder using JavaScript involves fetching data from a Pokemon API and displaying it in a user-friendly interface. 
In this example, I'll demonstrate how to use the PokeAPI to search for and display information about Pokemon using HTML, CSS, and JavaScript.
Create the HTML structure for your Pokemon finder:

## HTML code
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <link rel="stylesheet" href="./style.css">
</head>
<body>
    <div class="mainContainer">
        <form>
            <input type="text" id="searchBar">
            <button id="search">Search</button>
            <button id="resetButton">Reset</button>
        </form>
        <div class="cardsContainer" id="cardsContainer">
           
        </div>
    </div>
    <script src="./index.js"></script>
</body>
</html>
```
Create a CSS file (style.css) to style your Pokemon finder:
## CSS Code
```
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  
  form {
    outline: none;
    height: 10vh;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  
  input {
    margin: 1rem;
    height: 50%;
    width: 80ch;
    border: none;
    border-bottom: 2px solid black;
    outline: none;
    padding: 0 1rem;
    color: balck;
    font-size: 0.9rem;
    font-weight: 600;
  }
  
  button {
    margin: 1rem;
    padding: 0.5rem 1rem;
    background-color: royalblue;
    color: white;
    font-size: 1rem;
    border: none;
    outline: none;
    border-radius: 5px;
    font-weight: 600;
  }
  button:hover{
    background-color: white;
    color: black;
    cursor: pointer;
    border: 1px solid black;
    border-radius: 3px;
  }
  
  .cardsContainer {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;
  }
  
  .cardDiv {
    margin: 1rem;
    width: 20%;
    height: 350px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;
    position: relative;
    padding: 2rem;
    box-shadow: inset 2px 2px 10px grey, inset 1px 1px 5px grey;
    border-radius: 5px;
  }
  
  .cardDiv span {
    position: absolute;
    top: 20px;
    right: 20px;
  }
  
  span {
    width: 30px;
    height: 30px;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: transparent;
    color: royalblue;
    font-size: 1.5rem;
    font-weight: 600;
    font-family: cursive;
  }
  
  .abilitiesDiv {
    display: flex;
    width: 100%;
    justify-content: space-between;
  }
  
  .abilitiesDiv p {
    padding: 0.5rem 1rem;
    border-bottom: 1px dotted black;
    font-size: 1.2rem;
  }
  
  h2{
    color: rgb(0, 0, 0);
    text-transform: uppercase;
    color: royalblue;
    font-family: cursive;
  }
  
  img{
    filter: drop-shadow(3px 3px 10px balck);
  }
```
In this script, we listen for a click event on the "Search" button, fetch Pokemon data from the PokeAPI 
based on the user's input, and display it in the result div.
Accesses DOM elements using document.getElementById.
Adds a click event listener to the "Search" button.
Retrieves the user's input from the input field and converts it to lowercase.
Performs data fetching from the PokeAPI using the fetch function.
Checks if the response is successful (HTTP status 200) and parses the JSON response.
Displays the fetched Pokemon's name, image, type(s), height, and weight in the result div.
Handles errors by displaying an error message if the Pokemon is not found or if there's any other issue with the request.
## JAVASCRIPT Code
```
const SearchBar = document.getElementById("searchBar");
const cardsContainer = document.getElementById("cardsContainer");
const searchButton = document.getElementById("search");
const resetButon = document.getElementById("resetButton");

const pokemonDetails = [];
const fetchPokemonDetails = () => {
  const promises = [];
  for (let i = 1; i <= 150; i++) {
    const pokemonUrl = `https://pokeapi.co/api/v2/pokemon/${i}`;
    const promise = fetch(pokemonUrl).then((response) => {
      return response.json();
    });
    promises.push(promise);
    // promises.push(fetch(pokemonUrl).then((response) => response.json()));
  }
  Promise.all(promises).then((data) => {
    data.map((ele) => {
      console.log();
      const pokemonObj = {
        frontShinyImg: ele.sprites["front_shiny"],
        id: ele.id,
        name: ele.name,
        abilities: ele.abilities.map((item) => {
          return item.ability.name;
        }),
        // abilities: ele.abilities.map(ele => ele.ability.name),
        types: ele.types[0].type.name,
      };
      pokemonDetails.push(pokemonObj);
    });
    pokemonDetails.map((pokemon) => {
      createPokemonCard(pokemon);
    });
  });
};

const createPokemonCard = (pokemon) => {
  const cardDiv = document.createElement("div");
  const span = document.createElement("span");
  const img = document.createElement("img");
  const heading = document.createElement("h2");
  const abilitiesDiv = document.createElement("div");
  const abilitesparas = pokemon.abilities.map((ele, idx) => {
    const abilityPara = document.createElement("p");
    abilityPara.innerText = ele;
    return abilityPara;
  });
  const typePara = document.createElement("p");

  span.innerText = pokemon.id;
  img.src = pokemon.frontShinyImg;
  heading.innerText = pokemon.name;
  typePara.innerText = pokemon.types;
  cardDiv.classList.add("cardDiv");
  cardDiv.appendChild(span);
  cardDiv.appendChild(img);
  cardDiv.appendChild(heading);
  abilitesparas.map((ele) => {
    // console.log(ele);
    abilitiesDiv.appendChild(ele);
  });
  cardDiv.appendChild(abilitiesDiv);
  abilitiesDiv.classList.add("abilitiesDiv");
  cardDiv.appendChild(typePara);
  cardsContainer.appendChild(cardDiv);
};

searchButton.addEventListener("click", (e) => {
  e.preventDefault();
  console.log(SearchBar.value);
  const filteredValues = pokemonDetails.filter((ele) =>
    ele.name.includes(SearchBar.value.toLowerCase())
  );
  console.log(filteredValues);
  cardsContainer.innerHTML = "";
  filteredValues.map((pokemon) => {
    createPokemonCard(pokemon);
  });
  SearchBar.value = "";
});

resetButon.addEventListener("click", (e) => {
  e.preventDefault();
  cardsContainer.innerHTML = "";

  fetchPokemonDetails();
});

fetchPokemonDetails();
```


