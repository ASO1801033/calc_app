<template>
  <div class="hello">
    <h1>{{ title }}</h1>
    <p>{{ message }}</p>
    <hr>
    <div>
      <div>
        <textarea v-model="fomula" cols="40" rows="5"></textarea>
      </div>
      <div>
        <button v-on:click="doAction">CALC</button>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'Calc',
    props: {
      title: String,
    },
    deta:function() {
      return {
        message: 'Enter expression:',
        fomula: '0',
      }
    },
    methods: {
      doAction:function() {
        //配列作成
        var arr = this.fomula.trim().split('\n')
        //配列の末尾を取り出し
        var last = arr.pop()
        var fn = ''
        for(var n in arr) {
          if(arr[n].trim() != '') {
            fn += 'var ' + arr[n] + ';'
          }
        }
        fn += 'return ' + last + ';'
        var exp = 'function f(){' + fn + '} f();'
        var ans = eval(exp)
        this.message = 'answer: ' + ans
        //配列を結合
        var re = arr.join(';').trim()
        if(re != '') {
          re += ';'
        }
        re += last
        //App.vueのresult-eventと連携(発火・呼び出し)
        this.$emit('result-event', re, ans)
      }
    }
  }
</script>