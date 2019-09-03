<template>
  <v-app>
    <v-container>
      <v-layout align-start justify-center>
        <v-flex xs3><v-btn @click="locate">배치</v-btn></v-flex>
        <v-flex xs3><v-btn @click="space">빈칸</v-btn></v-flex>
        <v-flex xs3>
          <v-btn
            :disabled="loading"
            :loading="loading"
            @click="(loader = 'loading'), undo()"
            >되돌리기</v-btn
          >
        </v-flex>
        <v-flex xs3>
          <v-select
            v-model="cols"
            :items="[3, 4, 5, 6]"
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
      <v-layout class="row" fill-height>
        <!-- <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(1)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(2)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(3)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(4)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-3' : 'col-2'))" v-text="str(5)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(6)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(7)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(8)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(9)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-3' : 'col-2'))" v-text="str(10)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(11)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(12)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(13)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(14)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-3' : 'col-2'))" v-text="str(15)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(16)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(17)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(18)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(19)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-3' : 'col-2'))" v-text="str(20)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(21)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(22)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(23)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(24)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-3' : 'col-2'))" v-text="str(25)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(26)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(27)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(28)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(29)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-3' : 'col-2'))" v-text="str(30)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(31)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(32)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(33)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(34)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-3' : 'col-2'))" v-text="str(35)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(36)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(37)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(38)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(39)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-3' : 'col-2'))" v-text="str(40)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(41)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(42)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(43)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(44)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-3' : 'col-2'))" v-text="str(45)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(46)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(47)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(48)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-2' : 'col-2'))" v-text="str(49)"></div>
          <div :class="cols == 3 ? 'col-4' : (cols == 4 ? 'col-3' : (cols == 5 ? 'col-3' : 'col-2'))" v-text="str(50)"></div> -->
      </v-layout>
    </v-container>
  </v-app>
</template>

<script>
export default {
  data: () => ({
    loader: null,
    loading: false,
    num: "없음",
    docs: [],
    hist: [null],
    i: 0,
    n: 0,
    cnt: 0,
    cols: 5
  }),
  mounted: function() {
    // eslint-disable-next-line
    let docs = this.docs;
    // eslint-disable-next-line
    let i = this.i;
    // eslint-disable-next-line
    firebase
      .firestore()
      .collection("좌석배치기")
      .doc("18-204")
      .get()
      .then(function(doc) {
        if (doc.exists) {
          docs = doc.data();
          i = doc.data().names.length - 1;
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
