<template>
    
    <div class="bg-[#261918] min-h-screen ">
        <NavBar /> <!-- NavBar dos componentes-->

        <div class="flex items-center w-full mb-3 pt-32">
            <button @click="goToHome()"> <!--  Botão de voltar -->
                <img class="ml-4 sm:w-12 sm:h-12 w-8 h-8 object-contain" src="../assets/img/back-button.png" alt="back-button">
            </button> 
            
            <!-- Título da página-->
            <div class="ml-7">
                <h1 class="text-white font-bold sm:text-2xl text-xl"> Restaurantes </h1>
            </div> 
        </div> 
        <button  @click='goToShoppingCart()' type="submit" class=" hover:bg-red-800 focus:ring-4 absolute right-32 top-32 focus:outline-none bg-[#A62C21]  focus:ring-red-300 font-medium rounded-lg text-l inline-block px-6 py-1 mr-10 mb-2 ">
          <img src="@\assets\img\carrinho.png" alt="Carrinho de Compras" class=" h-10 w-11" >
        </button>

        <!-- Linha -->
        <div class="flex items-center justify-center mx-auto mb-12">
            <hr class="w-11/12 items-center">
        </div>
        

      <div class="mx-auto max-w-8xl justify-center px-6 md:flex md:space-x-7 xl:px-0">
        <div class="rounded-lg md:w-9/10">
          <div v-for="restaurant in restaurants" :key="restaurant.id">
            <div class="mb-6 rounded-lg bg-[#A62C21] p-6 shadow-md">
              <div class="justify-between mb-6 rounded-lg p-6 sm:flex sm:justify-start">
              <img :src="restaurant.logo" :alt="restaurant.nome_loja" class="w-full rounded-lg sm:w-40 px-3" />
              <div class="sm:ml-4 sm:flex md:w-3/4 sm:justify-between">
                <div class="mt-5 sm:mt-0">
                  <h2 class="text-white text-xl font-bold ">{{restaurant.nome_loja}}</h2>
                  <p class="mt-1 text-base text-white">{{restaurant.especialidade}}</p>
                  <p class="mt-1 text-base text-white">{{restaurant.cidade}}</p>
                <!-- </div>
                <div class="mt-4 flex justify-between sm:space-y-6 sm:mt-0 sm:block sm:space-x-5">
                  </div>
                  <div class="mt-4 flex items-center space-x-2">
                    <p class="text-white flex items-center">{{(parseFloat(prato.preco.replace(',','.'))*prato.quantidade).toFixed(2).replace('.',',')}} <span class="ml-1">R$</span></p>
                    
                  </div> 
                </div> -->
                </div>
              </div>

                
            </div> <!-- Aqui termina a caixa do restaurante -->
            <div class=" bg-[#85231A] flex flex-wrap items-center space-x-12 justify-center py-4 bg-cover inset-x-0  rounded-lg p-3" > <!-- Flexbox do cardápio de pratos-->
                  <div v-for="dish in restaurant.pratos" :key="dish.id">
                  <button @click="addToCart(dish)" class=" w-56 h-56 bg-[#541F1B] border-gray-200 rounded-lg shadow transform hover:scale-105 transition duration-300 ">
                    <a href="#">
                      <img :src="dish.url" :alt="dish.nome" class= ' object-scale-down h-28 w-full  mx-auto block'>
                    </a>
                    <div class="px-5 pb-5">
                        <a href="#">
                            <h5 class="text-base font-semibold tracking-tight text-white ">{{dish.nome}}</h5>
                        </a>
                        <div class="text-xs text-white">{{dish.descricao}} </div>
                        <div class="flex items-center justify-between p-1">
                            <div class="text-lg font-bold text-white">{{dish.preco}} </div>
                            <a href="#" class="text-white bg-blue-700   font-medium rounded-lg text-sm px-1 py-1.5 text-center ">Add to cart</a>
                        </div>
                      </div>
                    </button>
                  </div>
                </div>
            </div>
            
            
          </div>
        </div>
      </div>
      <div v-if="this.notCompatible">
              <div class="modal fixed w-full h-full top-0 left-0 flex items-center justify-center">
                  <div class="modal-overlay absolute w-full h-full bg-gray-900 opacity-50"></div>
                      <div class="modal-container bg-white w-[300px] mx-auto h-[175px] rounded-[20px] shadow-lg z-50 overflow-y-auto">
                          <div class="modal-content py-4 text-left px-6">
                              <div class="modal-body mt-2">
                                  <!-- Conteúdo do modal aqui -->
                                  <div class="text-center font-bold">
                                      <p>Só é possível acrescentar ao carrinho itens de um mesmo restaurante!</p>
                                  </div>
                                  <div class="flex justify-center">
                                    <button @click="fecharModal" class="bg-[#9DBF69] hover:bg-[#A62C21] rounded-lg text-sm px-9 py-2.5 mt-7" >
                                      Voltar
                                    </button>
                                  </div>
                              </div>
                          </div>
                      </div>
                  </div>
      </div>
  </div> 
