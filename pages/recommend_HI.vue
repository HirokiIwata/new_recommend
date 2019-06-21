<template>
  <section>
    <!-- <div>
     //デバッグゾーン
      {{q_num}}
      {{visitor_tags}}
    </div> -->

    <div v-if="q_num==0" class="title">
      <h3><p>「展示物推薦ver.2」のページです<br><br>
      表示される語句の中から<br>気になるものを<br>
      選んでください<br>(複数回答可)</p></h3>
      <v-spacer><br></v-spacer>
    </div>

    <!-- 質問の進捗表示 -->
    <div v-if="q_num>0&&q_num<=data_query.length">
      {{q_num}}/{{data_query.length}}
    </div>

    <!-- 質問表示 -->
    <div
    class="query"
    v-for="(item,index) in data_query"
    :key="index">
      <div
      v-if="q_num==index+1">
        <div v-for="item_a in item"
        :key="item_a.tag_id">
          <v-checkbox
          height = 2
          size = "7"
          v-model="selected"
          :label="item_a.tag"
          :value="[item_a.tag,item_a.tag_id]"></v-checkbox>
        </div>
      </div>
    </div>


    <!-- 推薦展示物表示 -->
    <div
    v-if="q_num==-10000">
      <v-layout column width="350px"
      v-for="(item,index) in recommend_exhibits"
      :key="index">
        <v-flex>
          <v-spacer><br></v-spacer>
          <v-card light raised>
            <v-chip color="orange" text-color="white" class="rank">
              オススメ{{index+1}}位
              <v-icon right>star</v-icon>
            </v-chip>

            <v-img
              class="card_img"
              :src="item.src"
              width="350px"
            >
            </v-img>

            <v-card-title primary-title class="primary-title">
              <div>
              <div class="headline">{{item.exhibit}}</div>
              <v-spacer><br></v-spacer>

              <div class="basis">
                <span
                v-for="tag in item.basis"
                :key="tag">
                  <v-chip
                  class="basis"
                  color="secondary"
                  text-color="white"
                  disable
                  small>
                    <v-icon>label</v-icon>{{ tag }}
                  </v-chip>
                </span>
              </div>
              </div>
            </v-card-title>

            <v-card-actions class="actions">
              <v-btn
              flat
              outline
              color="purple"
              :href="item.url"
              target="_blank"
              >解説を見る</v-btn>
              <v-btn flat outline color="accent" @click="show[index] = !show[index]">
                アクセス
                <v-icon>{{ show[index] ? 'keyboard_arrow_up' : 'keyboard_arrow_down' }}</v-icon>
              </v-btn>
            </v-card-actions>

            <v-slide-y-transition>
              <v-img class="more" v-show="show[index]"
                :src="item.map"
                width="350px">
              </v-img>
            </v-slide-y-transition>
          </v-card>
          <v-spacer><br></v-spacer>
        </v-flex>
      </v-layout>
    </div>

    <!-- テストボタン -->
    <!-- <div>
      <v-btn v-if="q_num==0" round color="primary" large v-on:click="show_result">テスト</v-btn>
    </div> -->

    <!-- スタートボタン -->
    <div>
      <v-btn v-if="q_num==0" round color="primary" large v-on:click="web_reccomend">スタート</v-btn>
    </div>

    <!-- 初期ロード表示 -->
    <div class="loading">
      <v-progress-circular
      v-if="q_num==-5000"
      :size="50"
      color="primary"
      indeterminate
    ></v-progress-circular>
    </div>

    <div>
      <v-btn v-if="q_num!=0&&data_query&&0<=q_num&&q_num<=data_query.length" round color="primary" large v-on:click="get_checkbox">次へ</v-btn>
    </div>

    <div v-if="q_num!=0&&data_query&&q_num>data_query.length">
      <p>お疲れ様でした！</p>
      <v-btn round color="primary" large v-on:click="show_result">結果を表示する</v-btn>
    </div>

    <!-- ロード中 -->
    <div v-if="q_num==-9000">
      <v-progress-circular
      :buffer-value="10"
      :rotate="-90"
      :size="200"
      :width="15"
      :value="value"
      color="primary"
    >
        分析中...
      </v-progress-circular>
    </div>

  </section>
</template>




<script>

