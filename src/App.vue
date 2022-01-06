<template>
  <div id="app">
    <header class="header">
      <div class="container text-center">
        <img src="./img/logo/logo.svg" width="92" alt="Суши и пицца">
        <div class="display-4">Доставка роллов</div>
        <p class="lead">Оперативно и вкусно</p>
      </div>
    </header>

    <div class="container mb-5">
      <div class="row">
        <div class="col-md-8">
          <div class="row">

            <!-- Ролл -->
              <div v-for="roll in rolls" :key="roll.name" class="col-md-6">
                <div class="card mb-4">
                  <img src="./img/roll/philadelphia.jpg" class="product-img" alt="roll">
                  <div class="card-body text-center">
                    <h4 class="item-title">{{roll.name}}</h4>
                    <p><small class="text-muted">6 шт.</small></p>
  
                    <div class="details-wrapper">
                      <div class="items">
                        <div @click="roll.current > 1 ? roll.current-- : ''" class="items__control">-</div>
                        <div class="items__current">{{roll.current}}</div>
                        <div @click="roll.current++" class="items__control">+</div>
                      </div>
  
                      <div class="price">
                        <div class="price__weight">{{roll.grams}}</div>
                        <div class="price__currency">{{roll.price}} ₽</div>
                      </div>
                    </div>

                    <button
                      @click="addToCart(roll.name, roll.price, roll.grams, roll.current), roll.current = 1"
                      type="button" 
                      class="btn btn-block btn-outline-warning">
                     + в корзину
                    </button>
                    
                  </div>
                </div>
              </div>
            <!-- // Ролл -->

          </div>
        </div>
        <div class="col-md-4">

          <!-- Корзина Пустая -->
          <div v-if="orders.length == 0" class="card mb-4">
            <div class="card-body">
              <h5 class="card-title">Ваш заказ</h5>
              <div class="alert alert-secondary" role="alert">
                Корзина пуста
              </div>
            </div>
          </div>
          <!-- // Корзина Пустая -->

          <!-- Корзина Полная -->
          <div v-else class="card">
            <div class="card-body">
              <h5 class="card-title">Ваш заказ</h5>
              
              <!-- cart-wrapper -->
              <div class="cart-wrapper">

                <!-- cart-item  -->
                <div v-for="(order, index) in orders" :key="order.name" class="cart-item">
                  <div class="cart-item__top">
                    <div class="cart-item__img">
                      <img src="./img/roll/philadelphia.jpg" alt="">
                    </div>
                    <div class="cart-item__desc">
                      <div class="cart-item__title">{{order.name}}</div>
                      <div class="cart-item__weight">6 шт. / {{order.grams}}</div>

                      <!-- cart-item__details -->
                      <div class="cart-item__details">

                        <div class="items items--small">
                          <div @click="order.current > 1 ? order.current-- : deletingACard(index)" class="items__control">-</div>
                          <div class="items__current">{{order.current}}</div>
                          <div @click="order.current++" class="items__control">+</div>
                        </div>

                        <div class="price">
                          <div class="price__currency">{{order.price}} ₽</div>
                        </div>

                      </div>
                      <!-- // cart-item__details -->

                    </div>
                  </div>
                </div>
                <!-- // cart-item  -->

                <div class="cart-total">
                  <p><span class="h5">Доставка:</span> <span class="delivery-cost free">{{freeShipping}}</span> </p>
                  <p><span class="h5">Итого:</span> <span class="total-price">{{costCalculation}}</span> <span class="rouble">₽</span></p>
                </div>

              </div>
              <!-- // cart-wrapper -->

            </div>

            <div class="card-body border-top">
              <h5 class="card-title">Оформить заказ</h5>

              <form>
                <div class="form-group">
                  <input type="text" class="form-control" placeholder="Ваш номер телефона">
                </div>
                <button type="submit" class="btn btn-primary">Заказать</button>
              </form>


            </div>
          </div>
          <!-- // Корзина Полная -->

        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',

  data() {
    return {
      rolls: [
        {name: 'Филадельфия хит ролл', price: '300', grams: '180г.', current: 1},
        {name: 'Калифорния темпура', price: '250', grams: '205г.', current: 1},
        {name: 'Запеченый ролл «Калифорния»', price: '183', grams: '180г.', current: 1},
        {name: 'Филадельфия', price: '300', grams: '420г.', current: 1},
        {name: 'Запеченый ролл Филадельфия', price: '720', grams: '120г.', current: 1},
      ],
      orders: [],
    }
  }, 

  computed: {
    costCalculation() {
      return this.orders.reduce((a, b) => a + (b.price * b.current), 0)
    },

    freeShipping() {
      return this.costCalculation < 1000 ? '300 ₽' : 'Бесплатная'
    }
  },

  methods: {
    addToCart(name, price, grams, current) {
      const order = { name, price, grams, current }
      let similarityСheck = 0

      this.orders.forEach(el => {
        if (el.name == order.name) {
          el.current = el.current + order.current
          similarityСheck++
          return
        }
      })

      if (similarityСheck == 0) {
        this.orders.push(order)
        similarityСheck = 0
      }
    },

    deletingACard(index) {
      this.orders = this.orders.filter((el, ind) => index !== ind)
    },
  },
}
</script>

<style src="./main.css"></style>
