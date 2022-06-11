
<template>

    <section class='mt-5 border p-5 shadow'>

            
        <h3>Shopping cart</h3>
        <i @click="closeCart" class="far fa-window-close fa-3x" title="Close"></i>

        <table class="table">
    
            <thead>
                <tr>
                    <th scope="col">#</th>
                    <th scope="col">Image</th>
                    <th scope="col">Title</th>
                    <th scope="col">Quantity</th>
                    <th scope="col">Price</th>
                    <th scope="col">Cost</th>
                </tr>
            </thead>
            

            <tr v-for="(item, id) in goodsInCart" :key="item.id">
                <th scope="row">{{id + 1}}</th>

                <td>
                    <img :src="item.product.image" :alt="item.product.title">
                </td>

                <td class="product-title">{{item.product.title}}</td>

                <td class="counter">
                    <div  class='d-flex justify-content-around align-items-center'>
                        <button class="btn btn-outline-primary" @click="item.quantity++">
                            <i class="fas fa-plus"></i>
                        </button>

                        {{item.quantity}} 

                        <button class="btn btn-outline-primary"
                            :disabled="item.quantity < 2"    @click="item.quantity--">
                            <i class="fas fa-minus"></i>
                        </button>
                    </div>
                </td>

                <td>{{item.product.price}}$</td>

                <td>{{(item.product.price * item.quantity).toFixed(2)}}$</td>

                <td>
                    <i class="far fa-trash-alt fa-2x" title='Remove Item' 
                        @click="deleteProdInCart(id)"></i>       
                </td>

            </tr>
        
        </table>

        <h3>Total: {{totalCost}}$</h3>

         <button class='btn btn-success mt-3' disabled>Complete Order</button>

    </section>

    
</template>

<script setup>

    defineProps({
        'goodsInCart': Array,
        'closeCart': Function,
        'deleteProdInCart': Function,
        'totalCost': String
    });


</script>

<style scoped>

section{
    width:85vw;
    margin:auto;
    position: relative;    
}

table{
    text-align: center;
}

section .fa-window-close{
    position: absolute;
    right: 15px;
    top: 10px;
    color: gray;
    
}
 img{
    width: 6vw;
    height: 6vw;
    object-fit: contain;
 }

 .table>:not(:first-child){
     border-top:none;
     border-bottom: 1px solid rgb(170, 170, 170);
 }
 .table > thead {
     border-bottom:2px solid black;
 }

.far{
    cursor: pointer;
}

.counter{
    width: 10vw;
}


.product-title{
    width: 30vw;
}

</style>