<template>
  <v-container class="justify-center">
    <div id="desc" class="text-center my-8">
      <div class="text-h2 font-weight-black ma-8">音ゲーマーレーティング</div>
      <div class="text-body-1 my-4">
        いろんな音ゲーを横断して音ゲー力の総合レーティングを計算したくて作りました。<br />
        思いつきで作ったサービスなのでそんなにまともな指標ではありません。<br />
        よりよい計算式を提案してくれる方募集中。GitHubでソースを公開しています。
      </div>
      <div class="text-body-2 ma-1">現在の計算式のバージョン: <span class="font-weight-bold">alpha (2024-03-xx)</span></div>
    </div>
    <div id="calc" class="text-center my-8">
      <v-sheet class="mx-auto" width="350">
        <v-form @submit.prevent="calculateRating">
          <v-text-field
            v-model="chuni"
            :rules="rules"
            label="CHUNITHM Rating (Max)"
            placeholder="16.00"
          ></v-text-field>
          <v-text-field
            v-model="sdvx"
            :rules="rules"
            label="SOUND VOLTEX VOLFORCE"
            placeholder="20.000"
          ></v-text-field>
          <v-btn class="mt-2" type="submit" block color="pink-darken-1">Submit</v-btn>
        </v-form>
      </v-sheet>
    </div>
    <Transition>
      <div id="result" class="text-center my-8 pa-6" v-if="overallRating > 0.0">
        <div id="overall-rating">
          <p class="text-h5">総合レーティング</p>
          <p class="text-h2 font-weight-black">{{ overallRating }}</p>
        </div>
        <div id="share" class="mt-8">
          <v-btn color="primary" @click="shareToTwitter" prepend-icon="mdi-twitter" size="large">Tweet</v-btn>
        </div>
      </div>
    </Transition>
  </v-container>
</template>

<script>
  export default {
    data: () => ({
      chuni: '',
      sdvx: '',
      rules: [
        value => {
          // 数値かどうかを検証
          if (parseFloat(value)) return true

          return '数値を入力してください。'
        },
      ],
      overallRating: 0.0,
    }),
    methods: {
      calculateRating() {
        const chuniRating = parseFloat(this.chuni) || 0.0
        const sdvxVF = parseFloat(this.sdvx) || 0.0
        this.overallRating = (chuniRating + sdvxVF) * 10
      },
      shareToTwitter() {
        const text = `わたしの音ゲーマーレーティングは${this.overallRating}！`
        const hashtags = 'otogamer_rating'
        const url = `https://twitter.com/intent/tweet?text=${text}&hashtags=${hashtags}`
        window.open(url, '_blank')
      },
    }
  }
</script>

<style>
.v-enter-active,
.v-leave-active {
  transition: opacity 1s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}
</style>