</template>

<script>

    import NavBar from '@/components/NavBar.vue'
    import axios from 'axios';
    
    export default {
        name: 'RestaurantsList',
        components: {
        NavBar,
        },
        data() {
            return {
                restaurants: [],
                shopping_cart: [],
                found: false,
                notCompatible: false
            }
        },
        async mounted() {
        await axios.get('http://localhost:3000/restaurants-list')
            .then(response => {
                this.restaurants = [...response.data];
               
            })
            .catch(error=> {
                console.error(error);
            });
            console.log(this.restaurants);
        await axios.get('http://localhost:3000/clienthome1')
            .then(response => {
              this.shopping_cart = [...response.data];
            })
            .catch(error=> {
              console.error(error);
            });
          console.log(this.shopping_cart)
        },
        methods: {
          goToHome() {
          this.$router.push('/clienthome');
          },
          goToShoppingCart() {
          this.$router.push('/shoppingcart');
          },
          update(){
          console.log('updated')
          axios.get('http://localhost:3000/clienthome1')
          .then(response => {
            this.shopping_cart = [...response.data];
          })
          .catch(error=> {
            console.error(error);
          });
        },
        addToCart(dish) {
          this.update();
          console.log('addToCart')
          console.log(this.shopping_cart)
          if (this.shopping_cart.length == 0){
            console.log('length == 0')
            axios.post('http://localhost:3000/clienthome',  {nome: dish.nome, descricao: dish.descricao, preco: dish.preco, url: dish.url, quantidade: 1, restaurante: dish.restaurante}  )
            .then(response => {
              console.log(response.data.message);
              location.reload();
            })
            .catch(error => {
              console.error(error);
            });
        } else {
          console.log("length !== 0")
          let i = 0;
          let index = 0;
          this.shopping_cart.forEach((doc) => {
            console.log(doc)
            console.log(dish.restaurante)
            if(doc.restaurante !== dish.restaurante){
              this.notCompatible = true;
            }
            console.log(doc.nome)
            if(doc.nome == dish.nome){
              console.log('Está no carrinho')
              index = i;
              this.found = true; 
            }
            i = i + 1;
          });
          if (!this.notCompatible){
            console.log('ENTROU NO IF')
            if(!this.found){
              console.log('not found');
              axios.post('http://localhost:3000/clienthome',  {nome: dish.nome, descricao: dish.descricao, preco: dish.preco, url: dish.url, quantidade: 1, restaurante: dish.restaurante}  )
              .then(response => {
                console.log(response.data.message);
                location.reload();
              })
              .catch(error => {
                console.error(error);
              });
            }
            if(this.found){
              console.log('found');
              console.log(index)
              axios.put('http://localhost:3000/clienthome2',  {nome: dish.nome, index: index}  )
              .then(response => {
                console.log(response.data.message);
                location.reload();
              })
              .catch(error => {
                console.error(error);
              });
            }
          }
        }
        this.found = false;
        },
        fecharModal(){
          this.notCompatible = false;
        }
          
      }
    }
</script>