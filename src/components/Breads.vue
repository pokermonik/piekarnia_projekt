<template>
    <div>

        <div class="upbar">
            <div class="logo">
                <img src="../assets/logo.png" class="logo1"/>
            </div>
            <!-- kurs walut api -->
            <div name="Waluta" class="waluta">
                <label>   Wybierz walutę </label>
                <select id="selectWaluta" ref="selectWaluta" @change="changeMnoznik($event)">
                <option value="1">PLN</option>
                <option value="2">USD</option>
                <option value="3">GBP</option>
                <option value="4">EUR</option>
                <option value="5">CHF</option>
                </select>

            </div>
        </div>

        <!-- wszystkie karty z chlebkiem -->
        <div class="breads">
            <!-- karta z chlebkiem -->
            <div v-for="bread in breads" :key="bread.id" class="breads_card">
                <div @click="setSelectedBread(bread)" class="image">
                    <img src="../assets/photoBreads/bulki.jpg" class="image_fit"/>
                </div>
                <h2 @click="setSelectedBread(bread)">{{ bread.name }}</h2>
                <p>{{ (bread.price/mnoznik).toFixed(2)}}</p>
               
                
                <!-- edycja chlebka -->
                <div v-if="editingBread === bread" class="edit">
                    <form >
                        <div class="form_edit">
                            <label>Nazwa:
                                <input v-model="editingBread.name" type="text"/>
                            </label>
                            <label>Cena:
                                <input v-model="editingBread.price" type="number"/>
                            </label>
                            <label>Przepis:
                                <textarea v-model="editingBread.recipe"></textarea>
                            </label>
                        </div>
                        <div class="editback">
                            <button @click="saveChanges(bread)">Zapisz zmiany</button>
                            <button @click="cancelEditing()">Anuluj</button>
                        </div>
                    </form>
                </div>
                <!-- Przepis -->
                <div v-if="recipeBread === bread" class="recipe">
                    <div class="recipe_text"><p >{{ bread.recipe }}</p></div>
                    <button @click="cancelRecipe()" class="back">Powrót</button>
                </div>

                <!-- dodawanie opinni -->
                <div v-if="opinionBread === bread" class="opinion">
            
                    <!-- informacje o recenzji -->
                    <div class="opinion_text">
                        <h3>Opinie:</h3>
                        <div v-for="review in bread.reviews" :key="review.id" class="recenzja">
                            <div class="punkty">
                                <p>Recenzent: {{ review.author }}</p>
                                <p>{{ review.text }}</p>
                                <p >Punkty: {{ review.score }}</p>
                            </div>
                        </div>
                        <div class="addrecension">
                            <form @submit.prevent="addReview(bread)">
                                <label>Tekst: 
                                    <input v-model="text" placeholder="text" type="text" />
                                </label>
                                <label>Autor: 
                                    <input v-model="author" placeholder="author" type="text" />
                                </label>
                                <label>Ocena:
                                    <select v-model="score">
                                        <option v-for="n in 5" :key="n.valueOf">{{n}}</option>
                                    </select>
                                </label>
                                <div class="editback">
                                    <button type="submit">Dodaj opinie</button>
                                    <button @click="cancelOpinion()">Powrót</button>
                                </div>
                            </form>
                        </div>
                    </div>
                </div>

                <!-- dodawanie chlebka do koszyka -->
                <div v-if="toCartBread === bread" class="addToCart">
                    <div class="cartInputs">
                        <h3>Dodawanie:</h3>
                        <div class="addArticle">
                            <form>
                                <label>Ilość: 
                                    <input type="number" id="quantity" v-model="quantity"
                                    min="1" max="100" step="1">
                                </label>
                                <div class="cartback">
                                    <button v-if="selectedBread===bread" @click="addBreadToCart(selectedBread)">Dodaj</button> 
                                    <button @click="cancelAddToCart()">Anuluj</button>
                                </div>
                            </form>
                        </div>
                    </div>
                </div>

                <div v-if="selectedBread === bread" class="buttons">
                    <!-- po kliknieciu w chlebek -->
                    <button  v-if="selectedBread === bread" @click="deleteBread(bread)" >Usun</button>
                    <button v-if="selectedBread === bread" @click="setEditingBread(bread)">Edytuj</button>
                    <button v-if="selectedBread === bread" @click="setOpinionBread(bread)">Opinie</button>
                    <button v-if="selectedBread === bread" @click="setRecipeBread(bread)">Przepis</button>
                    <button v-if="selectedBread === bread" @click="setBreadToCart(bread)">Dodaj do koszyka</button>
                </div>
            </div>
            


            <!-- panel do dodawania chlebków -->
            <div class="add">
                <div class="T">
                </div>
                    <form @submit.prevent="addBread">
                        <div class="add_text">
                            <input v-model="name" placeholder="name" type="text" />
                            <input v-model="price" placeholder="price" type="text" />
                            <input v-model="recipe" placeholder="recipe" type="text" />
                            <font-awesome-icon icon="fa-solid fa-circle-plus" />
                            <div class="imagePreviewWrapper" :style="{ 'background-image': `url(${previewImage})` }" @click="selectImage"> 
                        <input ref="fileInput" type="file" @input="pickFile">
                            </div>
                        </div>
                        <div class="btn_add">
                            <button class="btn_color">Dodaj chlebek</button>
                        </div>
                    </form>
            </div>
            
        </div>
        <div>
                <button @click="sortBreads('asc')">Sort by Price (Ascending)</button>
                <button @click="sortBreads('desc')">Sort by Price (Descending)</button>
                </div>
        <div class="cart">
            <div id="title">Koszyk</div>
            <div id="content">
                <table id="receipt" ref="receipt">
                    <tr id="receiptRow">
                        <td>    
                            LP
                        </td>
                        <td class="Nazwa" style="text-align:center">
                            NAZWA
                        </td>
                        <td class="Ilosc" style="text-align:center">
                            ILOŚĆ
                        </td>
                        <td class="Cena" style="text-align:center">
                            CENA
                        </td>
                        <td class="Suma" style="text-align:center">
                            SUMA
                        </td>
                        
                        
                    </tr>
                    
                    <tr id="totalCena">
                        <td style="border:none"></td>
                        <td style="border:none"></td>
                        <td style="border:none"></td>
                        <td style="text-align:center">
                            RAZEM:
                        </td>
                        <td id="c" style="text-align:center" ref="calkowitaSuma">
        
                        </td>
        
                    </tr>
                    <tr>
                        <td style="border:none"></td>
                        <td style="border:none"></td>
                        <td style="border:none"></td>
                        <td style="text-align:center">
                            RAZEM ({{ waluta }}):
                        </td>
                        <td id="cw" style="text-align:center" ref="calkowitaSumaWaluta">
                            {{ (total/mnoznik).toFixed(2) }}
                        </td>
                    </tr>
                </table>
            </div>
          
        </div>
    </div>
    <footer>@Copyright Marek Chojnowski, Bartek Dawidziuk, Mateusz Węcławski, Grzegorz Denert 2023</footer>
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
        quantity:1,
        editingBread: null,
        opinionBread: null,
        recipeBread: null,
        toCartBreadArray: [],
        toCartBread:null,
        licznikCart:0,
        total:0,
        mnoznik:1,
        waluta:'PLN',
        calkowitaSumaWaluta:0,
        previewImage: null
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
        selectImage(){
            this.$refs.fileInput.click()
        },
        pickFile(){
            let input = this.$refs.fileInput
            let file = input.files
            if(file && file[0]) {
                let reader = new FileReader
                reader.onload = e => {
                    this.previewImage = e.target.result
                }
                reader.readAsDataURL(file[0])
                this.$emit('input', file[0])
            }
        },
        changeMnoznik(e)
        {
            this.waluta =this.$refs.selectWaluta[e.target.value-1].textContent
            if(this.waluta=="PLN")
            {
                this.mnoznik=1
            }
            else
            {
                axios.get('https://api.nbp.pl/api/exchangerates/rates/A/'+this.waluta+'/?format=json')
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
            if (!isNaN(this.price)) {
                axios.post('http://localhost:3000/breads', {
            name: this.name,
            price: this.price,
            recipe: this.recipe,
            imagePath: this.previewImage
            })
            .then(response => {
            this.breads.push(response.data);
            })
            } else {
                alert("Popraw dane! Cena musi być numerem")
            }
           
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
        setOpinionBread(bread) {
            this.opinionBread = bread;
        },
        setEditingBread(bread) {
            this.editingBread = bread;
        },
        setRecipeBread(bread) {
            this.recipeBread = bread;
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
        },
        cancelOpinion() {
            this.opinionBread = null;
        },
        cancelRecipe() {
            this.recipeBread = null;
        },
        setBreadToCart(bread) {
            this.toCartBread = bread;
        },
        cancelAddToCart() {
            this.toCartBread = null;
        },
        addBreadToCart(bread) {  
            this.toCartBreadArray[this.licznikCart]=bread;
            console.log(this.toCartBreadArray[this.licznikCart]);
            var paragon = this.$refs.receipt
            var totalRowLength=paragon.rows.length;
            var row=paragon.insertRow(totalRowLength-2)
            console.log("TU="+totalRowLength);
            let testCell = row.insertCell(-1)
            testCell.innerHTML=this.licznikCart+1
            testCell = row.insertCell(-1)
            testCell.innerHTML=this.toCartBreadArray[this.licznikCart].name

            testCell = row.insertCell(-1)
            testCell.innerHTML=this.quantity

            testCell = row.insertCell(-1)
            let cena = this.toCartBreadArray[this.licznikCart].price*1
            testCell.innerHTML=cena.toFixed(2)

            testCell = row.insertCell(-1)
            let sumaChlebka=this.toCartBreadArray[this.licznikCart].price * this.quantity
            testCell.innerHTML=sumaChlebka.toFixed(2)
            this.total=this.total+sumaChlebka
        

            var testSuma = this.$refs.calkowitaSuma;
            testSuma.innerHTML=this.total.toFixed(2)
            this.licznikCart++;
            this.quantity=1;
            this.toCartBread = null;

            event.preventDefault();
        },
        getImgUrl(pic) {
            console.log('../assets/photoBreads/'+pic)
            return require('../assets/photoBreads/'+pic)

        },
        sortBreads(order) {
      if (order === 'asc') {
        this.breads.sort((a, b) => a.price - b.price)
      } else {
        this.breads.sort((a, b) => b.price - a.price)
      }
    },

    }
  }
