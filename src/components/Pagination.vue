<template>
  <div class="pagination">
    <button @click="changeBtn">首页</button>
    <button @click="changeBtn">上一页</button>
    <button class="pagebtn" v-if="jduge">...</button>
    <button
      v-for="btn in pagebtns"
      @click="changeBtn(btn)"
      :class="[{currentPage: btn === currentPage},'pagebtn']"
    >{{btn}}</button>
    <button @click="changeBtn">下一页</button>
  </div>
</template>


<script>
import $ from "jquery";
export default {
  name: "pagination",
  data() {
    return {
      pagebtns: [1, 2, 3, 4, 5, "..."],
      currentPage: 1,
      jduge: false
    };
  },
  methods: {
    changeBtn(page) {
      // 点击上一页，下一页，首页
      if (typeof page !== "number") {
        switch (page.target.innerText) {
          case "上一页":
            $("button.currentPage")
              .prev()
              .click();
            break;
          case "下一页":
            $("button.currentPage")
              .next()
              .click();
            break;
          case "首页":
            this.pagebtns = [1, 2, 3, 4, 5, "..."];
            this.changeBtn(1);
            break;
        }
        return;
      }

      this.currentPage = page;

      if (page === this.pagebtns[4]) {
        // 移除第一个数字
        this.pagebtns.shift();
        // 添加后一个数字
        this.pagebtns.splice(4, 0, this.pagebtns[3] + 1);
      } else if (page === this.pagebtns[0] && page > 1) {
        // 先在第一个位置加一个
        this.pagebtns.unshift(this.pagebtns[0] - 1);
        // 把最后一个数字减去
        this.pagebtns.splice(5, 1);
      }
      if (page > 4) {
        this.jduge = true;
      } else {
        if (this.pagebtns[0] === 1) {
          this.jduge = false;
        } else {
          this.jduge = true;
        }
      }

      this.$emit("handleList", this.currentPage);
    }
  }
};
</script>

<style scoped>
.pagination {
    /* border: 1px solid #ccc; */
    background-color: #fff;
    padding: 10px;
}

button {
    padding: 5px;
    border: 1px solid #ccc;
    margin-right: 10px;
}
button:focus {
    outline: none;
}
.pagebtn {
    padding-left: 10px;
    padding-right: 10px;
}
.currentPage {
  color: #fff;
  background-color: #ccc;
}
</style>

