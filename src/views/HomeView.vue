<template>
<header>
        <h1>Välkommen till BurgerHeaven!</h1>
    </header>
    <main>

    <section id="BurgerInfo">
    <h2>Vår meny</h2>
       <p>Välj bland våra underbara burgare! </p>

      <div class = wrapper>
        <Burger
          v-for="burger in burgers"
          v-bind:key="burger.name"
          v-bind:burger="burger"
          v-on:orderedBurger="addToOrder($event)"
        />
      </div>
    </section>

    <section id="KundInfo">
    <h2>Kundinformation</h2>
        <p> Fyll i kontaktuppgifter och leveransadress! </p>
        <p>
            <label for="firstname">För- och efternamn: </label>
            <input type="text" id="firstname" v-model="namn" required placeholder="Namn">
        </p>
        <p>
        <p>
            <label for="email">Mejladress: </label>
            <input type="email" id="email" v-model="em" required placeholder="Mejl">
        </p>
        <p>
            <label for="betalmetod">Betalmetod: </label>
            <select id="betalmetod" v-model="btl">
                <option selected="selected">Swish</option>
                <option>Kortbetalning</option>
                <option>Presentkort</option>
                <option>Kontant vid leverans</option>
            </select>
         </p> Kön: <br>
         <label>
            <input type="radio" v-model="gender" value="man">
            Man
        </label><br>
        <label>
            <input type="radio" v-model="gender" value="kvinna" checked>
            Kvinna
        </label><br>
        <label>
            <input type="radio" v-model="gender" value="vill-ej-ange">
            Vill ej ange
        </label>
        </p>
    </section>
 
<div class = "map-container">
  <button id="map" v-on:click="setLocation"> Välj plats </button>
  <div v-if="location.x !== null && location.y !== null" :style="{ left: location.x + 'px', top: location.y + 'px' }" class="dot"> T </div>
</div>

<button v-on:click="orderButton">Skicka Beställning</button>
    </main>
    <hr>
    <footer>
        &copy; 2024 hejhejBurgerCompany 
    </footer>
    
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menuData from '../assets/menu.json'

const socket = io("localhost:3000");

/*function MenuItem (name, imageUrl, kCal, hasGluten, hasLactose) {
  this.name = name;
  this.imageUrl = imageUrl;
  this.kCal = kCal;
  this.hasGluten = hasGluten;
  this.hasLactose = hasLactose;
}

const burgers = [
  new MenuItem("Juicy", "https://www.foodandwine.com/thmb/DI29Houjc_ccAtFKly0BbVsusHc=/1500x0/filters:no_upscale():max_bytes(150000):strip_icc()/crispy-comte-cheesburgers-FT-RECIPE0921-6166c6552b7148e8a8561f7765ddf20b.jpg", 500, false, true ),
  new MenuItem("Datenight", "https://media.istockphoto.com/id/1302436326/photo/junk-food-homemade-beef-burgers-on-vintage-wooden-background.jpg?s=612x612&w=0&k=20&c=NsyDE31unoNd80wGfrkMOqvsnjeNOpHER-yL_8KwcRw=", 400, false, false),
  new MenuItem("Cheesy", "https://www.foodandwine.com/thmb/XE8ubzwObCIgMw7qJ9CsqUZocNM=/1500x0/filters:no_upscale():max_bytes(150000):strip_icc()/MSG-Smash-Burger-FT-RECIPE0124-d9682401f3554ef683e24311abdf342b.jpg", 500, true, true),
];

console.log(burgers);*/

export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: menuData,
      namn: "",    
      em: "",      
      btl: "Swish",
      gender: "kvinna", 
      orderedBurgers: {}, 
      location: { x: null, y: null}
  };
},

  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },

    setLocation: function(event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      this.location.x = event.clientX - 12 - offset.x
      this.location.y = event.clientY - 12 - offset.y
    },
    orderButton: function(){
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y, 
                                           firstname: this.namn,
                                           email: this.em,
                                           betalmetod: this.btl,
                                           gender: this.gender },
                                orderItems: this.orderedBurgers
                              }
                 );
    }
  },

  addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;
    },
  };
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Agbalumo&family=Cormorant:wght@700&display=swap');