</script>

<style>
.imagePreviewWrapper{
    border-radius: 25px 25px 0 0;  
    
    
}
.punkty{
    border-color: #ccc;
    border-width: 0 0 2px 0;
    border-style: none none solid none;
    align-content: center;
}
.breads{
    font-family: Arial, Helvetica, sans-serif;
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    grid-gap: 25px;
    max-width: 1200px;
    margin: 0 auto; 
    padding-bottom: 25px;
    padding-top: 180px;
}
.breads_card{
    border: 1px solid black;
    background-color: #fff;
    border-radius: 25px 25px 25px 25px;
    width: 280px;
    height: 330px;
    position: relative;
    overflow:hidden;
    /* width: 280px;
    height: 330px; */ 
}
/* .breads_card:hover{
    filter:brightness(80%) ;
} */
.image{
    height: 220px;
}
.image_fit{
    height:100%;
    width: 100%;
    border-radius: 25px 25px 0 0;  
    
}
body{
    margin: 0;
    padding: 0;
}
.T{
    background-color: green;
    height: 330px;
    position: fixed;
}
.add{
    border: 1px solid black;
    background-color: #fff;
    border-radius: 25px 25px 25px 25px;
    width: 280px;
    height: 330px;
    transition: background-color 0.5s ease-in-out;
}
.add:hover{
    border: 1px solid black;
    background-color: green;
    border-radius: 25px 25px 25px 25px;
    width: 280px;
    height: 330px;
    
}

