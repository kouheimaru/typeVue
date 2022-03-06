<template>
  <div class="container" id="app">
    <div class="title">
      <h1>ことわざタイピング</h1>
    </div>
      <div v-if="startFlg==true">
    <p v-if="current_quesiton_counts == question_counts">
      記録
    </p>
        <p>
          {{sec}}秒
        </p>
      </div>
    <div>
    <button v-bind:style="styleSelectOne" v-if="startFlg!=true" class="selectButton mb-20" @click="gameSelectOne">3題</button>
    </div>
    <div>
    <button v-bind:style="styleSelectTwo" v-if="startFlg!=true" class="selectButton mb-20" @click="gameSelectTwo">5題</button>
    </div>
    <div>
    <button v-bind:style="styleSelectThree" v-if="startFlg!=true" class="selectButton mb-20" @click="gameSelectThree">7題</button>
    </div>
    <button v-if="startFlg!=true" class="startButton mb-20" @click="gameStart">スタート</button>
    <div v-if="startFlg">
    <div class="quistion mb-20">{{current_question}}</div>
    <div class="clear" v-if="current_quesiton_counts == question_counts">クリア!</div>
    <div class="clear mb-20" v-if="current_quesiton_counts == question_counts">
      <p>一覧</p>
      <li v-for="item in showWord" :key="item.id">
        {{ item }}
      </li>
    </div>
    <!-- リトライボタンを設置する -->
    <div class="typeFormWrapper mb-20"  v-if="current_quesiton_counts != question_counts">
      <input id="typeForm" v-model="typeBox" type="text" class="typeForm" />
    </div>
    <div class="gaugeWrapper mb-20">
      <div v-bind:style="styleObject" class="gauge"></div>
    </div>
    <!-- リトライボタンを設置する -->
    <button class="retryButton mb-20" v-if="current_quesiton_counts == question_counts" @click="gameRestart">リトライ</button>
    <div>{{current_quesiton_counts}}/{{question_counts}}</div>

    </div>
  </div>
</template>

<script>

