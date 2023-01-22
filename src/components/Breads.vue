<template>
  <div>
    <div name="Waluta">
      <label>Wybierz walutę </label>
      <select id="selectWaluta" ref="selectWaluta" @change="changeMnoznik($event)">
        <option value="1">PLN</option>
        <option value="2">USD</option>
        <option value="3">GBP</option>
        <option value="4">EUR</option>
        <option value="5">CHF</option>
      </select>
    </div>
    <div v-for="bread in breads" :key="bread.id">
      <h2 v-on:click="setSelectedBread(bread)">{{ bread.name }}</h2>
      <p>{{ (bread.price/mnoznik).toFixed(2)}}</p>
      <p>{{ bread.recipe }}</p>
      <button v-if="selectedBread === bread" v-on:click="setEditingBread(bread)">Edytuj</button>
      <button v-if="selectedBread === bread" v-on:click="deleteBread(bread)">Usun</button>
      <button v-if="selectedBread === bread" v-on:click="showForm = !showForm">Dodaj opinie</button>

      <div v-if="selectedBread === bread">
        <div v-if="editingBread === bread">
            <form>
                <label>Nazwa:
                    <input v-model="editingBread.name" type="text"/>
                </label>
                <label>Cena:
                    <input v-model="editingBread.price" type="number"/>
                </label>
                <label>Przepis:
                    <textarea v-model="editingBread.recipe"></textarea>
                </label>
                <button v-on:click="saveChanges(bread)">Zapisz zmiany</button>
                <button v-on:click="cancelEditing()">Anuluj</button>
            </form>
        </div>
        <div v-if="showForm">
        <form @submit.prevent="addReview(bread)">
          <label>Tekst: 
        <input v-model="text" placeholder="text" type="text" />
    </label>
    <label>Autor: 
        <input v-model="author" placeholder="author" type="text" />
    </label>
    <label>Ocena:
        <select v-model="score">
            <option v-for="n in 5" :value="n">{{n}}</option>
        </select>
    </label>
    <button type="submit">Dodaj opinie</button>
        </form>
      </div>
        <h3>Opinie:</h3>
        <div v-for="review in bread.reviews" :key="review.id">
          <p>Recenzent: {{ review.author }}</p>
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
  </div>
</template>

<script>
import axios from 'axios'


export default {
  name: 'Breads-component',
  data() {
    return {
    breads: [],
    name: '',
    price: '',
    recipe: '',
    imagePath: 'path',
    selectedBread: null,
    showForm: false,
    text: '',
    score: '',
    Author: '',
    editingBread: null,



    mnoznik:1
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
    changeMnoznik(e)
    {
      let waluta =this.$refs.selectWaluta[e.target.value-1].textContent
      if(waluta=="PLN")
        {
          this.mnoznik=1
        }
      else
      {
        axios.get('https://api.nbp.pl/api/exchangerates/rates/A/'+waluta+'/?format=json')
        .then(response=>
        {
          this.mnoznik=response.data.rates[0].mid
        })
        .catch(e=>
        {
          this.errors.push(e)
        })
      
    }
    },

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
        console.log(response)
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
    editBread(bread) {

  },
  setEditingBread(bread) {
            this.editingBread = bread;
        },
        saveChanges(bread) {
          axios.put('http://localhost:3000/breads/' + bread.id, {
          name: bread.name,
          price: bread.price,
          recipe: bread.recipe,
        })
        .then(response => {
          console.log(response);
          this.editingBread = null;
        })
        .catch(error => {
          console.log(error);
        });
            this.editingBread = null;
        },
        cancelEditing() {
            this.editingBread = null;
        }



  }
}
</script>
