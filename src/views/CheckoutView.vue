<template>
    <body class="bg-[#261918] min-h-screen">
        <div class="w-full">
            <nav class="bg-[#541F1B] border-gray-200 flex items-center p-4">
                <button @click="toCart" type="button">
                    <img src="../assets/img/back-button.png" alt="Back button">
                </button>
                <span class="text-3xl text-white text-center flex-1 mr-[60px]">Checkout</span>
            </nav>
        </div> 
        <div class="mt-10">
            <div class="grid grid-cols-7">
                <div class="col-start-2 col-span-2 bg-[#BA442A] h-[280px] rounded-[10px] overflow-auto">
                    <div class="mt-[20px] ml-[20px]">
                        <h1 class="text-3xl text-white">Meu carrinho</h1>
                        <div v-for=" (object, index) in this.clientDict" :key="index" class="grid grid-cols-9 p-1 text-white">
                            <div class="col-start-1 col-span-5">
                                {{ object.nome }}
                            </div>
                            <div class="col-start-7">
                                {{ object.quantidade }}x
                            </div>
                            <div class="col-start-8">
                                R${{ object.preco }} 
                            </div>
                        </div>
                    </div>
                    <div class="px-3 py-3">
                        <hr><hr/>
                    </div>
                    <div class="grid grid-cols-13 mt-[7px] ml-[30px] text-white">
                        <div class="col-span-1">
                            <p>Subtotal</p>
                        </div>
                        <div class="col-start-7">
                            R${{ this.orderPrice }}
                        </div>
                        <div class="col-span-1">
                            <p>Taxa de entrega</p>
                        </div>
                        <div class="col-start-7">
                            R${{ this.fee }}
                        </div>
                        <div class="col-span-1">
                            <p>Desconto</p>
                        </div>
                        <div class="col-start-7">
                            R${{ this.discount }}
                        </div>
                        <div class="col-span-1 font-bold py-3 text-2xl">
                            <p>Total</p>
                        </div>
                        <div class="col-start-7 font-bold py-3 text-2xl">
                            R${{ this.finalPrice }}
                        </div>
                    </div>
                </div>
                <div class="col-start-5 col-span-2 bg-[#BA442A] h-[280px] rounded-[10px] overflow-auto">
                    <div class="mt-[20px] ml-[20px]">
                        <h1 class="text-3xl text-white">Pagamento</h1>
                        <p class="text-white  mt-[20px]">
                            Selecione a forma de pagamento:
                        </p>
                        <div class="grid grid-cols-7">
                            <div class="bg-[#A62C21] col-span-6 rounded-[10px] h-[30px] flex justify-between">
                                <div class="text-white mt-[4px] ml-[10px]">
                                    <p class="text-sm">Cartão de Crédito</p>
                                </div>
                                <button class="mr-[5px]"><svg class="w-5 h-5 text-white" aria-hidden="true" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path></svg></button>
                            </div>
                        </div>
                        <div class="grid grid-cols-7">
                            <div class="text-white col-span-3 mt-[6px] ml-[5px]">
                                <p class="text-sm">Cartão de Crédito</p>
                            </div>
                            <div class="text-white col-span-2 mt-[6px] ml-[5px]">
                                <p class="text-sm">Validade</p>
                            </div>
                            <div class="text-white col-span-1 mt-[6px] ml-[5px]">
                                <p class="text-sm">CVV</p>
                            </div>
                        </div>
                        <div class="grid grid-cols-7 gap-2">
                            <div class="bg-[#A62C21] col-span-3 rounded-[10px] h-[30px] mt-1">
                                <p class="text-white mt-[3px] text-center">**** **** **** 3333</p>
                            </div>
                            <div class="bg-[#A62C21] col-span-2 rounded-[10px] h-[30px] mt-1">
                                <p class="text-white mt-[3px] text-center">08/30</p>
                            </div>
                            <div class="bg-[#A62C21] col-span-1 rounded-[10px] h-[30px] mt-1">
                                <p class="text-white text-center mt-[3px]">251</p>
                            </div>
                        </div>
                        <div class="grid grid-cols-7">
                            <div class="text-white col-span-6 mt-[6px] ml-[5px]">
                                <p class="text-sm">Nome impresso no cartão</p>
                            </div>
                        </div>
                        <div class="grid grid-cols-7">
                            <div class="bg-[#A62C21] col-span-6 rounded-[10px] h-[30px] mt-[5px]">
                                <div class="text-white mt-[6px] ml-[10px]">
                                    <p class="text-sm">{{ this.clientName }}</p>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div id="address" class="col-start-2 col-span-5 bg-[#BA442A] h-[200px] rounded-[10px] mt-10">
                    <div class="mt-[20px] ml-[20px]">
                        <h1 class="text-3xl text-white">Endereço de entrega</h1>
                    </div>
                    <div class="mt-[20px] ml-[20px]">
                        <span class="flex text-white">
                            <h1 class="ml-[10px]">
                                Rua, Bairro, Número
                            </h1>
                            <h1 class="ml-auto mr-[398px]">
                                CEP
                            </h1>
                        </span>
                        <span class="flex">
                            <div class="bg-[#A62C21] w-[420px] h-[25px] rounded-[10px] text-white">
                                <div class="ml-[10px]">
                                    {{ `${this.addressDict.rua}, ${this.addressDict.bairro}, ${this.addressDict.numero}` }}
                                </div>
                            </div>
                            <div class="bg-[#A62C21] w-[420px] h-[25px] rounded-[10px] text-white ml-auto mr-[20px]">
                                <div class="ml-[10px]">
                                    {{ this.addressDict.cep }}
                                </div>
                            </div>
                        </span>
                        <h1 class="text-white ml-[10px]">
                            Complemento
                        </h1>
                        <div class="bg-[#A62C21] w-[420px] h-[25px] rounded-[10px] text-white">
                            <div class="ml-[10px]">
                                {{ this.addressDict.complemento }}
                            </div>
                        </div>
                    </div>
                </div>
                <div class="col-start-4 col-span-1 bg-[#BA442A] h-[50px] rounded-[10px] mt-10">
                    <div class="text-center mt-[10px]">
                        <button @click="OrderConfirmation" type="button">
                            <h1 class="text-2xl text-white">Confirmar</h1>
                        </button>
                    </div>
                </div>
            </div>
        <!-- Modal -->
        <div v-if="this.modalCard">
            <div class="modal fixed w-full h-full top-0 left-0 flex items-center justify-center">
                <div class="modal-overlay absolute w-full h-full bg-gray-900 opacity-50"></div>
                    <div class="modal-container bg-white w-[300px] mx-auto h-[200px] rounded-[20px] shadow-lg z-50 overflow-y-auto">
                        <div class="modal-content py-4 text-left px-6">
                            <div class="modal-body mt-2">
                                <!-- Conteúdo do modal -->
                                <div class="text-center font-bold">
                                    <p>Seu pedido de número #{{ this.ordersAm }} foi realizado com sucesso!</p>
                                </div>
                                <div class="text-center">
                                    <p class="mt-5">Deseja acompanhá-lo?</p>
                                </div>
                                <div class="grid grid-cols-2 mt-2">
                                    <div class="col-span-1">
                                        <button @click="OrderConfirmation" type="button" class="bg-[#9DBF69] hover:bg-[#A62C21] rounded-lg text-sm px-9 py-2.5">
                                            Voltar
                                        </button>
                                    </div>
                                    <div class="col-start-3">
                                        <button @click="toTracking" type="button" class="bg-[#9DBF69] hover:bg-green-500 rounded-lg text-sm px-4 py-2.5">
                                            <p class="text-black font-bold">Acompanhar</p>
                                        </button>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </body>
  </template>

