<template>
    <body class="bg-[#261918] min-h-screen">
        <header class="bg-[#A62C21] w-full flex items-center justify-between py-4 mb-8">
            <a href="/clienthome">
                <img class="ml-4 pb-1 sm:w-32 sm:h-16 w-30 h-12 object-contain" src="../assets/img/logo-text.png" alt="logo">
            </a>
        </header> <!--header-->

        <div class="flex items-center w-full mb-3">
            <div>
                <a href="javascript:history.back()">
                    <img class="ml-4 sm:w-12 sm:h-12 w-8 h-8 object-contain" src="../assets/img/back-button.png" alt="back-button">
                </a>
            </div> <!--back-butoon-->
            
            <div class="ml-7">
                <h1 class="text-white font-bold sm:text-2xl text-xl"> Histórico </h1>
            </div> <!--Text-->
            
            
        </div>
        

        <div class="flex items-center justify-center mx-auto mb-3">
            <hr class="w-11/12 items-center">
        </div> <!--bar-->

        <div class="flex justify-end  text-white mb-2 w-full">
            <div v-if="this.$route.params.status === 'A caminho'" @click="isDelivered" class="bg-[#F26938] p-2 rounded-[20px] mr-5">
                <button>Confirmar entrega</button>
            </div>

            <div class="bg-[#F26938] p-2 rounded-[20px] mr-16">
                <button @click="goToTracking" class="">Acompanhar</button>
            </div>        
        </div>

        <!-- CARD -->
        <div :style="{ height: this.height + 'px' }" class="bg-[#A62C21] sm:w-11/12  w-[450px] h-[300px] grid grid-cols-4 rounded-[20px] mx-auto">
            <div class="flex col-span-1 text-white font-medium items-center">
                <div class="flex flex-col items-center justify-center ml-7 sm:mr-0 mr-4">
                    <img class="rounded-[20px] mb-3 sm:w-[200px] sm:h-[150px]" :src="this.$route.params.imgSrc">
                    <h2 class="text-white font-bold text-center">{{this.$route.params.name}}</h2>
                </div>
            </div>
            
            
            <div class="flex col-span-1 text-white">
                <div class="flex flex-col ml-7 sm:mr-0 mr-4 justify-center">
                    <h2 class="text-[#261918] font-bold">ID: #{{ this.$route.params.id }}</h2>
                    <h4 class="text-[#261918] mb-3">{{ this.$route.params.date }}, {{ this.$route.params.hour }}</h4>
  
                    <span class="text-white sm:text-[17px] text-[12px] mb-12">
                        {{ this.$route.params.items.join(', ') }}
                    </span>

                    <h2 class="text-gray-400">Entregue em:</h2>
                    <h3> {{this.address.rua}}, Nº {{this.address.numero}}, {{this.address.bairro}} - {{ this.address.complemento }}</h3> <!--PEgar esses dados depois-->
                </div>
            </div>

            <div class="flex col-span-2 text-white mr-2 ml-10">
                <div class="grid grid-cols-2 mt-20">
                    <div class="col-span-1 mt-5">
                        <div v-for="(dish, index) in this.$route.params.items" :key="index">
                            <span class="text-white sm:text-[17px] text-[12px] mb-1">{{ dish }}</span>
                        </div>
                    </div>
                    
                    <div class="relative col-span-1 mt-5 text-right">
                        <div v-for="(price, index) in this.$route.params.prices" :key="index">
                            <span class="text-white sm:text-[17px] text-[12px] mb-1">R$ {{ price }}</span>
                        </div>
                        
                        <hr class="text-center mr-3 mb-2 w-full">
                        <h1 class=" ml-auto mr-3 text-3xl font-bold mb-5">R$ {{ this.$route.params.total_price }} <br>({{ this.$route.params.form_pay }})</h1> 
                        <button @click="goToShoppingCart" class="bg-[#F26938] rounded-[25px] text-2xl font-bold p-3 absolute bottom-7 right-0">Pedir novamente</button>
                    </div>
                </div>
            </div>
        </div>
        
        <footer class="w-full bg-[#261918] h-20">

        </footer>
    </body>
</template>


<script>
import axios from 'axios';

    export default {
        data() {
            return {
                height: 450,
                address: {},
            }
        },
        methods: {
            async goToShoppingCart() {
                const data = JSON.parse(this.$route.params.dishes);
                await axios.post('http://localhost:3000/add-to-cart', data)
                .then(() => {
                    this.$router.push('/shoppingcart');
                }).catch(error => {
                    console.log(error.message);
                });
            },
            goToTracking() {
                var orderData = JSON.parse(this.$route.params.info_dishes);
                var id = this.$route.params.id;
                var date = this.$route.params.date;
                var hour = this.$route.params.hour;
                var total = this.$route.params.total_price;
                var restaurant_name = this.$route.params.name;
                var client_name = this.$route.params.client_name;
                var estimated_time = this.$route.params.estimated_time;
                var status = this.$route.params.status;
                axios.post('http://localhost:3000/tracking-field', {
                    orderData: orderData,
                    orderID: id,
                    orderDate: date,
                    orderTime: hour, 
                    orderAddress: this.address,
                    orderPrice: total,
                    clientName: client_name,
                    orderFee: "5",
                    eTime: estimated_time,
                    resName: restaurant_name,
                    status: status
                }).then(() => {
                    this.$router.push('/order-tracking')
                }).catch(error => {
                    console.error(error);
                });
            },
            isDelivered() {
                axios.post('http://localhost:3000/client-confirmation', { name: this.$route.params.client_name, 
                                                                  id: this.$route.params.id })
                .then(() => {
                }).catch(error => {
                    console.error(error);
                });

                axios.post('http://localhost:3000/restaurant-confirmation', { restaurant_name: this.$route.params.name, 
                                                                  id: this.$route.params.id, client_name: this.$route.params.client_name })
                .then(() => {
                }).catch(error => {
                    console.error(error);
                });
            }
        },
        mounted() {
            if (this.$route.params.items.length > 10) this.height = 600;
            
            axios.get('http://localhost:3000/address')
            .then(res => {
                this.address = res.data;
            }).catch(error => {
                console.log(error.message);
            });
        }
    }
</script>