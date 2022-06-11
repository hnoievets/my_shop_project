
<template>

  <div class="card shadow">


    <img :src="oneGood.image" class="card-img-top" :alt="oneGood.item">

    <div class="card-body">
      
      <h5 class="card-title">{{oneGood.title}}</h5>

      <p class="card-text">{{oneGood.description.substr(0, 100)}}...</p>

      <div class="flex-grow-1"></div>
      <h4 class=' text-primary mb-3'>{{oneGood.price}}$</h4>
        
      <button
      @click="addToCart(oneGood)" class="btn btn-primary" v-if="!showCard">
        <i class="fas fa-shopping-cart"></i> Add to Cart
      </button>

      <button
      class="btn btn-outline-primary"  v-else disabled>
          <i class="fas fa-shopping-cart"></i> Already in Cart
      </button>

    </div>

  </div>
    
</template>

<script setup>
    
  defineProps({
      'oneGood': Object,
      'showCard': Boolean,
      'addToCart' : Function
  });

</script>

<style scoped>

  .card{
    width: 21vw;
    padding: 1vw 1vw ;
    margin: 0.5vw;
  }

  .card img{
    height: 15vw;
    object-fit: contain ;
  }

  div.card-body{
    padding: 0;
    display: flex;
    flex-direction: column;
  }

  button{
    width: 13vw;
    margin: auto;
  }


</style>