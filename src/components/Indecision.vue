<template>
  <img :src="image" v-if="image" alt="bg">
  <div class="bg-dark"></div>
  <div class="indecision-container">
<input
      type="text"
      placeholder="Realiza una pregunta"
      v-model="pregunta">
    <p>Termina la pregunta con un signo de interrogación (?)</p>
    <h2 v-if="preguntaValida" >{{ pregunta }}</h2>
    <h1 v-if="preguntaValida">{{ respuesta }}</h1>
  </div>

</template>

<script>
export default {
    name: 'Indecision',
    data() {
      return {
        pregunta: '',
        respuesta: null,
        image: null,
        preguntaValida : false
      }
    },
    methods:{
      async getRespuesta(){
          this.respuesta = 'Pensando...'
        const { answer, image } = await fetch('https://yesno.wtf/api').then( r => r.json())
        this.preguntaValida = true
        this.image = image
        this.respuesta = (answer === 'yes')? 'Sí' : 'No'
      }
    },
    watch:{
      pregunta(value, oldValue){
        if( value.includes('?') ){
          this.getRespuesta()
        }
        this.preguntaValida = false
      }
    }

}
</script>

<style scoped>

    img, .bg-dark {
        height: 100vh;
        left: 0px;
        max-height: 100%;
        max-width: 100%;
        position: fixed;
        top: 0px;
        width: 100vw;
    }

    .bg-dark {
        background-color: rgba(0, 0, 0, 0.4);
    }

    .indecision-container {
        position: relative;
        z-index: 99;
    }
    
    input {
        width: 250px;
        padding: 10px 15px;
        border-radius: 5px;
        border: none;
    }
    input:focus {
        outline: none;
    }

    p {
        color: white;
        font-size: 20px;
        margin-top: 0px;
    }

    h1, h2 {
        color: white;
    }
    
    h2 {
        margin-top: 150px;
    }

</style>