.Ingredient {
    font-weight: bold;
}

#KundInfo {
    background-color: black;
    color: white;
}

body {
    font-family: "Times New Roman", Times, serif;
    font-size: 14pt;
}

p {
    font-family: "Times New Roman", Times, serif;
    font-size: 14pt;
    color: rgb(229, 14, 233);
    text-align: center;
}

h1 {
    font-family: 'Agbalumo';
    font-size: 36pt;
    color: rgb(229, 14, 233);
    position: absolute;   
    text-align: center;       
}

h2 {
    font-family: "Times New Roman", Times, serif;
    font-size: 20pt;
    color: rgb(139, 5, 142);
    text-decoration: underline;
    text-align: center;
}
h3 {
    font-family: "Times New Roman", Times, serif;
    font-size: 16pt;
    color: rgb(139, 5, 142);
    text-decoration: underline;
    text-align: center;
}
ul li {
    font-family: "Times New Roman", Times, serif;
    font-size: 14pt;
    color: rgb(139, 5, 142);
}
main, header, footer, nav ul {
    width: 100%;
    margin: 0 auto 0 auto;
}
main {
    background-color: rgb(254, 238, 255);
}

button {
    font-family: 'Agbalumo';
    font-size: 20pt;
    color: rgb(229, 14, 233); 
    text-align: center;           
    text-shadow: 3px 3px 5px white, -3px -3px 5px white, 3px -3px 5px white, -3px 3px 5px white;
    background-color: rgb(254, 238, 255);
    padding: 10px 20px;         
    margin: 20px 0;             
    border: 2px solid #f41ecd; 
    margin-left: 40%
}

button:hover {
    background-color: #f41ecd; 
    cursor: pointer; 
}

/* nav ul li {
    display: inline-block;
    background-color: grey;
    padding: 1em;
    margin: 1em;
} */

section {
    margin: 20px;   
    padding: 15px;  
    border: 1px solid #f41ecd; 
}

div {
    margin: 20px;   
    padding: 15px;  
    border: 1px dashed #f41ecd; 
}


.wrapper {
  display: grid;
  grid-gap: 10px;                  /* Avstånd mellan varje burger */
  grid-template-columns: repeat(3, 1fr); /* Tre lika breda kolumner */
  width: 100%;                     
  height: 100vh;                   
  padding: 20px;                   
  box-sizing: border-box;         
}

.wrapper img {
    width: 50%;  
    height: auto; 
}

header {
    background-image: url("https://t3.ftcdn.net/jpg/07/13/14/24/360_F_713142423_Rkh3b7Xjc3XrxN6I6buMEo9kTv3uLQiq.jpg");
    background-size: cover;
    background-position: center;
    display: flex;                /* Aktivera Flexbox på headern */
    justify-content: center;      /* Centrera horisontellt */
    align-items: center;          /* Centrera vertikalt */
    width: 100%;
    height: 200px;                /* Sätt höjd på headern */
    position: relative;           /* För Flexbox och andra positioneringar */
    opacity: 0.7;
}

header h1 {
    margin: 0;                    
    text-align: center;           
    text-shadow: 3px 3px 5px white, -3px -3px 5px white, 3px -3px 5px white, -3px 3px 5px white;
}

nav ul {
    display: grid;
    grid-template-columns: repeat(auto-fill, 9.25em);
    gap: 1em;
    padding: 0;
}

nav li {
    display: block;
    background-color: grey;
    padding: 1em;
}

.Very-good {
    color: green;
}

.Master {
    color: green;
    font-weight: bold;
}

#map {
    width: 1920px;
    height: 1078px;
    background: url("/img/polacks.jpg");
    text-align: center;         
    text-shadow: 3px 3px 5px white, -3px -3px 5px white, 3px -3px 5px white, -3px 3px 5px white;
    padding: 0;         
    margin: 0; 
    position: absolute;            
  }

.map-container {
  width: 95%; 
  height: 600px;
  overflow: scroll;
  border: 2px solid #f41ecd;  
  position: relative;

}

.dot {
  position: absolute;
  background-color: rgb(229, 14, 233);
  text-align: center;
  width: 5px;
  height: 5px;
  border-radius: 50%;
  font-size: 15pt;
  color: black; 
}

</style>