
<template>
  
  <h1 class="m-3">MyShop</h1>

  <div v-if="state.showElement">



    <div class="d-flex justify-content-start">
      
      <div class="form-check mx-5">
        <input class="form-check-input" v-model='state.sort' value="up" type="radio" name="sortPrice" id="sortPriceUp">
        <label class="form-check-label" for="sortPriceUp">
          Сортировать по возрастанию цены
        </label>
      </div>

      <div class="form-check mx-5">
        <input class="form-check-input" v-model='state.sort' value="down" type="radio" name="sortPrice" id="sortPriceDown">
        <label class="form-check-label" for="sortPriceDown">
          Сортировать по уменьшению цены
        </label>
      </div>

      <div class="col-lg-4">
        <input v-model = 'state.search' type="text" class="w-100 mb-1" placeholder="Поиск" id="searchInput">
      </div>

    </div>

  </div>
    

  <div @click="state.showElement = !state.showElement" class='cart-qt-cost'>

    <i class="fas fa-shopping-cart fa-2x"></i>

    <div>({{state.cart.length}}) {{itemsFullCost}}$</div>

  </div>

  <div class="cart" v-if="!state.showElement">

    <cart-page
      :goods-in-cart='state.cart'
      :close-cart='closeCart'
      :delete-prod-in-cart='deleteProdInCart'
      :total-cost='itemsFullCost'/>

  </div>

  <div v-show="state.showElement">

    <div class="container">
      <div class='row justify-content-center'> 
        <one-card v-for="item in renderGoods" 
          :one-good='item'
          :add-to-cart='addToCart'
          :show-card='IsInCard(item)'
          :key= 'item.id' />
      </div>
    </div>

  </div>

</template>

<script setup>

  import OneCard from './components/OneCard.vue'
  import CartPage from './components/CartPage.vue'
  import {reactive, onMounted, computed} from 'vue';


  let state = reactive({
    goods:[],
    cart: [],
    showElement: true,
    sort:'',
    search:''
  });

  function addToCart(product){

    state.cart.push({
      product: product,
      quantity: 1
    });
  }

  const itemsFullCost = computed(()=>state.cart.reduce((acc,i)=> acc + (i.product.price * i.quantity), 0).toFixed(2));

  const MaxOfPrice = computed(()=>state.goods.reduce((acc,i)=> acc + (i.product.price * i.quantity), 0).toFixed(2));

  function IsInCard(product){
    return state.cart.some(i => i.product.id === product.id);
  }

  function closeCart(){
    state.showElement = true;
  }

  function deleteProdInCart(id){
    state.cart.splice(id,1);
  }

  function forSearchFilter(item){
    let suitThing = state.search.toLowerCase().trim();

    if(item.title.toLowerCase().trim().includes(suitThing)){
        return true;
    }

    if(item.description.toLowerCase().trim().includes(suitThing)){
        return true;
    }
  }

  const renderGoods = computed(() => {
    let sortGoods = state.goods.filter(forSearchFilter);

    if(state.sort == 'up'){
      sortGoods = sortGoods.sort((a,b) => a.price - b.price);
    }

    if(state.sort == 'down'){
      sortGoods = sortGoods.sort((a,b) => b.price - a.price);
    }

    return sortGoods;

  } );

  onMounted(async () => {
    const URL = 'https://fakestoreapi.com/products?limit=50';

    let data = await fetch(URL);
    data = await data.json();

    state.goods = data;
  });

</script>


<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif; 
    color: #2c3e50;
  }

  .cart-qt-cost{
    position: absolute;
    right: 1vw;
    top: 1vw;
    cursor: pointer;
  }

</style>
