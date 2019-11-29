<template>
  <div class="field">
    <div class="cardsField">
        <div v-for="(card,index) in cards" :key="index" class="cardContainer">
          <div v-on:click="$emit('card-clicked', index)" :data-key="index" class="card"></div>
        </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "CardsField",
  data() {
    return {
      pos: 0,
      cardPosition: 0
    };
  },
  props: {
    cards: Array
  },
  methods: {
  }
};
</script>


<style scoped lang="scss">
$base-color: #03a9f4;

.field {
  display: flex;
  flex-direction: column;
  justify-content: center;
  margin: 5px;
  .cardsField {
  
    display: grid;
    grid-template-columns: 100px 100px 100px 100px 100px 100px;
    grid-gap: 10px;
    align-items: start;
    .cardContainer {
      width: 100px;
      height: 100px;
      .selected {
        background: $base-color !important;
        color: white !important;
        transform: scale(1.05);
      }
    }

    .card {
      box-shadow: 0 0 3px 1px #bdbdbd;
      height: 100%;
      width: 100%;
      display: flex;
      justify-content: center;
      align-items: center;
      border-radius: 8px;
      background: #ffffff;

      &:hover {
        cursor: pointer;
        transform: scale(1.05);
        box-shadow: 0 0 10px 3px #bdbdbd;
      }
    }
  }
}

h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

@media only screen and (max-width: 1000px) {
    .cardsField {
      display: flex!important;
  justify-content: center;
      flex-wrap: wrap;
    
   }
   .cardContainer{
      width:150px!important;
      height: 200px!important;
   }
}

</style>
