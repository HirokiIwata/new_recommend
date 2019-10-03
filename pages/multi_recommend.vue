<template>
  <section>
  
    <div v-if="page_count==0" class="title">
      <h3><p>展示物推薦のページです<br><br>
      次に表示される言葉から<br>気になるものを<br>
      選んでください<br>(複数回答可)</p></h3>
      <v-spacer><br></v-spacer>
    </div>

    <!-- 質問の進捗表示 -->
    <div v-if="page_count>0 && page_count<=data_query.length">
      {{page_count}}/{{data_query.length}}
    </div>

    <!-- 質問表示 -->
    <div
    class="query"
    v-for="(item,index) in data_query"
    :key="index">
      <div
      v-if="page_count==index+1">
        <div v-for="item_a in item"
        :key="item_a.tag_id">
          <v-checkbox
          height = 2
          v-model="selected_items"
          :size = "200"
          :label="item_a.tag"
          :value="[item_a.tag,item_a.tag_id]"></v-checkbox>
        </div>
      </div>
    </div>


    <!-- 推薦展示物表示 -->
    <div
    v-if="page_count==-10000">
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

    <div>
      <v-btn v-if="page_count==0" round color="primary" large v-on:click="web_recommend">スタート</v-btn>
    </div>

    <!-- 初期ロード表示 -->
    <div class="loading">
      <v-progress-circular
      v-if="page_count==-5000"
      :size="50"
      color="primary"
      indeterminate
    ></v-progress-circular>
    </div>

    <div>
      <v-btn v-if="page_count!=0&&data_query&&0<=page_count&&page_count<=data_query.length" 
      round color="primary" large v-on:click="get_checkbox">次へ</v-btn>
    </div>

    <div v-if="page_count!=0&&data_query&&page_count>data_query.length">
      <p>お疲れ様でした！</p>
      <v-btn round color="primary" large v-on:click="show_result">結果を表示する</v-btn>
    </div>

    <!-- ロード中 -->
    <div v-if="page_count==-9000">
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
      page_count: -5000,
      v_tags:[
        {tag:'月',tag_id:1},
        {tag:'かぐや',tag_id: 2},
        {tag:'衛星',tag_id: 3},
        {tag:'惑星',tag_id: 4},
        {tag:'引力',tag_id: 5},
        {tag:'ブラックホール',tag_id: 6},
        {tag:'探査機',tag_id: 7},
        {tag:'人工衛星',tag_id: 8},
        {tag:'銀河系',tag_id: 9},
        {tag:'天の川',tag_id: 10},
        {tag:'太陽系',tag_id: 11},
        {tag:'プラネタリウム',tag_id: 12},
        {tag:'歴史',tag_id: 13},
        {tag:'旧名古屋市科学館',tag_id: 14},
        {tag:'タイムカプセル',tag_id: 15},
        {tag:'思い出',tag_id: 16},
        {tag:'天動説',tag_id: 17},
        {tag:'地動説',tag_id: 18},
        {tag:'ガリレオ',tag_id: 19},
        {tag:'古代',tag_id: 20},
        {tag:'神話',tag_id: 21},
        {tag:'遺跡',tag_id: 22},
        {tag:'江戸時代',tag_id: 23},
        {tag:'古美術',tag_id: 24},
        {tag:'望遠鏡',tag_id: 25},
        {tag:'レンズ',tag_id: 26},
        {tag:'鏡',tag_id: 27},
        {tag:'観測',tag_id: 28},
        {tag:'倍率',tag_id: 29},
        {tag:'X線',tag_id: 30},
        {tag:'紫外線',tag_id: 31},
        {tag:'電波',tag_id: 32},
        {tag:'光',tag_id: 33},
        {tag:'色',tag_id: 34},
        {tag:'波長',tag_id: 35},
        {tag:'アンテナ',tag_id: 36},
        {tag:'星空',tag_id: 37},
        {tag:'光害',tag_id: 38},
        {tag:'名古屋',tag_id: 39},
        {tag:'宇宙線',tag_id: 40},
        {tag:'放射線',tag_id: 41},
        {tag:'イオン',tag_id: 42}
      ],
      data_query: null,
      visitor_tags: [],
      selected_items: [],
      recommend_exhibits: []
    }
  },
  methods: {
    show_result: function(event){

      this.page_count = -9000 //値は適当です
      const recommend_num = 3; //オススメする展示物の数

      let a518 = {id: 518, exhibit: "5. 月の満ち欠け", 
                  url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/57", 
                  point: 0, basis: [], exhibit_tag: ['月', 'かぐや', '衛星'], 
                  src: require("../static/A518-photo.jpg"),  
                  map: require("../static/map5.jpg")}
      let a521 = {id: 521, exhibit: "7. 惑星の動きと引力", 
                  url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/65", 
                  point: 0, basis: [], exhibit_tag: ['惑星', '引力', 'ブラックホール'], 
                  src: require("../static/A521-photo.jpg"), 
                  map: require("../static/map7.jpg")}
      let a522 = {id: 522, exhibit: "6. 惑星探査", 
                  url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/60", 
                  point: 0, basis: [], exhibit_tag: ['惑星', '探査機', '人工衛星'], 
                  src: require("../static/A522-photo.jpg"), 
                  map: require("../static/map6.jpg")}
      let a527 = {id: 527, exhibit: "14. 銀河系と天の川", 
                  url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/58", 
                  point: 0, basis: [], exhibit_tag: ['銀河系', '天の川', '太陽系'], 
                  src: require("../static/A527-photo.jpg"), 
                  map: require("../static/map14.jpg")}
      let a529 = {id: 529, exhibit: "1. プラネタリウムの歴史", 
                  url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/74", 
                  point: 0, basis: [], exhibit_tag: ['プラネタリウム', '歴史', '旧名古屋市科学館'], 
                  src: require("../static/A529-photo.jpg"), 
                  map: require("../static/map1.jpg")}
      let a534 = {id: 534, exhibit: "16. デジタルタイムカプセル", 
                  url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/67", 
                  point: 0, basis: [], exhibit_tag: ['タイムカプセル', '思い出', '旧名古屋市科学館'], 
                  src: require("../static/A534-photo.jpg"), 
                  map: require("../static/map16.jpg")}
      let a501 = {id: 501, exhibit: "2. 古代人の宇宙", 
                  url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/56", 
                  point: 0, basis: [], exhibit_tag: ['古代', '神話', '遺跡'], 
                  src: require("../static/A501-photo.jpg"), 
                  map: require("../static/map2.jpg")}
      let a502 = {id: 502, exhibit: "4. 天動説から地動説へ", 
                  url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/69", 
                  point: 0, basis: [], exhibit_tag: ['天動説', '地動説', 'ガリレオ'], 
                  src: require("../static/A502-photo.jpg"), 
                  map: require("../static/map4.jpg")}
      let a503 = {id: 503, exhibit: "3. 江戸時代の天文学", 
                  url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/62", 
                  point: 0, basis: [], exhibit_tag: ['江戸時代', '歴史', '古美術'], 
                  src: require("../static/A503-photo.jpg"), 
                  map: require("../static/map3.jpg")}
      let a504 = {id: 504, exhibit: "9. 光学望遠鏡のしくみ", 
                  url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/75", 
                  point: 0, basis: [], exhibit_tag: ['望遠鏡', 'レンズ', '鏡'], 
                  src: require("../static/A504-photo.jpg"), 
                  map: require("../static/map9.jpg")}
      let a505 = {id: 505, exhibit: "8. 望遠鏡をのぞいてみよう", 
                  url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/59", 
                  point: 0, basis: [], exhibit_tag: ['望遠鏡', '観測', '倍率'], 
                  src: require("../static/A505-photo.jpg"), 
                  map: require("../static/map8.jpg")}
      let a508 = {id: 508, exhibit: "10. さまざまな波長", 
                  url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/66", 
                  point: 0, basis: [], exhibit_tag: ['X線', '紫外線', '電波'], 
                  src: require("../static/A508-photo.jpg"), 
                  map: require("../static/map10.jpg")}
      let a509 = {id: 509, exhibit: "13. 分光観測とスペクトル", 
                  url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/70", 
                  point: 0, basis: [], exhibit_tag: ['光', '色', '波長'], 
                  src: require("../static/A509-photo.jpg"), 
                  map: require("../static/map13.jpg")}
      let a510 = {id: 510, exhibit: "12. 電波天文学", 
                  url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/61", 
                  point: 0, basis: [], exhibit_tag: ['電波', 'アンテナ', '観測'], 
                  src: require("../static/A510-photo.jpg"), 
                  map: require("../static/map12.jpg")}
      let a512 = {id: 512, exhibit: "15. X線天文学", 
                  url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/72", 
                  point: 0, basis: [], exhibit_tag: ['X線', '人工衛星', '望遠鏡'], 
                  src: require("../static/A512-photo.jpg"), 
                  map: require("../static/map15.jpg")}
      let a513 = {id: 513, exhibit: "17. 市街光と星空", 
                  url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/71", 
                  point: 0, basis: [], exhibit_tag: ['星空', '光害', '名古屋'], 
                  src: require("../static/A513-photo.jpg"), 
                  map: require("../static/map17.jpg")}
      let a515 = {id: 515, exhibit: "11. 宇宙線をみる", 
                  url: "https://heroku-app-mobileguide.herokuapp.com/exhibits/73", 
                  point: 0, basis: [], exhibit_tag: ['宇宙線', '放射線', 'イオン'], 
                  src: require("../static/A515-photo.jpg"), 
                  map: require("../static/map11.jpg")}

      let exhibits_list = [a518,a521,a522,a527,a529,a534,a501,a502,a503,a504,a505,a508,a509,a510,a512,a513,a515]
      let visitor_tags = this.visitor_tags

      // 「分析中...」の表示
      this.interval = setInterval(() => {
        if (this.value === 110) {
          this.page_count = -10000;
        }
        this.value += 5;
      }, 200);

      for(let vtag of visitor_tags){
        console.log(vtag);
        for(let exhibit of exhibits_list){
          console.log(exhibit);
          console.log(exhibit.exhibit_tag)
          if(exhibit.exhibit_tag.indexOf(vtag[0]) >= 0){
            exhibit.point += 1;
            exhibit.basis.push(vtag[0]);
          }
        }
      }

      exhibits_list.sort(function(a, b){
	     if (a.point < b.point) return 1;
	     if (a.point > b.point) return -1;
       return 0;
      });

      for(let i=0; i<recommend_num; i++){
        this.recommend_exhibits.push(exhibits_list[i])
      }

/*      gapi.load('client:auth2', ()=> {
        gapi.client.init({
          'apiKey': AIzaSyA8ehMZYKnaFZTSNfWcfQRwE6MVXLXrjSc,
          'clientId': 178072879019-hup08tcbt51k0ciir87bpobl72bja4tu.apps.googleusercontent.com,
          'scope': https://www.googleapis.com/auth/spreadsheets,
          'discoveryDocs': ['https://sheets.googleapis.com/$discovery/rest?version=v4'],
        })
      });

      gapi.client.sheets.spreadsheets.values.get({
        spreadsheetId: '12ZftsGFxqwAw278FWBvIjkICemr1BnG_lhKjMjdXTBE',
        range: 'sheet1' // rangeの指定(<YOUR Sheet Name>!A2:E)
      }).then((res)=>{
        console.log(res.result.value);
      });
*/
    },

    get_checkbox: function(event){
      for(let i=0; i<this.selected_items.length; i++){
        this.visitor_tags.push(this.selected_items[i])
      }
      this.page_count++;
      this.selected_items.length = 0; // 1ページ進むごとに配列を初期化
    },

    web_recommend: function (event) {
      let array = this.v_tags;
      let v_tags; //来館者タグ一覧
      const q_tag_num = 7 //一回あたりのタグ提示数
      let q_times = []
      let query = []

      // 来館者タグ候補をシャッフル
      for(let i = array.length-1; i>0; i--){
        let r = Math.floor(Math.random() * (i + 1));
        let tmp = array[i];
        array[i] = array[r];
        array[r] = tmp;
        v_tags = array
      }
      // 質問クエリの単語数を決める
      let tmp = v_tags.length%q_tag_num
      if (tmp<=Math.ceil(q_tag_num/2)){ // ceil: q_tag_num/2より大きい最小の整数を算出
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
        // 0から7(上記例の場合, 8は含まれない)までのv_tagsを切り抜いて質問作成
        query.push(v_tags.slice(0,q_times[i]))
        v_tags.splice(0,q_times[i])
      }

      this.data_query = query; // dataに入れ込んでhtmlに反映

      this.page_count++;
    }
  },

  created() {
    this.json = require('../static/new_recommend_deviation.json')
  },

  mounted() {
    this.page_count = 0;
  }
}

  // script(src='https://apis.google.com/js/api.js')

</script>

<!--<script async defer src="https://apis.google.com/js/api.js">
</script> -->

<style>
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
