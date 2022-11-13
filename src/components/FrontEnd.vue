<template>
  <v-app id="inspire">
    <v-system-bar app color="red">
      <v-spacer></v-spacer>
      <v-icon>mdi-microsoft-windows</v-icon>
      <v-icon>mdi-circle</v-icon>
      <v-icon>mdi-triangle</v-icon>
    </v-system-bar>
    <v-app-bar
        class="mt-4" max-height="50"
        color="deep-purple"
        dark
    >
      <v-app-bar-nav-icon @click="drawer = true"></v-app-bar-nav-icon>
      <v-toolbar-title>Vuetify入門で作ったもの</v-toolbar-title>
    </v-app-bar>

    <v-navigation-drawer
        v-model="drawer"
        absolute
        temporary
    >
      <v-list
          nav
          dense
      >
        <v-list-item-group
            v-model="group"
            active-class="deep-purple--text text--accent-4"
        >
          <v-list-item href="https://kikuichige.com" target="_blank">
            <v-list-item-icon>
              <v-icon>mdi-home</v-icon>
            </v-list-item-icon>
            <v-list-item-title>イチゲブログ</v-list-item-title>
          </v-list-item>
          <v-list-item href="https://vuetifyjs.com/ja/" target="_blank">
            <v-list-item-icon>
              <v-icon>mdi-account</v-icon>
            </v-list-item-icon>
            <v-list-item-title>Vuetify</v-list-item-title>
          </v-list-item>
        </v-list-item-group>
      </v-list>
    </v-navigation-drawer>

    <v-main class="pink lighten-5">
      <v-container>
        <CCalouse v-bind:moji="message"></CCalouse>
        <v-row>
          <v-col cols="12" md="4">
            <v-sheet height="150" color="orange lighten-2">
              <div class="text-center">
                <v-text-field
                    v-model="message"
                    label="メッセージを入力してください"
                    filled>
                </v-text-field>
                <p>カルーセルに表示します</p>
              </div>
            </v-sheet>
          </v-col>
          <!-- じゃんけん -->
          <v-col cols="12" md="4">
            <v-sheet height="300" color="cyan accent-1">
              <div class="text-center">
                <h1>ジャンケン ポン！</h1>
                <!-- v-on:click="関数('データ')"   関数はscriptのmethods:で定義 -->
                <v-btn color="warning" dark v-on:click="janken('goo')">グー</v-btn>
                <v-btn color="error" dark v-on:click="janken('choki')">チョキ</v-btn>
                <v-btn color="success" dark v-on:click="janken('paa')">パー</v-btn>
                <v-img max-height="150" contain v-bind:src="myHandImage"></v-img>
                <h1>{{result}}</h1>
              </div>
            </v-sheet>
          </v-col>
          <v-col cols="12" md="4">
            <v-sheet height="250" color="cyan accent-1">
              <h1 class="text-center">相手</h1>
              <!-- v-imgはsrcに指定した画像を表示 v-bind:によりenemyHandImage変数の中身がsrcに代入される -->
              <v-img max-height="200" contain v-bind:src="enemyHandImage"></v-img></v-sheet>
          </v-col>
          <!-- じゃんけんend -->
        </v-row>
      </v-container>
    </v-main>
    <!-- フッター -->
    <v-footer padless>
      <v-col
          class="text-center"
          cols="12"
      >
        <p>Copyright (c) 2022 イチゲブログ</p><v-btn href="https://opensource.org/licenses/mit-license.php">Released under the MIT license</v-btn>
      </v-col>
    </v-footer>
    <!-- フッターend -->
  </v-app>
</template>

<script>
import CCalouse from './CarouselComp.vue'
export default {
  components: {
    CCalouse,
  },
  data () {
    return {
      drawer: null,
      message:"",
      enemyHand:'',
      /*じゃんけん */
      hand:['goo','choki','paa'],
      enemyHandImage: '',
      myHandImage: '',
      result:'',
      goo:'https://2.bp.blogspot.com/-VhlO-Yfjy_E/Uab3z3RNJQI/AAAAAAAAUVg/fX8VnSVDlWs/s800/janken_gu.png',
      choki:'https://4.bp.blogspot.com/-__yEIXe5SxU/Uab3zO7BB2I/AAAAAAAAUVI/MYg6TVeiv-Y/s800/janken_choki.png',
      paa:'https://3.bp.blogspot.com/-qZtyoue9xKs/Uab30IG0Q5I/AAAAAAAAUVk/qnH8a2OgrvI/s800/janken_pa.png'
    }},
  methods:{
//ジャンケン関数  myHandには'goo''choki''paa'が送られてくる。
    janken: function(myHand){
//乱数により敵の手を決める
      var index = Math.floor(Math.random() * Math.floor(3));//0から3までの乱数が得られる
      this.enemyHand = this.hand[index];
//敵の手表示
      if(this.enemyHand === 'goo'){
        this.enemyHandImage= this.goo;
      } else if(this.enemyHand === 'choki'){
        this.enemyHandImage=this.choki;
      } else if(this.enemyHand === 'paa'){
        this.enemyHandImage=this.paa;
      }
//自分の手表示
      if(myHand === 'goo'){
        this.myHandImage= this.goo;
      } else if(myHand === 'choki'){
        this.myHandImage=this.choki;
      } else if(myHand === 'paa'){
        this.myHandImage=this.paa;
      }
//勝負判定
      if(myHand === this.enemyHand){
        this.result='引き分け'
      }
      if(this.enemyHand === 'goo' && myHand === 'paa'){
        this.result='勝ち'
      }
      if(this.enemyHand === 'choki' && myHand === 'goo'){
        this.result='勝ち'
      }
      if(this.enemyHand === 'paa' && myHand === 'choki'){
        this.result='勝ち'
      }
      if(this.enemyHand === 'goo' && myHand === 'choki'){
        console.log('負け');
        this.result='負け'
      }
      if(this.enemyHand === 'choki' && myHand === 'paa'){
        this.result='負け'
      }
      if(this.enemyHand === 'paa' && myHand === 'goo'){
        this.result='負け'
      }
    }
  }
}
</script>