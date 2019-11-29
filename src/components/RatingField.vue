<template>
  <div class="ratingContainer">
    <h2>Лучшее время</h2>
    <ul>
      <li v-for="(rating,index) in ratings" :key="index" class="rating"><div >{{formatRating(rating).result}} мс.</div><div>{{formatRating(rating).date}}</div></li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "RatingField",
  props: {
    ratings: Array
  },
  methods:{
    formatRating(rating){
       let json = JSON.parse(rating)
       return json
    }
  }
};
</script>


<style lang="scss">
  .ratingContainer{
    min-height: 500px;
    max-height: 80vh;
    overflow: auto;
    width: 500px;
    border: 1px solid #e4e4e4;
    border-radius: 8px;
    margin: 5px;
     .rating{
    border-bottom: 1px solid #e4e4e4;
    display: flex;
    flex-direction: column;
  }
  }
 
  h2{
    text-align: center
  }
</style>