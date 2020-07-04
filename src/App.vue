<template>
  <div id="app">
    <Calc v-bind:title="message" v-on:result-event="appAction" />
    <hr>
    <div><table v-html="log"></table></div>
  </div>
</template>

<script>
import Calc from './components/Calc.vue'

//↓main.jsのnew Vue部分
export default {
  name: 'app',
  components: {
    //Calcコンポーネントを利用
    Calc
  },
  data:function() {
    return {
      message: 'CALC',
      result: [],
    }
  },
  computed: {
    log:function() {
      var table = '<tr><th class="head">Expression</th>' + 
      '<th class="head">Value</th></tr>'
      for(var i in this.result) {
        table += '<tr><td>' + this.result[i][0] + '</td><th>' + 
        this.result[i][1] + '</th></tr>'
      }
      return table
    }
  },
  created:function() {
    var items = localStorage.getItem('log')
    var logs = JSON.parse(items)
    if(logs != null) {
      this.result = logs
    }
  },
  methods: {
    appAction:function(exp, res) {
    //配列の先頭に追加
      this.result.unshift([exp, res])
      if(this.result.length > 10) {
        //配列の末尾を削除
        this.result.pop()
      }
      var log = JSON.stringify(this.result)
      //ブラウザ上のローカルストレージに保存
      localStorage.setItem('log', log)
    }
  }
}
//↑main.jsのnew Vue部分
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 5px;
}

tr td {
  padding: 5px;
  border: 1px solid gray;
}

tr th {
  padding: 5px;
  border: 1px solid gray;
}

tr th.head {
  background-color: black;
  color: white;
}
</style>
