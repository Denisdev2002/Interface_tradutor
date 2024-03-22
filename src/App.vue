<template>
<div class="forms">
  <div class="select-left">
    <select v-model="selectedOption1">
      <option disabled selected value="">Selecione uma opção</option>
      <option v-for="option in options" :value="option.value" :key="option.value">{{ option.label }}</option>
    </select>
  </div>
  <div class="texto">
    <input class="text-input" type="text" v-model="text" placeholder="Texto">
  </div>
  <div class="select-right">
    <select v-model="selectedOption2">
      <option disabled selected value="">Selecione uma opção</option>
      <option v-for="option in options" :value="option.value" :key="option.value">{{ option.label }}</option>
    </select>
  </div>
  <div>
    <button  class="btn btn-primary btn-margin" @click="traduzir()">
      Enviar
    </button>
  </div>
  <div class="resposta">
      <p>{{ translation }}</p>
  </div>
  
</div>
</template>

<script>
import { createApp, onMounted } from 'vue';
import axios from 'axios';
import translated_text from './components/translated_text.vue';


const API_URL = 'http://127.0.0.1:8000/traducao/'

export default {
  components: { translated_text },
  name: 'app',
  data() {
    return {
      translation:'',
      text:'',
      selectedOption1: '',
      selectedOption2: '',
      options: [
        {value:'pt', label:'Português'},
        {value:'en', label:'Inglês'},
        {value:'es', label:'Espanhol'},
        {value:'zh', label:'Chinês'},
        {value:'ko', label:'Coreano'},
        {value:'no', label:'Norueguês'},
        {value:'ru', label:'Russo'},
        {value:'pl', label:'Polonês'},
        {value:'su', label:'Suêco'}
      ]
    };
    
  },

  methods:{
     async traduzir(){
        // Verifica se os campos de texto e idioma estão preenchidos
        if (!this.text || !this.selectedOption1 || !this.selectedOption2) {
          alert('Por favor, preencha todos os campos.');
          return;
      }
      axios.post(API_URL,{
        text: this.text,
        language: this.selectedOption1,
        target_language: this.selectedOption2
        
      })
      .then(response => {
        console.log(this.text, this.selectedOption1, this.selectedOption2);
        var respon = response.data
        console.log(respon);
        this.translation = respon.translated_text;
        alert('Enviado com sucesso');
      })
      .catch(error => {
        console.error('Erro ao enviar dados:', error);
      });
    }
  }
}

</script>

<style scoped>
  .forms{
    border: 2px solid #302a2a;
    padding: 20px;
    margin: 20px auto;
    background-color: #bdb2b2;
    display: grid;
    grid-template-columns: auto 1fr auto auto; /* Define a estrutura de grid */
    align-items: center;
  }  
  .select-left {
  grid-column: 1;
}

.texto {
  grid-column: 2; 
}

.select-right {
  grid-column: 3; 
}

button {
  grid-column: 4;
  border-radius: 2cap;
  font-style: oblique;
}
p{
  border: 2px solid #302a2a;
  padding: 20px;
  margin: 25px auto;
  background-color: #fcf8f8;
  grid-template-columns: auto 3fr auto auto; /* Define a estrutura de grid */
  align-items: center;
  
}

</style>
