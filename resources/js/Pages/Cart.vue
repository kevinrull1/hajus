<template>

    <Head title="Cart" />
    <NavBar />
    <div class="py-12">
        <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
            <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                <div class="p-6 bg-white border-b border-gray-200">
                    <div class="column-1">
                        <div class="container mx-auto mt-10">
                            <div class="flex shadow-md my-10">
                                <div class="w-3/4 bg-white px-10 py-10">
                                    <div class="flex justify-between border-b pb-8">
                                        <h1 class="font-semibold text-2xl">Shopping Cart</h1>
                                        <h2 class="font-semibold text-2xl">{{Object.keys(props.data).length}} Items</h2>
                                    </div>
                                    <div class="flex mt-10 mb-5">
                                        <h3 class="font-semibold text-gray-600 text-xs uppercase w-2/5">Product Details
                                        </h3>
                                        <h3 class="font-semibold text-center text-gray-600 text-xs uppercase w-1/5 ">
                                            Quantity</h3>
                                        <h3 class="font-semibold text-center text-gray-600 text-xs uppercase w-1/5 ">
                                            Price</h3>
                                        <h3 class="font-semibold text-center text-gray-600 text-xs uppercase w-1/5 ">
                                            Total</h3>
                                    </div>

                                    <CartItem v-for="item in data" :key="item.id" :data="item" />

                                    <a href="#" class="flex font-semibold text-indigo-600 text-sm mt-10">

                                        <svg class="fill-current mr-2 text-indigo-600 w-4" viewBox="0 0 448 512">
                                            <path
                                                d="M134.059 296H436c6.627 0 12-5.373 12-12v-56c0-6.627-5.373-12-12-12H134.059v-46.059c0-21.382-25.851-32.09-40.971-16.971L7.029 239.029c-9.373 9.373-9.373 24.569 0 33.941l86.059 86.059c15.119 15.119 40.971 4.411 40.971-16.971V296z" />
                                        </svg>
                                        Continue Shopping
                                    </a>
                                </div>

                                <div id="summary" class="w-1/4 px-8 py-10">
                                    <h1 class="font-semibold text-2xl border-b pb-8">Order Summary</h1>
                                    <div class="flex justify-between mt-10 mb-5">
                                        <span class="font-semibold text-sm uppercase">Items {{Object.keys(props.data).length}}</span>
                                        <span class="font-semibold text-sm">{{sum(data)}}€</span>
                                    </div>
                                    <div class="border-t mt-8">
                                        <!-- <div class="flex font-semibold justify-between py-6 text-sm uppercase">
                                            <span>Total cost</span>
                                            <span>$600</span>
                                        </div> -->
                                        <button
                                            @click="redirect"
                                            class="bg-indigo-500 font-semibold hover:bg-indigo-600 py-3 text-sm text-white uppercase w-full">Checkout
                                            </button>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script setup>
import { Head, useForm } from "@inertiajs/inertia-vue3";
import {onMounted} from 'vue'
import { defineComponent } from '@vue/runtime-core';
const props = defineProps({ data: String, cart: String });



onMounted(() => {
    console.log(Object.keys(props.data).length)
    console.log(props.cart)
})

let stripe = Stripe("pk_test_51L7QREIT9JxsWLL8KiwrgZRiU627FiL6bb7XJiQOJG9WrGmDkf8Vk6tyfnMYEGPfnqUplgmgKL4zZeXGwN9d6ea900EPV3tVrS")

function sum(data) {
    var sum = 0;
  for( let el in data ) {
      console.log(el)
    if( data.hasOwnProperty( el ) ) {
      sum += parseFloat( data[el].price) * data[el].qty ;
    }
  }
  return sum;
}

function redirect() {
    const items = []
    props.cart.forEach(element => {
        items.push({price: element.default_price, quantity: parseInt(element.metadata.amount)})
    });
    console.log(items)
    stripe.redirectToCheckout({
        successUrl: "http://127.0.0.1:8000/success",
        cancelUrl: "http://127.0.0.1:8000/cart",
        lineItems: items,
        mode: "payment"
    })
}

</script>
