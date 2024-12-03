<template>
<div>
       <h3>
       {{ burger.name }}
      </h3>
      <img :src="burger.imageUrl" alt="Burger Image">
      <p>{{ burger.description }}</p>
      <ul>
        <li>{{burger.prompts[0]}}</li>
        <li>{{burger.prompts[1]}}</li>
      </ul>
      <ul class="button-container">
        <li >
          <button v-on:click="decreaseAmount">-</button>
        </li>
        <li>
          <p>{{ amountOrdered }}</p>
        </li>
        <li>
          <button v-on:click="increaseAmount">+</button>
        </li>
      </ul>
      <p> Innehåll av gluten/laktos: </p>
      <ul class = "Ingredient">
        <li v-if="burger.gluten == true"> Gluten </li>
        <li v-if="burger.lactose == true"> Laktos </li>
    </ul>
  </div>

</template>

<script>
export default {
  name: 'OneBurger',
  props: {
    burger: Object,
    updateOrder: Function, // Får metoden från HomeView
  },
  data() {
    return {
      amountOrdered: 0, // För att hålla reda på antalet beställda burgare
    };
  },
  methods: {
    increaseAmount() {
      this.amountOrdered++; // Ökar med 1
      this.$emit('orderedBurger', { name: this.burger.name, amount: this.amountOrdered 
      }
    );
    },
    decreaseAmount() {
      if (this.amountOrdered > 0) {
        this.amountOrdered--; // Minskar med 1
      }
      this.$emit('orderedBurger', { name: this.burger.name, amount: this.amountOrdered 
        }
        );
      },
    },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
div {
    margin: 20px;   /* 20px avstånd runt hela sektionen (utanför) */
    padding: 15px;  /* 15px avstånd inuti sektionen (inuti) */
    border: 1px dashed #f41ecd;  /* Tunn kantlinje*/
}

.wrapper {
    display: grid;
    grid-gap: 10px;                  /* Avstånd mellan varje div */
    grid-template-columns: repeat(3, 1fr); /* Tre lika breda kolumner */
}

.wrapper img {
    width: 100%;  /* Gör bilden lika bred som sin föräldrakontainer */
    height: auto; /* Bevarar bildens proportioner */
}

.Burger {
  width: 100%;                     /* Gör varje burger lika bred som sin kolumn */
  max-width: 350px;                /* Maxbredd för varje burger så att de inte blir för stora */
  margin: 0 auto;                  /* Centrerar varje burger inom sin kolumn */
  padding: 0px;                   /* Lägger till lite padding runt varje burger */
  border-radius: 8px;              /* Valfritt för att runda hörnen på varje burger */
}

/* Gör så att varje bild i burgaren anpassar sig efter sin föräldrakontainer */
.Burger img {
  width: 100%;                     /* Gör bilden lika bred som sin föräldrakontainer */
  height: auto;                    /* Bevarar bildens proportioner */
}
p {
    font-family: "Times New Roman", Times, serif;
    font-size: 14pt;
    color: rgb(229, 14, 233);
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

button {
  font-size: 25px;
  font-family: "Times New Roman", Times, serif;
  color: rgb(139, 5, 142);
  cursor: pointer;
}

.button-container {
  list-style: none; /* Ta bort standardpunkter */
  display: grid; /* Gör att barnen visas horisontellt */
  grid-template-columns: repeat(3, 1fr); /* Tre lika breda kolumner */
  border: 1px dashed #f41ecd;  /* Tunn kantlinje*/
}


</style>