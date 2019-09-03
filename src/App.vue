<template>
  <v-app>
    <v-container>
      <v-layout align-start justify-center>
        <v-flex xs3><v-btn @click="locate">배치</v-btn></v-flex>
        <v-flex xs2>
          <v-btn icon color="blue" @click="space">
            <v-icon>mdi-keyboard-space</v-icon>
          </v-btn>
        </v-flex>
        <v-flex xs2>
          <v-btn
            icon
            color="blue"
            :disabled="loading"
            :loading="loading"
            @click="(loader = 'loading'), undo()"
          >
            <v-icon>mdi-undo</v-icon>
          </v-btn>
        </v-flex>
        <v-flex xs3>
          <v-select
            v-model="cols"
            :items="[1, 2, 3, 4, 5, 6]"
            label="열 수"
          ></v-select>
        </v-flex>
        <v-flex xs3>
          <v-select
            v-model="num"
            :items="['없음', 'ASC', 'ID']"
            label="번호"
          ></v-select>
        </v-flex>
      </v-layout>
      <v-layout fill-height>
        <span v-for="k of i" :key="k">
          <v-row v-if="cols == 1">
            <v-col cols="12" v-text="str(k)"></v-col>
          </v-row>
          <!-- <span v-if="cols == 5 && k % 5 == 0">
            <v-flex xs2 v-text="str(k)"></v-flex>
          </span>
          <span v-else>
            <v-flex
              :xs12="cols == 1"
              :xs6="cols == 2"
              :xs4="cols == 3"
              :xs3="cols == 4 || cols == 5"
              v-text="str(k)"
            ></v-flex>
          </span> -->
        </span>
      </v-layout>
    </v-container>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      loader: null,
      loading: false,
      num: "없음",
      docs: [],
      hist: [null],
      i: 0,
      k: 0,
      n: 0,
      cnt: 0,
      cols: 5
    };
  },
  mounted() {
    // eslint-disable-next-line
    firebase
      .firestore()
      .collection("좌석배치기")
      .doc("18-204")
      .get()
      .then(doc => {
        if (doc.exists) {
          this.docs = doc.data();
          this.i = doc.data().names.length - 1;
        } else {
          location.replace("/");
          alert("문서가 없습니다!");
        }
      })
      .catch(function(error) {
        document.write(error);
      });
  },
  watch: {
    loader() {
      const l = this.loader;
      this[l] = !this[l];
      setTimeout(() => (this[l] = false), 700);
      this.loader = null;
    }
  },
  methods: {
    str(n) {
      console.log("i", this.i);
      console.log("hist", this.hist);
      // 번호 포맷
      if (this.hist[n]) {
        if (this.num == "ASC") {
          return (n < 10 ? "0" + n : n) + " " + this.docs.names[this.hist[n]];
        } else if (this.num == "ID") {
          return (
            (this.hist[n] < 10 ? "0" + this.hist[n] : this.hist[n]) +
            " " +
            this.docs.names[this.hist[n]]
          );
        } else {
          return this.docs.names[this.hist[n]];
        }
      }
    },
    locate() {
      while (this.cnt <= this.i) {
        this.n = Math.floor(Math.random() * this.i + 1);
        if (this.hist.indexOf(this.n) == -1) {
          this.hist.push(this.n);
          this.cnt++;
          return 0;
        }
      }
    },
    space() {
      this.hist.push(null);
    },
    undo() {
      if (this.hist[this.hist.length - 1] !== null) this.cnt--;
      this.hist.pop();
    }
  }
};
</script>
