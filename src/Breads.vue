<template>
  <div>
    <div v-for="bread in breads" :key="bread.id">
      <h2 v-on:click="setSelectedBread(bread)">{{ bread.name }}</h2>
      <p>{{ bread.price }}</p>
      <p>{{ bread.recipe }}</p>
      <button v-if="selectedBread === bread" v-on:click="editBread(bread)">Edytuj</button>
      <button v-if="selectedBread === bread" v-on:click="deleteBread(bread)">Usun</button>
      <button v-if="selectedBread === bread" v-on:click="showForm = !showForm">Dodaj opinie</button>
      <div v-if="showForm">
        <form @submit.prevent="addReview(bread)">
          <input v-model="text" placeholder="text" type="text" />
          <input v-model="score" placeholder="score" type="text" />
          <input v-model="author" placeholder="author" type="text" />
          <button type="submit">Dodaj opinie</button>
        </form>
      </div>
      <div v-if="selectedBread === bread">
        <h3>Opinie:</h3>
        <div v-for="review in bread.reviews" :key="review.id">
          <p>Recenzent: {{ review.Author }}</p>
          <p>{{ review.text }}</p>
          <p>Punkty: {{ review.score }}</p>
        </div>
      </div>
    </div>

    <form @submit.prevent="addBread">
      <input v-model="name" placeholder="name" type="text" />
      <input v-model="price" placeholder="price" type="text" />
      <input v-model="recipe" placeholder="recipe" type="text" />
      <button type="submit">Dodaj chlebek</button>
      
    </form>
    <form v-if="showForm" @submit.prevent="addReview(bread)">
          <input v-model="text" placeholder="text" type="text" />
          <input v-model="score" placeholder="score" type="text" />
          <input v-model="author" placeholder="author" type="text" />
          <button type="submit">Add Review</button>
    </form>
  </div>
</template>

<script>
import axios from 'axios'
import Reviews from './reviews.vue'
export default {
  name: 'Breads',
  data() {
    return {
    breads: [],
    name: '',
    price: '',
    recipe: '',
    imagePath: 'path',
    selectedBread: null,
    editingBread: null,
    editingIndex: -1,
    showForm: false,
    text: '',
    score: '',
    author: '',
    }
  },
  created() {
    axios.get('http://localhost:3000/breads')
    .then(response => {
      this.breads = response.data;
    })
    axios.get('http://localhost:3000/reviews')
    .then(response => {
      this.reviews = response.data;
    })
  },
  methods: {
    addBread() {
      axios.post('http://localhost:3000/breads', {
      name: this.name,
      price: this.price,
      recipe: this.recipe,
      imagePath: 'path'
      })
      .then(response => {
      this.breads.push(response.data);
      })
      },
    setSelectedBread(bread) {
      this.selectedBread = bread
      this.selectedBread.reviews = this.reviews.filter(r => r.breadId === bread.id)
    },
    deleteBread(bread) {
      axios.delete(`http://localhost:3000/breads/${bread.id}`)
      .then(response => {
        this.breads = this.breads.filter(b => b.id !== bread.id)
        this.selectedBread = null
      })
    },
    addReview(bread) {
      axios.post('http://localhost:3000/reviews', {
        text: this.text,
        score: this.score,
        author: this.author,
        breadId: bread.id
      })
      .then(response => {
        bread.reviews.push(response.data);
        this.text = '';
        this.score = '';
        this.author = '';
      })
    },



  }
}
</script>