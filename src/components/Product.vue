<template>
    <div>
        <my-header></my-header>
        <h1> This is the id {{ $route.params.id }}</h1>
        <div class="row">
            <div class="col-md-5 col-md-offset-0">
                <figure>
                    <img class="product" v-bind:src="product.image">
                </figure>
            </div>
            <div class="col-md-6 col-md-offset-0 description">
                <h1>{{ product.title }}</h1>
                <p v-html="product.description"></p>
                <p class="price">
                    {{ product.price | formatPrice  }}
                </p>
                <button @click="edit">Edit Product</button>
                <router-view></router-view>
            </div>
        </div>
    </div>
</template>

<script>
import MyHeader from './Header.vue'
export default {
    components: { MyHeader },
    data() {
        return {
            product: ''
        }
    },
    methods: {
        edit() {
            this.$router.push({ name: 'Edit' })
        }
    },
    filters: {
        formatPrice(price) {
            if (!parseInt(price)) {
                return '';
            }
            if (price > 99999) {
                var priceString = (price / 100).toFixed(2);
                var priceArray = priceString.split('').reverse();
                var index = 3;
                while (priceArray.length > index + 3) {
                    priceArray.splice(index + 3, 0, ',');
                    index += 4;
                }
                return '$' + priceArray.reverse().join('');
            } else {
                return '$' + (price / 100).toFixed(2);
            }
        }
    },
    created: function(){
        axios.get('/static/products.json')
            .then((response) => {
            this.product = response.data.products.filter(data => data.id == this.$route.params.id)[0]
            console.log(this.product)
            this.product.image = '/' + this.product.image;
        });
    }

}
</script>