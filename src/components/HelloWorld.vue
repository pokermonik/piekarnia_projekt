<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <div>
      
      <p id="testowaCena" ref="testowaCena">5.69</p>
      
      <label>Wybierz walutę </label>
      <select id="selectWaluta" ref="selectWaluta" @change="testkur($event)">
        <option value="1">PLN</option>
        <option value="2">USD</option>
        <option value="3">GBP</option>
        <option value="4">EUR</option>
      </select>
    </div>
    <p>
      For a guide and recipes on how to configure / customize this project,<br>
      check out the
      <a href="https://cli.vuejs.org" target="_blank" rel="noopener">vue-cli documentation</a>.
    </p>
    <h3>Installed CLI Plugins</h3>
    <ul>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-babel" target="_blank" rel="noopener">babel</a></li>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-eslint" target="_blank" rel="noopener">eslint</a></li>
    </ul>
    <h3>Essential Links</h3>
    <ul>
      <li><a href="https://vuejs.org" target="_blank" rel="noopener">Core Docs</a></li>
      <li><a href="https://forum.vuejs.org" target="_blank" rel="noopener">Forum</a></li>
      <li><a href="https://chat.vuejs.org" target="_blank" rel="noopener">Community Chat</a></li>
      <li><a href="https://twitter.com/vuejs" target="_blank" rel="noopener">Twitter</a></li>
      <li><a href="https://news.vuejs.org" target="_blank" rel="noopener">News</a></li>
    </ul>
    <h3>Ecosystem</h3>
    <ul>
      <li><a href="https://router.vuejs.org" target="_blank" rel="noopener">vue-router</a></li>
      <li><a href="https://vuex.vuejs.org" target="_blank" rel="noopener">vuex</a></li>
      <li><a href="https://github.com/vuejs/vue-devtools#vue-devtools" target="_blank" rel="noopener">vue-devtools</a></li>
      <li><a href="https://vue-loader.vuejs.org" target="_blank" rel="noopener">vue-loader</a></li>
      <li><a href="https://github.com/vuejs/awesome-vue" target="_blank" rel="noopener">awesome-vue</a></li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';

export default
 {
  
  name: 'HelloWorld',
  props: {
    msg: String
  },
  testAPI:()=>(
  { 
    ceny:[],
    kurs:[],
    errors:[]
  }),

  methods:
  {
    testkur(e)
    {
      let nowaCena
      let cenaWyjsciowa=this.$refs.testowaCena.textContent
      let waluta =this.$refs.selectWaluta[e.target.value-1].textContent
      if(waluta=="PLN")
        {
          nowaCena=cenaWyjsciowa/this.kurs
          this.$refs.testowaCena.textContent=nowaCena
        }
      else
      {
        axios.get("http://localhost:3000/breads")
        .then(response=>
        {
          console.log(response.data.length)
          for(let i=0;i<response.data.length;i++)
          {
            this.ceny.push(response.data[i].price)
            console.log(response.data[i].price)
            console.log(this.ceny[i])
          }
          
        }
        
        )
        
      
        axios.get('https://api.nbp.pl/api/exchangerates/rates/A/'+waluta+'/?format=json')
        .then(response=>
        {
          this.ceny=response.data.rates[0].mid
        
          nowaCena=this.ceny*cenaWyjsciowa
          this.kurs=this.ceny
          
          this.$refs.testowaCena.textContent=nowaCena
        })
        .catch(e=>
        {
          this.errors.push(e)
        })
      }
      
      
    
    }
  },
  mounted()
  
  {
 
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
