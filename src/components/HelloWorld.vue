<template>
  <div class="hello">
    <h1>細胞自動機</h1>
    <table class = "in" v-for="(r,i) in ran(8)" :key="r" >
      <tr>
        <td>{{comp(7-i)[2] || 0}}</td>
        <td>{{comp(7-i)[1] || 0}}</td>
        <td>{{comp(7-i)[0] || 0}}</td>
      </tr>
      <tr>
        <td><input type="checkbox" name="" v-model="rList[rList.length - i - 1]" @change="reset"/></td>
      </tr>
    </table>
    <h3>Rule {{rule}}</h3>
    <div v-for="(c, idx) in cells" :key = "idx">
      <div class = "cell" :class = "{black: item == 1}" v-for="(item,i) in c" :key="i">{{item || 0}}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      rList: [false,true,true,true,true,false,false,false],
      l: 10,
      cells: [[]]
    }
  },
  computed: {
    rule() {
      var ans = 0
      for (var i = 0; i < this.rList.length; i++) {
        ans += (this.rList[i] ? 1 : 0) * (2 ** i)
      }
      return ans
    }
  },
  mounted() {
    console.log(window.innerWidth)
    if (window.innerWidth <= 600) {
      this.l = 6;
    }
    this.start()
    setInterval(this.next, 500)
  },
  methods: {
    comp(i) {
      var ans = []
      while (i > 0) {
        ans.push(i % 2)
        i = Math.floor(i / 2)
      }
      return ans
    },
    reset() {
      this.start()
    },
    ran(k) {
      var list = []
      for (var i = 0; i < k; i++) {
        list[i] = i
      }
      return list
    },
    getRule(r, a, s, d) {
      a = a || 0
      s = s || 0
      d = d || 0
      var k = r
      var idx = 0
      var list = []
      while (k > 0) {
        list[idx] = k % 2
        k = Math.floor(k / 2)
        idx++
      }
      var ans = list[a + s * 2 + d*4]
      return ans
    },
    start() {
      this.cells = [[]]
      console.log(this.ran(this.l))
      this.cells[0] = this.ran(this.l).map(function () {
        return Math.floor(Math.random()*2)
      })
    },
    next() {
      var vm = this
      var list = 
      this.cells[this.cells.length - 1];
      this.cells[this.cells.length] = this.ran(this.l);
      for (var i = 0; i < list.length; i++) {
        var a = list[i - 1] || 0;
        var s = list[i];
        var d = list[i+1] || 0;
        this.cells[this.cells.length - 1][i] = vm.getRule(vm.rule, a, s, d)
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.in {
  display: inline-table;
  border: 1px solid black;
}

.cell {
  display: inline-block;
  border: 1px solid black;
  font-size: 22px;
  width: 2em;
  height: 2em;
  text-align: center;
  line-height: 2em;
  background-color: white;
  color: white;
}

.cell.black {background-color: black; color: black}

</style>
