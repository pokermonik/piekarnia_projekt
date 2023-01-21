
  
  <script>
  import axios from 'axios'
  
  export default {
    name: 'Reviews',
    data() {
      return {
        reviews: [],
        text: '',
        score: '',
        author: '',
        breadId: ''
        }
    },
    created() {
      axios
        .get('http://localhost:3000/reviews')
        .then(response => (this.reviews = response.data))
    },
    methods: {
    addReview() {
      axios.post('http://localhost:3000/reviews', {
        text: this.text,
        score: this.score,
        author: this.author,
        breadId: this.breadId
      })
      .then(response => {
        this.reviews.push(response.data);
        this.text = '';
        this.score = '';
        this.author = '';
        this.breadId = '';
      })
    }
  }
  }
  </script>
  