export default {
  data: () => ({
    i: 0,
    demo:[],
    sample:[
      "弱い犬ほどよく吠える",
      "豚に真珠",
      "寝耳に水",
      "備えあれば憂い無し",
      "人間万事塞翁が馬",
      "覆水盆に返らず",
      "早起きは三文の徳",
      "口は災いの元",
      "聞くは一時の恥、聞かぬは一生の恥",
      "泣き面に蜂",
      "一を聞いて十を知る",
    ],
    number:5,
    //時間の計測する
    sec: 0, 
    showWord:[],
    timerOn: false,
    timerObj: null,
    selectFlg: 0,
    startFlg: '',
    current_question: "",
    //歌を選択できるようにする
    questions:[],
    //間違えていたキーを保存する
    typeBox: "",
    current_quesiton_counts:0,
    question_counts:1
  }),
  computed:{
    styleSelectOne:function(){
      let color = ""
      // 選択ごとに色を変更する
      if(this.selectFlg == 1 || this.selectFlg == 0){
        color = "lightgreen"
      }
      return{
        'background-color':color
      }
    },
    styleSelectTwo:function(){
      let color = ""
      // 選択ごとに色を変更する
      if(this.selectFlg == 2){
        color ="lightgreen"
      }
      return{
        'background-color':color
      }
    },
    styleSelectThree:function(){
      let color = ""
      // 選択ごとに色を変更する
      if(this.selectFlg == 3){
        color ="lightgreen"
      }
      return{
        'background-color':color
      }
    },
    styleObject:function(){
      let width = ""
      let color = ""
      width = 100 * this.current_quesiton_counts/this.question_counts + "%"
      // 最後の問題に正解したとき
      if(this.current_quesiton_counts == this.question_counts){
        color ="#03a9f4"
      }else{
        color = "orange"
      }
      return{
        'width' :width,
        'background-color':color
      }
    }
  },
  methods: {
   
    count: function() {
      this.sec++
    },
    start: function() {
      let self = this;
      this.timerObj = setInterval(function() {self.count()}, 1000)
    },
    stop: function() {
      clearInterval(this.timerObj);
    },
    shufle: function([...array]){
      for (let i = array.length - 1; i >= 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [array[i], array[j]] = [array[j], array[i]];
  }
  return array;
    },
    gameSelectOne: function(){
      this.selectFlg = 1
      this.questions = []
      this.demo =[]
      this.questions = this.shufle(this.sample)
      this.demo = this.shufle(this.sample)
      this.questions = this.demo.splice(0,3)
      this.current_question = []
      this.current_question = this.questions[0]
      this.question_counts = this.questions.length
    },
    gameSelectTwo: function(){
      this.selectFlg = 2
      this.questions = []
      this.demo =[]
      this.questions = this.shufle(this.sample)
      this.demo = this.shufle(this.sample)
      this.questions = this.demo.splice(0,5)
      this.current_question =[]
      this.current_question = this.questions[0]
      this.question_counts = this.questions.length
    },
    gameSelectThree: function(){
      this.selectFlg = 3
      this.questions = []
      //ランダムで10個
      this.demo =[]
      this.questions = this.shufle(this.sample)
      this.demo = this.shufle(this.sample)
      this.questions = this.demo.splice(0,7)
      this.current_question =[]
      this.current_question = this.questions[0]
      this.question_counts = this.questions.length
    },
    gameRestart: function(){
      //ゲーム情報を初期化する
      this.startFlg = false
      this.questions = this.demo
      this.current_quesiton_counts = 0
      this.question_counts = this.questions.length
      this.current_question = this.questions[0]
    },
    gameStart: function(){
      if(this.selectFlg == 0){
        this.gameSelectOne()
      }
      this.start()
      //配列を変換する
      this.startFlg = true;
      this.current_quesiton_counts = 0
      this.question_counts = this.questions.length

      //自動でカーソルをつける
      this.$nextTick(function(){
        document.getElementById('typeForm').focus()
      })
    }
  },
  //描画したときに実行する
  mounted: function(){
    this.current_question = this.questions[0]
    this.question_counts = this.questions.length
  },
  watch:{
    typeBox:function(e){
      if(e == this.current_question){
        this.showWord.push(this.questions[0])
        this.questions.splice(0,1)
        this.current_question = this.questions[0]
        this.typeBox = ""
        this.current_quesiton_counts += 1
        if(this.current_quesiton_counts == this.question_counts){
          this.stop()
        }
      }
        //一文字ずつ検証する(正解した部分は取り消しされない)
        //文字を分解して、入力した値を分解して、最後の一文字を検証（e）
        //正解した部分のみ色を変更する
        //正解したとき
        //間違えたときの値を保存する
      
    }
  }
};
</script>

<style>
* {
  font-family: inherit;
  font-size: inherit;
  line-height: inherit;
}
body {
  font-size: 32px;
}
.mb-20 {
  margin-bottom: 20px;
}
.container {
  width: 400px;
  margin: 0 auto;
  text-align: center;
}
.title {
  position: relative;
  font-size: 40px;
}
.startButton {
  background-color: rgb(40, 175, 238);
  color: #fff;
  padding: 4px 60px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
}
.selectButton{
  border-color: rgb(3, 39, 56);
  outline: none;
  background-color: rgb(78, 105, 117)
}

.retryButton {
  background-color: rgb(81, 255, 154);
  color: #fff;
  padding: 4px 60px;
  border: none;
  outline: none;
  border-radius: 8px;
  cursor: pointer;
}

.retryButton:hover {
  opacity: 0.7;
}

.startButton:hover {
  opacity: 0.7;
}

.quistion {
  color: gray;
}
.clear {
  color: #03a9f4;
}
.typeForm {
  text-align: center;
  outline: none;
  border: none;
}
.typeFormWrapper {
  border-bottom: 1px solid gray;
}
.gauge {
  height: 12px;
  transition: all .3s ease;
}
.gaugeWrapper{
  border: 1px solid;
  height: 12px;
}
</style>