.upbar{
    width: 100%;
    background-color: #bb7900;
    position:fixed;
    top:0;
    left:0;
    height: 80px;
    z-index: 8;
}
.logo{
    margin: 0 auto;
    float:left;
    padding-left: 43.73%;
    position: absolute;
    width: 150px;
    height:150px;
    padding-top:5px;
}
.logo1{
    height:100%;
    width: 100%;
    padding: 5px;
}
.waluta{
    background-color: #fff;
    float: right;
    margin-right: 30px;
    margin-top: 30px;
}
.buttons{
    position: relative;
    background-color: rgba(128, 128, 128, .7);
    width: 280px;
    bottom: 305px;
    height: 170px;
    border-radius: 25px;
    padding-top:161px;
    z-index: 2;
    
}
button{
    background: none;
    border-radius: 0;
    border-color: #ccc;
    border-width: 0 2px 0 0;
    border-style: none solid none none;
    font-size:18px;
    color:#fff;
    
}
button:hover{
    color:#e29f22;
;
}
button:last-child{
    border:none;
}
button:hover:first-child{
    color: red;
}
a {
    text-decoration: none;
}
.recipe{
    position: relative;
    background-color: rgba(128, 128, 128);
    bottom: 305px;
    width: 280px;
    height: 332px;
    border-radius: 25px;
    z-index: 5;
    
}
.edit{
    position: relative;
    background-color: rgba(128, 128, 128);
    bottom: 305px;
    width: 280px;
    height: 332px;
    border-radius: 25px;
    z-index: 5;
}
.opinion{
    position: relative;
    background-color: rgba(128, 128, 128);
    bottom: 325px;
    width: 280px;
    height: 332px;
    border-radius: 25px;
    z-index: 5;
}
.back{
    position: absolute;
    bottom: 10px;
    left: 40%;
}
.editback{
    
    position:absolute;
    bottom:8px;
    left: 17%;
}
.cartback{
    position:absolute;
    bottom:30px;
    left: 30%;
}
.recipe_text{
    text-align: center;
    justify-content: center;
    padding:10px;
    inline-size: 250px;
    overflow-wrap: break-word;
}
.form_edit{
    padding-top: 8px;
    width: 250px;
}
.opinion_text{
    width: 250px;
    padding-left:10px;
}
.btn_add{
    color:black;
    padding-top: 210px;
    width: 260px;
}
p{
    margin:0;
}
.add_text{
    padding-top: 10px;

}
.btn_color{
    color:black;
}
.btn_add .btn_color:hover{
    color:#e29f22;
}
.addToCart{
    position: relative;
    background-color: rgba(128, 128, 128);
    bottom: 324px;
    width: 280px;
    height: 332px;
    border-radius: 25px;
    z-index: 5;
}
.cartInputs{
    width: 250px;
    padding-left:10px;
}


