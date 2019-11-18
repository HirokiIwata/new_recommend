<template>
  <v-app>
    <v-navigation-drawer
      absolute
      temporary
      v-model="navBar"
    >
        <v-list dense class="pt-0">
          <router-link to="/">
            <v-list-tile>
              <v-list-tile-action>
                <v-icon>home</v-icon>
              </v-list-tile-action>
              <v-list-tile-content>
                <v-list-tile-title>HOME</v-list-tile-title>
              </v-list-tile-content>
            </v-list-tile>
          </router-link>

          <v-list-group
            dense class='pt-0'
            value="true"
          >
            <v-list-tile slot="activator">
              <v-list-tile-action>
                <v-icon>important_devices</v-icon>
              </v-list-tile-action>
              <v-list-tile-content>
                <v-list-tile-title>推薦システム</v-list-tile-title>
              </v-list-tile-content>
            </v-list-tile>

            <router-link to="/multiple_recommend">
              <v-list-tile>
                <v-list-tile-action>
                  <v-icon>web</v-icon>
                </v-list-tile-action>
                <v-list-tile-title>展示物推薦</v-list-tile-title>
              </v-list-tile>
            </router-link>

            <!-- <router-link to="/recommend_curator">
              <v-list-tile>
                <v-list-tile-action>
                  <v-icon>timeline</v-icon>
                </v-list-tile-action>
                <v-list-tile-title>時事推薦</v-list-tile-title>
              </v-list-tile>
            </router-link>

            <router-link to="/recommend_planet">
              <v-list-tile>
                <v-list-tile-action>
                  <v-icon>local_play</v-icon>
                </v-list-tile-action>
                <v-list-tile-title>プラネ推薦</v-list-tile-title>
              </v-list-tile>
            </router-link> -->
          </v-list-group>

          <v-list-group dense class='pt-0'>
            <!-- <router-link to="/exhibits"> -->
            <v-list-tile slot="activator">
              <v-list-tile-action>
                <v-icon>extension</v-icon>
              </v-list-tile-action>
              <v-list-tile-content>
                <v-list-tile-title>展示物一覧</v-list-tile-title>
              </v-list-tile-content>
            </v-list-tile>
            <!-- </router-link> -->
              <v-list-group
                prepend-icon="expand_more"
                sub-group
                >
                <v-list-tile slot="activator">
                  <v-list-tile-content>
                    <v-list-tile-title>宇宙の広がり</v-list-tile-title>
                  </v-list-tile-content>
                </v-list-tile>

                  <v-list-tile>
                    <v-list-tile-action>
                      <v-icon>chevron_right</v-icon>
                    </v-list-tile-action>
                      <v-list-tile-title>
                        <a
                          href="http://www.ncsm.city.nagoya.jp/exhibit_files/pdf/A516.pdf"
                          target="_blank"
                          >パワーズオブテン</a>
                      </v-list-tile-title>
                  </v-list-tile>
                  <v-list-tile>
                    <v-list-tile-action>
                      <v-icon>chevron_right</v-icon>
                    </v-list-tile-action>
                      <v-list-tile-title>
                        <a
                          href="http://www.ncsm.city.nagoya.jp/exhibit_files/pdf/A517.pdf"
                          target="_blank"
                          >地球</a>
                      </v-list-tile-title>
                  </v-list-tile>
                  <v-list-tile>
                    <v-list-tile-action>
                      <v-icon>chevron_right</v-icon>
                    </v-list-tile-action>
                      <v-list-tile-title>
                        <a
                          href="http://www.ncsm.city.nagoya.jp/exhibit_files/pdf/A518.pdf"
                          target="_blank"
                          >月の満ち欠け</a>
                      </v-list-tile-title>
                  </v-list-tile>
                  <v-list-tile>
                    <v-list-tile-action>
                      <v-icon>chevron_right</v-icon>
                    </v-list-tile-action>
                      <v-list-tile-title>
                        <a
                          href="http://www.ncsm.city.nagoya.jp/exhibit_files/pdf/A519.pdf"
                          target="_blank"
                          >月への挑戦</a>
                      </v-list-tile-title>
                  </v-list-tile>
                  <v-list-tile>
                    <v-list-tile-action>
                      <v-icon>chevron_right</v-icon>
                    </v-list-tile-action>
                      <v-list-tile-title>
                        <a
                          href="http://www.ncsm.city.nagoya.jp/exhibit_files/pdf/A520.pdf"
                          target="_blank"
                          >太陽系</a>
                      </v-list-tile-title>
                  </v-list-tile>
                  <v-list-tile>
                    <v-list-tile-action>
                      <v-icon>chevron_right</v-icon>
                    </v-list-tile-action>
                      <v-list-tile-title>
                        <a
                          href="http://www.ncsm.city.nagoya.jp/exhibit_files/pdf/A521.pdf"
                          target="_blank"
                          >惑星の動きと引力</a>
                      </v-list-tile-title>
                  </v-list-tile>
                  <v-list-tile>
                    <v-list-tile-action>
                      <v-icon>chevron_right</v-icon>
                    </v-list-tile-action>
                      <v-list-tile-title>
                        <a
                          href="http://www.ncsm.city.nagoya.jp/exhibit_files/pdf/A522.pdf"
                          target="_blank"
                          >惑星探査</a>
                      </v-list-tile-title>
                  </v-list-tile>
                  <v-list-tile>
                    <v-list-tile-action>
                      <v-icon>chevron_right</v-icon>
                    </v-list-tile-action>
                      <v-list-tile-title>
                        <a
                          href="http://www.ncsm.city.nagoya.jp/exhibit_files/pdf/A523.pdf"
                          target="_blank"
                          >宇宙を測る・宇宙を探る</a>
                      </v-list-tile-title>
                  </v-list-tile>
                  <v-list-tile>
                    <v-list-tile-action>
                      <v-icon>chevron_right</v-icon>
                    </v-list-tile-action>
                      <v-list-tile-title>
                        <a
                          href="http://www.ncsm.city.nagoya.jp/exhibit_files/pdf/A524.pdf"
                          target="_blank"
                          >星座を形づくる星々</a>
                      </v-list-tile-title>
                  </v-list-tile>
                  <v-list-tile>
                    <v-list-tile-action>
                      <v-icon>chevron_right</v-icon>
                    </v-list-tile-action>
                      <v-list-tile-title>
                        <a
                          href="http://www.ncsm.city.nagoya.jp/exhibit_files/pdf/A525.pdf"
                          target="_blank"
                          >星の世界</a>
                      </v-list-tile-title>
                  </v-list-tile>
                  <v-list-tile>
                    <v-list-tile-action>
                      <v-icon>chevron_right</v-icon>
                    </v-list-tile-action>
                      <v-list-tile-title>
                        <a
                          href="http://www.ncsm.city.nagoya.jp/exhibit_files/pdf/A526.pdf"
                          target="_blank"
                          >銀河の世界</a>
                      </v-list-tile-title>
                  </v-list-tile>
                  <v-list-tile>
                    <v-list-tile-action>
                      <v-icon>chevron_right</v-icon>
                    </v-list-tile-action>
                      <v-list-tile-title>
                        <a
                          href="http://www.ncsm.city.nagoya.jp/exhibit_files/pdf/A527.pdf"
                          target="_blank"
                          >銀河系と天の川</a>
                      </v-list-tile-title>
                  </v-list-tile>
                  <v-list-tile>
                    <v-list-tile-action>
                      <v-icon>chevron_right</v-icon>
                    </v-list-tile-action>
                      <v-list-tile-title>
                        <a
                          href="http://www.ncsm.city.nagoya.jp/exhibit_files/pdf/A528.pdf"
                          target="_blank"
                          >宇宙の果て</a>
                      </v-list-tile-title>
                  </v-list-tile>

                </v-list-group>

                <v-list-group
                  prepend-icon="expand_more"
                  sub-group
                  >
                  <v-list-tile slot="activator">
                    <v-list-tile-content>
                      <v-list-tile-title>天文学の歩み</v-list-tile-title>
                    </v-list-tile-content>
                  </v-list-tile>

                    <v-list-tile
                      v-for="item in astro_items"
                      :key="item.title"
                    >
                      <v-list-tile-action>
                        <v-icon>chevron_right</v-icon>
                      </v-list-tile-action>
                        <v-list-tile-title>
                          <a
                            :href="item.url"
                            target="_blank"
                            >{{ item.title }}</a>
                        </v-list-tile-title>
                    </v-list-tile>
                  </v-list-group>

                  <v-list-group
                    prepend-icon="expand_more"
                    sub-group
                    >
                    <v-list-tile slot="activator">
                      <v-list-tile-content>
                        <v-list-tile-title>プラネタリウムの歴史</v-list-tile-title>
                      </v-list-tile-content>
                    </v-list-tile>

                      <v-list-tile
                        v-for="item in planet_items"
                        :key="item.title"
                      >
                        <v-list-tile-action>
                          <v-icon>chevron_right</v-icon>
                        </v-list-tile-action>
                          <v-list-tile-title>
                            <a
                              :href="item.url"
                              target="_blank"
                              >{{ item.title }}</a>
                          </v-list-tile-title>
                      </v-list-tile>
                    </v-list-group>
          </v-list-group>

          <v-list-tile>
            <v-list-tile-action>
              <v-icon>info</v-icon>
            </v-list-tile-action>
            <v-list-tile-content>
              <v-list-tile-title><a
                href="http://mdg.si.i.nagoya-u.ac.jp/~yasuda/"
                target="_blank"
                >当研究室について</a></v-list-tile-title>
            </v-list-tile-content>
          </v-list-tile>

        </v-list>
      </v-navigation-drawer>
    <v-toolbar
      dark
      color="primary"
      clipped-left
      fixed
      app
    >
      <v-toolbar-side-icon @click.stop="navBar = !navBar"></v-toolbar-side-icon>
      <v-toolbar-title class="white--text">Exhibition Guide</v-toolbar-title>
      <v-spacer></v-spacer>
      <!-- <v-btn icon>
        <v-icon>search</v-icon>
      </v-btn>
      <v-btn icon>
        <v-icon>apps</v-icon>
      </v-btn>
      <v-btn icon>
        <v-icon>refresh</v-icon>
      </v-btn>
      <v-btn icon>
        <v-icon>more_vert</v-icon>
      </v-btn> -->

    </v-toolbar>
    <v-content>
      <v-container fluid fill-height>
        <v-fade-transition mode="out-in">
          <router-view></router-view>
        </v-fade-transition>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>

  export default {
    name: 'app',
    data () {
      return {
          navBar:null,
          planet_items: [
            {title: 'プラネタリウムの歴史', url: 'http://www.ncsm.city.nagoya.jp/exhibit_files/pdf/A529.pdf' },
            {title: 'アイジンガー・プラネタリウム', url: 'http://www.ncsm.city.nagoya.jp/exhibit_files/pdf/A530.pdf' },
            {title: 'ピンホール式プラネタリウム', url: 'http://www.ncsm.city.nagoya.jp/exhibit_files/pdf/A531.pdf' },
            {title: 'ツァイスⅣ型プラネタリウム', url: 'http://www.ncsm.city.nagoya.jp/exhibit_files/pdf/A532.pdf' },
            {title: 'ファイバー式プラネタリウム', url: 'http://www.ncsm.city.nagoya.jp/exhibit_files/pdf/A533.pdf' },
            {title: 'デジタルタイムカプセル', url: 'http://www.ncsm.city.nagoya.jp/exhibit_files/pdf/A534.pdf' },
            {title: '星座早見盤', url: 'http://www.ncsm.city.nagoya.jp/exhibit_files/pdf/A535.pdf' },
            {title: 'デジタル式プラネタリウム', url: 'http://www.ncsm.city.nagoya.jp/exhibit_files/pdf/A536.pdf' }
          ],
          astro_items: [
            {title: '古代人の宇宙', url: 'http://www.ncsm.city.nagoya.jp/exhibit_files/pdf/A501.pdf'},
            {title: '天動説から地動説へ', url: 'http://www.ncsm.city.nagoya.jp/exhibit_files/pdf/A502.pdf'},
            {title: '江戸時代の天文学', url: 'http://www.ncsm.city.nagoya.jp/exhibit_files/pdf/A503.pdf'},
            {title: '光学望遠鏡の仕組み', url: 'http://www.ncsm.city.nagoya.jp/exhibit_files/pdf/A504.pdf'},
            {title: '望遠鏡をのぞいてみよう', url: 'http://www.ncsm.city.nagoya.jp/exhibit_files/pdf/A505.pdf'},
            {title: '65cm望遠鏡', url: 'http://www.ncsm.city.nagoya.jp/exhibit_files/pdf/A506.pdf'},
            {title: '望遠鏡の大きさくらべ', url: 'http://www.ncsm.city.nagoya.jp/exhibit_files/pdf/A507.pdf'},
            {title: 'さまざまな波長', url: 'http://www.ncsm.city.nagoya.jp/exhibit_files/pdf/A508.pdf'},
            {title: '分光観測とスペクトル', url: 'http://www.ncsm.city.nagoya.jp/exhibit_files/pdf/A509.pdf'},
            {title: '電波天文学', url: 'http://www.ncsm.city.nagoya.jp/exhibit_files/pdf/A510.pdf'},
            {title: '赤外線天文学', url: 'http://www.ncsm.city.nagoya.jp/exhibit_files/pdf/A511.pdf'},
            {title: 'X線天文学', url: 'http://www.ncsm.city.nagoya.jp/exhibit_files/pdf/A512.pdf'},
            {title: '市街光と星空', url: 'http://www.ncsm.city.nagoya.jp/exhibit_files/pdf/A513.pdf'},
            {title: '星のまたたき', url: 'http://www.ncsm.city.nagoya.jp/exhibit_files/pdf/A514.pdf'},
            {title: '宇宙線をみる', url: 'http://www.ncsm.city.nagoya.jp/exhibit_files/pdf/A515.pdf'}
          ]
      }
    },
    mounted: function () {

    },
    destroyed: function () {

    }
  }
</script>

<style>
  a {
    text-decoration: none;
  }

</style>
