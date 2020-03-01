<template>
  <div>
    <el-row>
      <el-col :span="12">
        <div class="grid-content bg-purple">
          <transition name="el-fade-in-linear">
            <img src="../assets/my_bu.png" @click="choose(0)" v-show="showMy[0]">
          </transition>
          <transition name="el-fade-in-linear">
            <img src="../assets/my_chui.png" @click="choose(1)" v-show="showMy[1]">
          </transition>
          <transition name="el-fade-in-linear">
            <img src="../assets/my_jiandao.png" @click="choose(2)" v-show="showMy[2]">
          </transition>
          <p></p>
          <img src="../assets/my_bu.png" v-show="showBu==0">
          <img src="../assets/my_chui.png" v-show="showBu==1">
          <img src="../assets/my_jiandao.png" v-show="showBu==2">
        </div>
        <el-button type="primary" @click="newRound()">再来一次</el-button>

      </el-col>
      <el-col :span="12">
        <div class="grid-content bg-purple-light">
          <el-table
            :data="tableResult"
            height="700"
            border
            style="width: 100%">
            <el-table-column
              prop="my"
              label="徐蔚澜大王"
              width="180">
              <template slot-scope="scope">
                <span style="margin-left: 10px"><img :src="op[scope.row.my]" class="small" alt=""></span>
              </template>
            </el-table-column>
            <el-table-column
              prop="op"
              label="阿尔法牛"
              width="180">
              <template slot-scope="scope">
                <span style="margin-left: 10px"><img :src="op[scope.row.op]" class="small" alt=""></span>
              </template>
            </el-table-column>
            <el-table-column
              prop="ru"
              label="结果">
              <template slot-scope="scope">
                <span style="margin-left: 10px">{{ scope.row.re }}</span>
              </template>
            </el-table-column>
          </el-table>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
  export default {
    name: 'Index',
    data() {
      return {
        showBu: 0,
        showMy: [1, 1, 1],
        randIntval: 0,
        results: [],
        choosed: false,
        resultsArr: [0, 0, 0],
        op: [require("../assets/op_bu.png"), require("../assets/op_chui.png"), require("../assets/op_jiandao.png")],
        tableResult: []
      }
    },
    mounted() {
      this.randBu();
    },
    watch: {
      results: {
        handler(newValue, oldValue) {

        },
        deep: true
      },
    },
    methods: {
      winOrLose(val1, val2) {
        if (val2 == val1) {
          return '平局';
        } else if (val2 - val1 == 1) {
          return '胜利'
        } else if (val2 - val1 == -1) {
          return '失败'
        } else if (val2 - val1 == 2) {
          return '失败'
        } else if (val2 - val1 == -2) {
          return '胜利'
        }
      },
      newRound() {
        this.showMy = [1, 1, 1];
        this.randBu();
      },
      choose(index) {
        let arr = {};
        let showBu = this.showBu;
        let winOrLose;
        let op = this.op;
        let resultsArr = this.resultsArr;
        this.showMy = [0, 0, 0];
        this.showMy[index] = 1;
        if (this.randIntval == 0) {
          //
        } else {
          clearInterval(this.randIntval);
          this.randIntval = 0;
        }
        winOrLose = this.winOrLose(index, showBu);
        if (winOrLose == '平局') {
          resultsArr[0] += 1;
        } else if (winOrLose == '胜利') {
          resultsArr[1] += 1;
        } else {
          resultsArr[2] += 1;
        }
        arr = {"my": index, "op": showBu, "re": winOrLose};
        this.results.push(arr);
        this.tableResult.push(arr);
      },
      randBu() {
        let that = this;
        this.randIntval = setInterval(function () {
          that.showBu = Math.floor(Math.random() * 3)
        }, 20)
      }
    },
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .small {
    width: 64px;
    height: 64px;
  }

  .el-table .warning-row {
    background: oldlace;
  }

  .el-table .success-row {
    background: #f0f9eb;
  }
</style>