upbar{
    width: 100%;
    background-color: #bb7900;
    position:fixed;
    top:0;
    left:0;
    height: 80px;
    z-index: 8;
}

.cart{
    width: 100%;
    background-color: #f7f5f1;
    color:black;
    /* position:fixed;
    top:0;
    left:0;
    height: 80px;
    z-index: 8; */
}


#title, #content{
    margin: auto;
    width: 50%;
    padding: 10px;
    border-top: 2px solid grey;
    border-bottom: 2px solid grey;
}

#title{
    width: 50%;
    border-top: 2px solid grey;
    text-align: center;
    font-size: xx-large;
}

#receipt{
    margin:auto;
    width:100%;
    table-layout: fixed;
}
.Nazwa{
    
    text-align:left;
}
.Ilosc, .Cena, .Suma{
    text-align:right;
}
#receipt td{
    border: 1px solid black;
    word-break:break-all;
}

.tableData
{
    border-left: 1px solid gray;
    padding:5px;
}

#nowaPozycjaTable{
    border:none;
}

#c
{
    color: blue;
    text-align:right;}


table tr:not(:first-child)
{
    cursor: pointer;transition: all .25s ease-in-out;
}
table tr:not(:first-child):hover
{
    background-color: #ddd;
}

#errorsHere{
    height: 20px;
    position: relative;
    margin-left: 30%;
    width: 40%;
    text-align: center;
    color: red;
}

#error{
    align-items: center;
    height: 50px;
}

#btnReset{
    position: relative;
    bottom: 10px;
    margin-left: 45%;
}
input[type=text]:focus {
  background-color: lightblue;
}
.waluta{
  border-radius: 25px;
  border-style: solid;
  border-color:black;
  border-width:  medium;
  border-bottom-style:solid;
  
  background-color:#e29f22;
}
select {
  border-radius: 25px;

  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  background-color:#e29f22;
  border-style:none;
}
select:hover
{

  background-color:rgba(201, 135, 14, 0.578);
}
label
{
  
  border-right-style:solid;
  border-width:  medium;
  border-color:black;
  cursor: default;
  float:left;
}
footer{
  text-align: center;
  padding: 3px;
  background-color:#e29f22;
  color: white;
}

</style>
