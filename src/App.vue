<template>
  <div id="app">

    <div id="search_bar">
      <form @submit="checkSlang">
        <b-input-group prepend="문장" class="mt-3">
          <b-form-input id="sentence" name="sentence" v-model="sentence"></b-form-input>
          <b-input-group-append>
            <b-button variant="outline-success" @click="sentence=0">지우기</b-button>
            <b-button variant="info" @click="checkSlang" type="submit">판별</b-button>
          </b-input-group-append>
        </b-input-group>
      </form>
    </div>

    <div id="result">
      <div v-if="result && sentence">
        <span>{{result}} %</span>
        <br>
        <iframe id="limer_html" v-bind:src="`http://127.0.0.1:5000/limer_html?text=${sentence}`"></iframe>
      </div>
    </div>

    <div id="footer">
      footer
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  data() {
    return {sentence: '', result: 0}

  },
  methods: {
    async checkSlang(e) {
      e.preventDefault()
      const res = await this.axios.get('http://127.0.0.1:5000', {
        params: {
          text: this.sentence
        }
      })
      this.result = res.data.result * 100
    }

  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
  #limer_html {
    width: 80vw;
    height: 300px;
    border:none
  }
</style>