<script>
    import axios from 'axios';
    import qs from 'qs';

    export default {
        name: 'checkoutView',
        data() {
            return {
                resName: '',
                clientDict: '',
                addressDict: '',
                clientName: '',
                estTime: '',
                todayDate: '', 
                currTime: '', 
                fee: 0,
                ordersAm: 0,
                finalPrice: 0,
                discount: 0,
                orderPrice: 0, 
                modalCard: false,
                cupons : [],
            }
        },
        methods: {
            async getAddress() {
                await axios.get('http://localhost:3000/address')
                .then(response => {
                    this.addressDict = response.data;
                })
                .catch(error => {
                    console.error(error);
                });
            }, 
            async OrdersAmount() {
                await axios.get('http://localhost:3000/orders')
                .then(response => {
                    this.ordersAm = response.data.amount;
                    this.ordersAm += 1;
                })
                .catch(error => {
                    console.error(error);
                });
            }, 
            async getName() {
                await axios.get('http://localhost:3000/client-name')
                .then(response => {
                    this.clientName = response.data.nome;
                })
                .catch(error => {
                    console.error(error);
                });
            },
            async storeClientOrder() {
                try {
                    await axios.post('http://localhost:3000/store-client-order', {
                        orderData: this.clientDict,
                        orderID: this.ordersAm,
                        orderDate: this.todayDate,
                        orderTime: this.currTime, 
                        orderFee: this.fee,
                        eTime: this.estTime,
                    });
                } catch (error) {
                    console.log(error);
                }
            },
            async storeResOrder() {
                try {
                    await axios.post('http://localhost:3000/store-res-order', {
                        orderData: this.clientDict,
                        orderID: this.ordersAm,
                        orderDate: this.todayDate,
                        orderTime: this.currTime, 
                        orderAddress: this.addressDict,
                    });
                } catch (error) {
                    console.log(error);
                }
            },
            async trackingField() {
                try {
                    await axios.post('http://localhost:3000/tracking-field', {
                        orderData: this.clientDict,
                        orderID: this.ordersAm,
                        orderDate: this.todayDate,
                        orderTime: this.currTime, 
                        orderAddress: this.addressDict,
                        orderPrice: this.finalPrice,
                        clientName: this.clientName,
                        orderFee: this.fee,
                        eTime: this.estTime,
                        resName: this.resName,
                        status: 'Pagamento'
                    });
                } catch (error) {
                    console.log(error);
                }
            },
            async clearCart() {
                try {
                    await axios.put('http://localhost:3000/clear-cart')
                }
                catch (error) {
                    console.log(error);
                }
            },
            async clearCouponsUsed() {
                try {
                    await axios.put('http://localhost:3000/clear-coupons-used')
                }
                catch (error) {
                    console.log(error);
                }
            },
            async getEstimatedTime() {
                try{
                    const response = await axios.post('http://localhost:3000/estimated-time', {
                        resName: this.resName,
                    });
                    const auxFee = response.data.taxa.split(" ");
                    const indexAux = auxFee[1]
                    this.fee = parseFloat(indexAux);
                    this.finalPrice = (this.orderPrice - this.discount + this.fee).toFixed(2);

                    this.estTime = response.data.tempo_estimado;
                }
                catch (error){
                    console.log(error);
                }

            }, 
            OrderConfirmation(){
                this.modalCard = !this.modalCard;
            }, 
            toTracking(){
                this.storeClientOrder();
                this.storeResOrder();
                this.trackingField();
                this.clearCart();
                this.clearCouponsUsed();
                
                this.$router.push ({
                    path: '/order-tracking',
                });
            },
            toCart(){
                this.$router.push({
                    path: '/shoppingcart'
                })
            },
        },

        mounted() {
            this.getAddress();
            this.OrdersAmount();
            this.getName();

            const objectString = this.$route.params.pratos;
            const object = qs.parse(objectString);
            this.clientDict = object;

            const subTotal = this.$route.params.subtotal;
            const discount = this.$route.params.desconto;
            const resName = this.$route.params.restaurante;
            this.resName = resName;
            this.orderPrice = parseFloat(subTotal);
            this.discount = parseFloat(discount);

            const aDate = new Date();
            const options = { timeZone: 'America/Sao_Paulo', hour12: false };
            const localTime = aDate.toLocaleString('pt-BR', options);
            const aux = localTime.split(" ");
            this.todayDate = aux[0];
            this.currTime = aux[1];

            this.getEstimatedTime();
        },
    }
</script>