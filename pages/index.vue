<template>
  <v-responsive>
    <section>
      <v-parallax :src="background" height="450">
        <v-layout
          column
          align-center
          justify-center
          class="white--text"
        >
          <!-- <img src="logo.gif" alt="logo" height="60"> -->
          <h1 class="white--text mb-2 display-1 text-xs-center"><b>展示コンシェルジュ</b></h1>
          <div class="subheading mb-3 text-xs-center"><b>興味に合わせて展示を巡ろう！</b></div>
          <v-btn
            class="mt-5"
            color="blue lighten-2"
            dark
            large
            href="/multi_recommend"
          >
            推薦ページへ行く
          </v-btn>
        </v-layout>
      </v-parallax>
    </section>

    <section>
      <v-layout
        column
        wrap
        class="my-5"
        align-center
      >
        <v-flex xs12 sm4 class="my-3">
          <div class="text-xs-center">
            <h2 class="headline">コンテンツ一覧</h2>
            <!-- <span class="subheading">
              Cras facilisis mi vitae nunc
            </span> -->
          </div>
        </v-flex>
        <v-flex xs12>
          <v-container grid-list-xl>
            <v-layout row wrap align-center justify-center justify-content >
              <v-card flat class="transparent">
                <router-link to="/multi_recommend">
                  <v-card-text class="text-xs-center">
                    <v-icon x-large class="blue--text text--lighten-2">build</v-icon>
                  </v-card-text>
                </router-link>
                <router-link to="/multi_recommend">
                  <v-card-title primary-title class="layout justify-center">
                    <div class="headline text-xs-center">展示物推薦</div>
                  </v-card-title>
                </router-link>
                <v-card-text>
                  提示される言葉の中から<br>気になるものを選ぶことで<br>あなたの興味に合わせた<br>展示物をオススメします！
                </v-card-text>
              </v-card>
              <v-card flat class="transparent">
                <v-card-text class="text-xs-center">
                  <a
                  href="https://heroku-app-mobileguide.herokuapp.com/exhibits/index"
                  target="_blank">
                  <v-icon x-large class="blue--text text--lighten-2">color_lens</v-icon>
                  </a>
                </v-card-text>
                <v-card-title primary-title class="layout justify-center">
                  <div class="headline text-xs-center"><a
                  href="https://heroku-app-mobileguide.herokuapp.com/exhibits/index"
                  target="_blank">展示物一覧</a></div>
                </v-card-title>
                <v-card-text>
                  天文に関する展示物の解説を<br>デジタル形式で閲覧できます．<br>
                  展示室には書かれていない<br>ここだけの情報も・・・！
                </v-card-text>
              </v-card>
              <v-card flat class="transparent">
                <v-card-text class="text-xs-center">
                  <a
                  href="http://www.ncsm.city.nagoya.jp/study/astro/index.html"
                  target="_blank">
                    <v-icon x-large class="blue--text text--lighten-2">flash_on</v-icon>
                  </a>
                </v-card-text>
                <v-card-title primary-title class="layout justify-center">
                  <a
                  href="http://www.ncsm.city.nagoya.jp/study/astro/index.html"
                  target="_blank">
                    <div class="headline">天文情報 | 天文データ</div>
                  </a>
                </v-card-title>
                <v-card-text>
                  名古屋市科学館が公開している<br>天文情報にアクセスできます．<br>
                  スーパーカミオカンデの内部映像など<br>貴重な映像が盛りだくさん！
                </v-card-text>
              </v-card>

              <v-flex xs12 md4>
              </v-flex>
            </v-layout>
          </v-container>
        </v-flex>
      </v-layout>
    </section>

    <section>
      <v-parallax :src="background" height="380">
        <v-layout column align-center justify-center>
          <div class="headline white--text mb-3 text-xs-center"><b>Exhibit Recommendation System</b></div>
          <em><b>Let's see the exhibition<br>according to your interests!<br>※English ver. comming soon...</b></em>
          <v-btn
            class="mt-5"
            color="blue lighten-2"
            dark
            large
            href="/multi_recommend"
          >
            Get Started
          </v-btn>
        </v-layout>
      </v-parallax>
    </section>

    <section>
      <v-container grid-list-xl>
        <v-layout row wrap justify-center class="my-5">
          <v-flex xs12 sm4>
            <v-card flat class="transparent">
              <v-card-text class="text-xs-center">
                <a
                href="http://www.mdg.ss.is.nagoya-u.ac.jp/"
                target="_blank">
                  <v-icon x-large class="blue--text text--lighten-2">category</v-icon>
                </a>
              </v-card-text>
              <v-card-title primary-title class="layout justify-center">
                <div class="headline"><a
                href="http://www.mdg.ss.is.nagoya-u.ac.jp/"
                target="_blank">当研究室について</a></div>
              </v-card-title>
              <v-card-text>
                インターネットに代表される情報通信ネットワークにおける新技術の調査研究と、
                それらがもたらす新しい社会の在り方について教育、文化、経済など多方面から調査研究を行っています。
              </v-card-text>
            </v-card>
          </v-flex>
        </v-layout>
      </v-container>
    </section>

    <v-footer color="blue darken-2">
      <v-layout row wrap align-center>
        <v-flex xs12>
          <div class="white--text ml-3">
            by 名古屋大学大学院情報学研究科 安田研究室
          </div>
        </v-flex>
      </v-layout>
    </v-footer>

  </v-responsive>
</template>



<script src="assets/style_index2.css"></script>

<script>
export default {
  data(){
    return{
      background: require("../static/plane_l.jpg")
    }
  }
}
</script>

<style>

</style>
