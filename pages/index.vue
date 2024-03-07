<template>
  <v-container class="justify-center">
    <div id="desc" class="text-center my-8">
      <div class="text-h2 font-weight-black ma-8">音ゲーマーレーティング</div>
      <div class="text-body-1 my-4">
        いろんな音ゲーを横断して音ゲー力の総合レーティングを計算したくて作りました。<br />
        思いつきで作ったサービスなのでそんなにまともな指標ではありません。<br />
        よりよい計算式を提案してくれる方募集中。GitHubでソースを公開しています。
      </div>
      <div class="text-body-2 ma-1">現在の計算式のバージョン: <span class="font-weight-bold">{{ ratingVersion }} ({{ updateDate }})</span></div>
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
      iidx: '',
      rules: [
        value => {
          // 数値かどうかを検証
          const v = parseFloat(value)
          if (!isNaN(v)) {
            if (v >= 0.0) return true
          }

          return '有効な数値を入力してください。'
        },
      ],
      overallRating: 0.0,
      ratingVersion: 'alpha',
      updateDate: '2024-03-07',
    }),
    methods: {
      calculateRating() {
        const chuniRatingRaw = parseFloat(this.chuni) || 0.0
        const sdvxVF = parseFloat(this.sdvx) || 0.0

        // CHUNITHM
        let chuniScore;
        if (chuniRatingRaw >= 17.00) {
          const r = (chuniRatingRaw - 17.00);
          chuniScore = 200 + 250 * r;
        } else if (chuniRatingRaw >= 16.00) {
          const r = (chuniRatingRaw - 16.00);
          chuniScore = 50 + 150 * r;
        } else {
          const r = chuniRatingRaw / 16.00;
          chuniScore = 50 * r;
        }

        // SDVX
        let sdvxScore;
        if (sdvxVF >= 20.000) {
          const r = (sdvxVF - 20.000);
          const f = 2;
          const bonus = f * Math.exp(r) - f;
          sdvxScore = 200 + 75 * r + bonus;
        } else if (sdvxVF >= 18.000) {
          const r = (sdvxVF - 18.000) / 2.0;
          sdvxScore = 50 + 150 * r;
        } else {
          const r = sdvxVF / 18.000;
          sdvxScore = 50 * r;
        }

        this.overallRating = (chuniScore + sdvxScore) * 25

        // 小数点以下第3位で四捨五入
        this.overallRating = Math.round(this.overallRating * 1000) / 1000
      },
      shareToTwitter() {
        const text = `わたしの音ゲーマーレーティングは${this.overallRating}！ (ver ${this.ratingVersion})`
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
