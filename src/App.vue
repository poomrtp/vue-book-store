<template>
  <div id="app">
    <nav class="navbar navbar-expand-lg navbar-light bg-light fixed-top">
      <div class="container-fluid">
        <a class="navbar-brand" href="#">Se Education</a>
        <button
          class="navbar-toggler"
          type="button"
          data-bs-toggle="collapse"
          data-bs-target="#navbarNavAltMarkup"
          aria-controls="navbarNavAltMarkup"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
          <div class="navbar-nav">
            <a class="nav-link" aria-current="page" @click="gotoHome()">Home</a>
            <a class="nav-link" href="#">Category</a>
          </div>
        </div>
        <div class="navbar-nav right">
          <a class="nav-link" @click="gotoCart()"
            ><b-icon icon="cart4"></b-icon
          ></a>
          <a class="nav-link">Login</a>
        </div>
      </div>
    </nav>
    <div class="container">
      <div class="row">
        <div class="col-md-2" v-for="product in products" :key="product.id">
          <div class="card h-80">
            <img
              v-bind:src="product.img"
              alt=""
              class="card-img-top w-100"
              width="100%"
            />
            <div class="card-body">
              <p>{{ product.name }}</p>
              <p>price : {{ product.price }}</p>
            </div>
            <div class="card-footer">
              <button v-on:click="addCart(product)">add to cart</button>
            </div>
          </div>
        </div>
      </div>
      <h1><b-icon icon="cart4"></b-icon> cart</h1>
      <table class="table table-striped" v-if="this.cart.length > 0">
        <thead class="table-light">
          <tr>
            <th scope="col">#</th>
            <th scope="col">Pic</th>
            <th scope="col" width="30%">Name</th>
            <th scope="col" width="10%">Price</th>
            <th scope="col" width="20%">Total</th>
            <th scope="col" width="5%"></th>
            <th scope="col" width="15%">Total</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(product, index) in cart" :key="index">
            <th scope="row">{{ index + 1 }}</th>
            <td><img :src="product.img" alt="" width="60px" /></td>
            <td>{{ product.name }}</td>
            <td>{{ product.price }}</td>
            <td>
              <b-icon
                icon="dash-square"
                class="icon"
                @click="minusQty(product)"
              ></b-icon>
              <input
                type="number"
                name=""
                id=""
                min="1"
                max="10"
                v-model="product.quantity"
                v-on:keyup.enter="changeQty(product)"
              />
              <b-icon
                icon="plus-square"
                class="icon"
                @click="addQty(product)"
              ></b-icon>
            </td>
            <td>
              <b-icon
                icon="trash"
                class="icon"
                @click="removeProduct(product)"
              ></b-icon>
            </td>

            <td align="right">{{ product.total }}</td>
          </tr>
        </tbody>
        <tfoot>
          <tr>
            <th></th>
            <td colspan="4" align="right">Amount</td>
            <td colspan="2" align="right">{{ amountPrice() }}</td>
          </tr>
          <tr>
            <th></th>
            <td colspan="4" align="right">Discount {{ discountPercent }} %</td>
            <td colspan="2" align="right">{{ discount() }}</td>
          </tr>
          <tr>
            <th></th>
            <td colspan="4" align="right">Net price</td>
            <td colspan="2" align="right">{{ netPrice() }}</td>
          </tr>
        </tfoot>
      </table>
      <h5 v-if="this.cart.length == 0">No product list</h5>
      <button type="button" class="btn btn-warning" v-if="this.cart.length > 0">
        purchase
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      products: [
        {
          id: 1,
          name: "My Hero Academia vol.1",
          price: 100,
          img: "https://inwfile.com/s-du/_max_images/600/600/0x/2z/jc.jpg",
          quantity: 0,
        },
        {
          id: 2,
          name: "My Hero Academia vol.2",
          price: 100,
          img: "https://inwfile.com/s-du/_max_images/600/600/fa/lq/lq.jpg",
          quantity: 0,
        },
        {
          id: 3,
          name: "My Hero Academia vol.3",
          price: 100,
          img: "https://inwfile.com/s-du/_max_images/600/600/kk/m4/v6.jpg",
          quantity: 0,
        },
        {
          id: 4,
          name: "My Hero Academia vol.4",
          price: 100,
          img: "https://inwfile.com/s-du/_max_images/600/600/ba/57/jg.jpg",
          quantity: 0,
        },
        {
          id: 5,
          name: "My Hero Academia vol.5",
          price: 100,
          img: "https://inwfile.com/s-du/_max_images/600/600/w6/q7/g4.jpg",
          quantity: 0,
        },
        {
          id: 6,
          name: "My Hero Academia vol.6",
          price: 100,
          img: "https://inwfile.com/s-du/_max_images/600/600/3s/z9/5j.jpg",
          quantity: 0,
        },
        {
          id: 7,
          name: "My Hero Academia vol.7",
          price: 100,
          img: "https://inwfile.com/s-du/_max_images/600/600/o8/ag/1s.jpg",
          quantity: 0,
        },
      ],
      cart: [],
      discountPercent: 0,
    };
  },
  components: {},
  methods: {
    addCart(productInCart) {
      let findIndex = false;
      this.cart.forEach((cart) => {
        if (productInCart.id === cart.id) {
          findIndex = true;
        }
      });
      if (findIndex) {
        this.cart.forEach((cart) => {
          if (productInCart.id === cart.id) {
            cart.quantity++;
            cart.total = cart.quantity * cart.price;
          }
        });
      } else {
        this.cart.push({
          id: productInCart.id,
          name: productInCart.name,
          price: productInCart.price,
          img: productInCart.img,
          quantity: 1,
          total: productInCart.price * 1,
        });
      }
    },
    changeQty(product) {
      if (Number.isNaN(product.quantity)) {
        product.quantity = 1;
      }
      if (product.quantity <= 0) {
        product.quantity = 1;
      }
      product.total = product.quantity * product.price;
    },
    addQty(product) {
      product.quantity++;
      product.total = product.quantity * product.price;
    },
    minusQty(product) {
      if (product.quantity > 1) {
        product.quantity--;
        product.total = product.quantity * product.price;
      }
    },
    discount() {
      let discount = 0;
      return discount;
    },
    amountPrice() {
      var amount = 0;
      this.cart.forEach((product) => {
        amount += product.total;
      });
      return amount;
    },
    netPrice() {
      return this.amountPrice() - this.discount();
    },
    removeProduct(product) {
      var index = this.cart.indexOf(product);
      this.cart.splice(index, 1);
    },
    gotoCart() {
      window.scrollTo(0, document.body.scrollHeight);
    },
    gotoHome() {
      window.scrollTo(0, 0);
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 0px;
}
.container {
  margin-top: 100px;
  margin-bottom: 100px;
}
.icon {
  margin-left: 5px;
  margin-right: 5px;
  cursor: pointer;
}
</style>