export default {
  layout: 'nested',
  data(){
    return{
      interval: {}, //progress
      value: 0, //progress
      show: {0: false, 1: false, 2: false},
      json: null,
      result: null,
      q_num: -5000,
      v_tags:[
        {tag:'文学',tag_id:1},
        {tag:'映画',tag_id: 2},
        {tag:'歴史',tag_id: 3},
        {tag:'経済',tag_id: 4},
        {tag:'アニメ',tag_id: 5},
        {tag:'恋愛',tag_id: 6},
        {tag:'旅行',tag_id: 7},
        {tag:'科学',tag_id: 8},
        {tag:'アート',tag_id: 9},
        {tag:'芸能',tag_id: 10},
        {tag:'ビジネス',tag_id: 11},
        {tag:'コンピュータ',tag_id: 12},
        {tag:'法律',tag_id: 13},
        {tag:'政治',tag_id: 14},
        {tag:'環境',tag_id: 15},
        {tag:'軍事',tag_id: 16},
        {tag:'音楽',tag_id: 17},
        {tag:'テレビ',tag_id: 18},
        {tag:'SNS',tag_id: 19},
        {tag:'住宅',tag_id: 20},
        {tag:'哲学',tag_id: 21},
        {tag:'宗教',tag_id: 22},
        {tag:'グルメ',tag_id: 23},
        {tag:'美容',tag_id: 24},
        {tag:'ファッション',tag_id: 25},
        {tag:'生物',tag_id: 26},
        {tag:'数学',tag_id: 27},
        {tag:'医学',tag_id: 28},
        {tag:'鉄道',tag_id: 29},
        {tag:'車',tag_id: 30},
        {tag:'占い',tag_id: 31},
        {tag:'料理',tag_id: 32},
        {tag:'写真',tag_id: 33},
        {tag:'絵画',tag_id: 34},
        {tag:'ゲーム',tag_id: 35},
        {tag:'教育',tag_id: 36},
        {tag:'スポーツ',tag_id: 37},
        {tag:'アウトドア',tag_id: 38},
        {tag:'外国語',tag_id: 39},
        {tag:'漫画',tag_id: 40}
      ],
      e_tags: [
        {tag:'月食',tag_id:1},
        {tag:'満ち欠け',tag_id: 2},
        {tag:'かぐや',tag_id: 3},
        {tag:'衛星',tag_id: 4},
        {tag:'ケプラー',tag_id: 5},
        {tag:'万有引力',tag_id: 6},
        {tag:'ニュートン',tag_id: 7},
        {tag:'ブラックホール',tag_id: 8},
        {tag:'探査機',tag_id: 9},
        {tag:'ミッション',tag_id: 10},
        {tag:'惑星',tag_id: 11},
        {tag:'タイムラグ',tag_id: 12},
        {tag:'銀河系',tag_id: 13},
        {tag:'天の川',tag_id: 14},
        {tag:'円盤',tag_id: 15},
        {tag:'断面図',tag_id: 16},
        {tag:'プラネタリウム',tag_id: 17},
        {tag:'歴史',tag_id: 18},
        {tag:'オルゴール',tag_id: 19},
        {tag:'カールツァイス',tag_id: 20},
        {tag:'旧名古屋市科学館',tag_id: 21},
        {tag:'思い出',tag_id: 23},
        {tag:'タイムカプセル',tag_id: 24},
        {tag:'天動説',tag_id: 25},
        {tag:'地動説',tag_id: 26},
        {tag:'ガリレオ',tag_id: 27},
        {tag:'コペルニクス',tag_id: 28},
        {tag:'宇宙観',tag_id: 29},
        {tag:'神話',tag_id: 30},
        {tag:'占星術',tag_id: 31},
        {tag:'遺跡',tag_id: 32},
        {tag:'江戸',tag_id: 33},
        {tag:'和製',tag_id: 36},
        {tag:'レンズ',tag_id: 37},
        {tag:'屈折',tag_id: 38},
        {tag:'反射',tag_id: 39},
        {tag:'望遠鏡',tag_id: 40},
        {tag:'倍率',tag_id: 41},
        {tag:'視野',tag_id: 42},
        {tag:'口径',tag_id: 44},
        {tag:'X線',tag_id: 45},
        {tag:'紫外線',tag_id: 46},
        {tag:'赤外線',tag_id: 47},
        {tag:'電波',tag_id: 48},
        {tag:'スペクトル',tag_id: 49},
        {tag:'プリズム',tag_id: 50},
        {tag:'波長',tag_id: 51},
        {tag:'回折格子',tag_id: 52},
        {tag:'パラボラアンテナ',tag_id: 54},
        {tag:'宇宙背景放射',tag_id: 55},
        {tag:'無線',tag_id: 56},
        {tag:'超高温',tag_id: 58},
        {tag:'高エネルギー',tag_id: 59},
        {tag:'レントゲン',tag_id: 60},
        {tag:'光害',tag_id: 61},
        {tag:'夜景',tag_id: 62},
        {tag:'環境',tag_id: 63},
        {tag:'都会',tag_id: 64},
        {tag:'宇宙線',tag_id: 65},
        {tag:'霧箱',tag_id: 66},
        {tag:'放射線',tag_id: 67},
        {tag:'イオン',tag_id: 68}],
      data_query: null, // , {tag:'ブラックホール',tag_id: 48}, {tag:'ブラックホール',tag_id: 55}
      visitor_tags: [],
      // visitor_tags_test: [['万有引力',26],['ケプラー',28] , ['ブラックホール', 29], ['ニュートン', 27], ['物理法則',30]],
      selected: [],
      recommend_exhibits: []
    }
  },
  methods: {
    show_result: function(event){

      this.q_num = -9000 //値は適当です
      const recommend_num = 3; //オススメする展示物の数
      const adopted_num = 2; //pmi上位採用数
      let a518 = {id: 518,exhibit: "5. 月の満ち欠け",url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/57",point: 0,pmi: 0,basis: [],src: require("../static/A518-photo.jpg"),map: require("../static/map5.jpg")}
      let a521 = {id: 521,exhibit: "7. 惑星の動きと引力",url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/65",point: 0,pmi: 0,basis: [],src: require("../static/A521-photo.jpg"),map: require("../static/map7.jpg")}
      let a522 = {id: 522,exhibit: "6. 惑星探査",url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/60",point: 0,pmi: 0,basis: [],src: require("../static/A522-photo.jpg"),map: require("../static/map6.jpg")}
      let a527 = {id: 527,exhibit: "14. 銀河系と天の川",url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/58",point: 0,pmi: 0,basis: [],src: require("../static/A527-photo.jpg"),map: require("../static/map14.jpg")}
      let a529 = {id: 529,exhibit: "1. プラネタリウムの歴史",url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/74",point: 0,pmi: 0,basis: [],src: require("../static/A529-photo.jpg"),map: require("../static/map1.jpg")}
      let a534 = {id: 534,exhibit: "16. デジタルタイムカプセル",url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/67",point: 0,pmi: 0,basis: [],src: require("../static/A534-photo.jpg"),map: require("../static/map16.jpg")}
      let a501 = {id: 501,exhibit: "2. 古代人の宇宙",url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/56",point: 0,pmi: 0,basis: [],src: require("../static/A501-photo.jpg"),map: require("../static/map2.jpg")}
      let a502 = {id: 502,exhibit: "4. 天動説から地動説へ",url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/69",point: 0,pmi: 0,basis: [],src: require("../static/A502-photo.jpg"),map: require("../static/map4.jpg")}
      let a503 = {id: 503,exhibit: "3. 江戸時代の天文学",url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/62",point: 0,pmi: 0,basis: [],src: require("../static/A503-photo.jpg"),map: require("../static/map3.jpg")}
      let a504 = {id: 504,exhibit: "9. 光学望遠鏡のしくみ",url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/75",point: 0,pmi: 0,basis: [],src: require("../static/A504-photo.jpg"),map: require("../static/map9.jpg")}
      let a505 = {id: 505,exhibit: "8. 望遠鏡をのぞいてみよう",url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/59",point: 0,pmi: 0,basis: [],src: require("../static/A505-photo.jpg"),map: require("../static/map8.jpg")}
      let a508 = {id: 508,exhibit: "10. さまざまな波長",url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/66",point: 0,pmi: 0,basis: [],src: require("../static/A508-photo.jpg"),map: require("../static/map10.jpg")}
      let a509 = {id: 509,exhibit: "13. 分光観測とスペクトル",url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/70",point: 0,pmi: 0,basis: [],src: require("../static/A509-photo.jpg"),map: require("../static/map13.jpg")}
      let a510 = {id: 510,exhibit: "12. 電波天文学",url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/61",point: 0,pmi: 0,basis: [],src: require("../static/A510-photo.jpg"),map: require("../static/map12.jpg")}
      let a512 = {id: 512,exhibit: "15. X線天文学",url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/72",point: 0,pmi: 0,basis: [],src: require("../static/A512-photo.jpg"),map: require("../static/map15.jpg")}
      let a513 = {id: 513,exhibit: "17. 市街光と星空",url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/71",point: 0,pmi: 0,basis: [],src: require("../static/A513-photo.jpg"),map: require("../static/map17.jpg")}
      let a515 = {id: 515,exhibit: "11. 宇宙線をみる",url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/73",point: 0,pmi: 0,basis: [],src: require("../static/A515-photo.jpg"),map: require("../static/map11.jpg")}

      let exhibits_list = [a518,a521,a522,a527,a529,a534,a501,a502,a503,a504,a505,a508,a509,a510,a512,a513,a515]
      let visitor_tags = this.visitor_tags
      let db = [];


      //分析中...表示用
      this.interval = setInterval(() => {
        if (this.value === 110) {
          // return (this.value = 0)
          this.q_num = -10000
        }
        this.value += 5
      }, 200)

      console.log(visitor_tags)
      for(let v of visitor_tags){
        let tmp_json = [];
        let tmp = this.json.filter(function(element){
          return element.visitor_tag_id == v[1]
        })
        // console.log(tmp)
        for (let w of tmp){
          let tmp1 = {}
          tmp1.exhibit = w.exhibit
          tmp1.exhibit_id = w.exhibit_id
          tmp1.exhibit_tag = w.exhibit_tag
          tmp1.visitor_tag = w.visitor_tag
          tmp1.visitor_tag_id = w.visitor_tag_id
          tmp1.pmi = w.pmi_diff_from_med
          tmp_json.push(tmp1)
        }
        // console.log(tmp_json)
        db.push(tmp_json)
      }
      // console.log(db)
      // 以下，来館者タグの重複を考慮した処理
      let db_unique = []
      for(let d of db){
        var arrObj = {};
        for (var n = 0; n < d.length; n++) {
          arrObj[d[n]['exhibit_tag']] = d[n];
        }
        d = [];
        for (var key in arrObj) {
          d.push(arrObj[key]);
        }
        db_unique.push(d)
      }

      // console.log(db)
      let db_sorted = [];
      for(let v of db_unique){
        v.sort((a, b) => {
        if (a.pmi > b.pmi) return -1;
        if (a.pmi < b.pmi) return 1;
        return 0;
        });
        db_sorted.push(v)
      }
      console.log(db_sorted)
      for(let v of db_sorted){ // v = 65行
        // console.log(v)
        for(let i=0; i<adopted_num; i++){
          let tmp_exhibit_tag = v[i].exhibit_tag
          let tmp_exhibit_id = v[i].exhibit_id
          let tmp_json = this.json.filter(function(element){
            return element.exhibit_tag == tmp_exhibit_tag
          })
          // console.log(tmp_json)

          // tmp_jsonの重複除去
          var arrObj = {};
          for (var n = 0; n < tmp_json.length; n++) {
            arrObj[tmp_json[n]['exhibit']] = tmp_json[n];
          }
          tmp_json = [];
          for (var key in arrObj) {
            tmp_json.push(arrObj[key]);
          }

          // console.log(tmp_json)
          for(let l of tmp_json){
            for(let w of exhibits_list){
              if(w.id===l.exhibit_id){
                w.point++;
                w.pmi=w.pmi+(Math.round(v[i].pmi*100))/100;
                w.basis.push(v[i].visitor_tag)
                // console.log(exhibits_list)
              }
            }
          }
        }
      }

      exhibits_list.sort(function(a, b){
	       if (a.point < b.point) return 1;
	       if (a.point > b.point) return -1;
         if (a.pmi < b.pmi) return 1;
	       if (a.pmi > b.pmi) return -1;
	       return 0;
      });

      // console.log(exhibits_list)
      // for(let i=0; i<recommend_num; i++){
      //   var arrObj = {};
      //   for (var n = 0; n < exhibits_list[i].length; n++) {
      //     arrObj[tmp_json[n]['basis']] = tmp_json[n];
      //   }
      //   tmp_json = [];
      //   for (var key in arrObj) {
      //     tmp_json.push(arrObj[key]);
      //   }
      // }


      for(let i=0; i<recommend_num; i++){
      //   var overlap = exhibits_list[i].basis.filter(function (x, i, self) {
      //       return self.indexOf(x) !== self.lastIndexOf(x);
      //   });
        // console.log(exhibits_list[i].basis)
        var unique = exhibits_list[i].basis.filter(function (x, i, self) {
            return self.indexOf(x) === i;
        });


        // x3 とかの表示
        // console.log(overlap)
        // console.log(unique)
        //
        // var counts = {};
        //
        // for(var k=0;k< overlap.length;k++)
        // {
        //   var key = overlap[k];
        //   counts[key] = (counts[key])? counts[key] + 1 : 1 ;
        // }

        // for(let v in counts){
        //   for(let j=0; j<unique.length; j++){
        //     if(v===unique[j]){
        //       unique[j] = unique[j] + "x" + counts[v]
        //     }
        //   }
        // }

        exhibits_list[i].basis = unique
        // console.log(exhibits_list)
        this.recommend_exhibits.push(exhibits_list[i])
      }
      // console.log(this.recommend_exhibits)

    },

    get_checkbox: function(event){
      // this.visitor_tags.push(this.selected[0])
      for(let i=0; i<this.selected.length; i++){
        this.visitor_tags.push(this.selected[i])
      }
      this.q_num++;
      this.selected.length = 0;
    },
    web_reccomend: function (event) {
      // this.$router.push({ path: 'hoge', query: { plan: 'pri', aaa: 'AAA'}})
      // console.log(this.json)
      // const tmp = this.json.find(function(element){
      //   return element.id == 3078
      // })
      // const tmp = this.json.filter(function(element){
      //   return element.visitor_tag_id == 30
      // })
      // console.log(tmp)
      // this.result = tmp[0].id
      // this.isResultShow = true
      // ここにオススメプログラムをつらつらと書いていく
      // let tags;
      // tags = this.json.filter(function(element){
      //   return element.visitor_tag_id == 2
      // })
      let array = this.v_tags;
      let v_tags; //来館者タグ一覧
      const q_tag_num = 7 //一回あたりのタグ提示数
      // let tags
      let q_times = []
      let query = []

      for(let i = array.length - 1; i > 0; i--){
        let r = Math.floor(Math.random() * (i + 1));
        let tmp = array[i];
        array[i] = array[r];
        array[r] = tmp;
        v_tags = array
      }
      // 質問クエリの単語数を決める
      let tmp = v_tags.length%q_tag_num
      if (tmp <= Math.ceil(q_tag_num/2)){
        for (let i=0; i<tmp; i++){
          q_times.push(q_tag_num+1);
        }
        tmp = (v_tags.length-q_times.length*(q_tag_num+1))/q_tag_num
        for (let i=0; i<tmp; i++){
          q_times.push(q_tag_num);
        }
      }else{
        for (let i=0; i<q_tag_num-tmp; i++){
          q_times.push(q_tag_num-1);
        }
        tmp = (v_tags.length-q_times.length*(q_tag_num-1))/q_tag_num
        for (let i=0; i<tmp; i++){
          q_times.push(q_tag_num);
        }
      } // q_times = [8, 8, 7, 7, 7, 7, 7, 7, 7]

      // 質問クエリを作成する
      for (let i=0; i<q_times.length; i++){
        query.push(v_tags.slice(0,q_times[i]))
        v_tags.splice(0,q_times[i])
      }

      this.data_query = query;

      this.q_num++;

      // console.log(this.q_num)

    }
  },



  created() {
    //do something after creating vue instance
    // console.log('こんいgんkg')
    this.json = require('../static/WebPMI_deviation2.json')
    // this.q_num = 0;
  },

  mounted() {
    //do something before mounting vue instance
    this.q_num = 0;
  }
  // asyncData(){
  //   // console.log('testss')
  //   return{
  //     json:require('../static/mysql.json')
  //   }
  // }
}
</script>

<style>
/* .basis{
  width: 350px;
} */
.query{
  font-weight: bold;
  font-size: 2em;
}
.title{
  font-size: 14px;
  font-weight: bold;
}
.card_img{
  justify-content: center;
  align-items: center;
}
.actions{
  justify-content: center;
}
.primary-title{
  align-items: center;
  justify-content: center;
  width: 350px;
}
.more {
  max-width: 350px;
}

.rank {
  font-size: 16px;
  font-weight: bold;
}
.container {
  min-height: 100vh;
  justify-content: center;
  align-items: center;
  text-align: center;
}

</style>
