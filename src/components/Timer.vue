<template>
  <div class="timer">
      <span  id="time">{{time}}</span>
  </div>
</template>

<script>
export default {
  name: "Timer",
  props: {
    time: String
  }
};
</script>


<style lang="scss">
  .timer{
  
    width: 100%;
    text-align: center;
  }
  #time{
    color: #2196f3;
    padding: 15px;
    text-align: center;
    font-size: 170%;
    font-weight: 600;
  }
</style>