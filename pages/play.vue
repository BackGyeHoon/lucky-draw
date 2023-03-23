<template>
  <section class="container">
    <div class="content">
      <header class="header">
        <h2 class="header-title">{{ getTitle }}</h2>
        <p class="draw-title" v-if="drawItem">
          상품 : <span class="bold">{{ drawItem }}</span>
        </p>
      </header>
      <div class="body2">
        <div class="search2">
          <el-input
            placeholder="드로우 품목을 입력해주세요 !"
            v-model="input2"
          />
          <el-button
            type="primary"
            @click="addItem()"
            @keydown.enter="addItem()"
            >등록</el-button
          >
        </div>
        <div class="search">
          <el-input
            placeholder="참여자 이름을 입력해주세요 !"
            v-model="input1"
          />
          <el-button type="primary" @click="invite()" @keydown.enter="invite()"
            >등록</el-button
          >
        </div>
        <div class="list" v-if="filterDatas.length !== 0">
          <el-card class="box-card" :class="{ _more: filterDatas.length > 7 }">
            <h2 class="list-title">
              이름 (총 인원: {{ filterDatas.length }} 명 )
            </h2>
            <div
              v-for="(item, index) in filterDatas"
              :key="item.index"
              class="text item"
            >
              {{ index + 1 }}. {{ item }}
            </div>
            <div style="display: flex; padding-top: 30px">
              <el-button
                type="success"
                icon="el-icon-switch-button"
                @click="start()"
                v-loading.fullscreen.lock="fullscreenLoading"
                >추첨하기</el-button
              >
              <el-button
                type="danger"
                icon="el-icon-delete"
                @click="reset()"
              ></el-button>
            </div>
            <el-button
              type="info"
              class="lucky-button"
              icon="el-icon-search"
              @click="openLuckyModal()"
              >당첨자 리스트</el-button
            >
          </el-card>
        </div>
      </div>
      <div class="footer">
        <el-button
          type="danger"
          icon="el-icon-arrow-left"
          class="back-button"
          @click="backPage()"
          >뒤로가기</el-button
        >
      </div>
    </div>
    <div class="modal" v-if="isLuckyListDialog">
      <div class="dim"></div>
      <div class="modal-content">
        <ul class="list">
          <li class="lucky-item lucky-item-first">
            <h2 class="item-title">당첨자</h2>
            <p class="item-value">당첨품목</p>
          </li>
          <li
            class="lucky-item"
            v-for="(item, index) in luckys"
            :key="item.index"
          >
            <h2 class="item-title">{{ item.value }}</h2>
            <p class="item-value">{{ item.title }}</p>
          </li>
        </ul>
        <el-button
          type="success"
          class="lucky-button"
          @click="closeLuckyModal()"
          >확인</el-button
        >
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: "PlayPage",
  data() {
    return {
      isLuckyListDialog: false,
      input1: "",
      input2: "",
      datas: [],
      drawItem: "",
      filterDatas: [],
      result: "",
      luckys: [],
      fullscreenLoading: false,
    };
  },
  computed: {
    getTitle() {
      return this.$route.query.title;
    },
  },
  mounted() {
    this.isTitle();
  },
  methods: {
    isTitle() {
      if (!this.$route.query.title) {
        this.$router.push("/");
      }
    },
    backPage() {
      this.$router.push("/");
      localStorage.clear();
    },
    addItem() {
      this.drawItem = this.input2;
    },
    openLuckyModal() {
      this.isLuckyListDialog = true;
    },
    closeLuckyModal() {
      this.isLuckyListDialog = false;
    },
    async invite() {
      if (this.input1 === "") {
        this.$alert("이름을 입력해주세요", "알림", {
          confirmButtonText: "OK",
        });
        return false;
      } else {
        await this.datas.push(this.input1);
        const array = this.datas;
        this.filterDatas = [...new Set(array)];
        this.input1 = "";
      }
    },
    reset() {
      this.$confirm("정말 삭제하시겠습니까?", "알림", {
        confirmButtonText: "네",
        cancelButtonText: "아니오",
        type: "warning",
      })
        .then(() => {
          this.$message({
            type: "success",
            message: "삭제 되었습니다.",
          });
          this.datas = [];
          this.filterDatas = [];
        })
        .catch(() => {});
    },
    start() {
      if (this.datas.length === 1) {
        this.$alert("2명이상 입력해주세요 !", "알림", {
          confirmButtonText: "OK",
        });
        return false;
      } else {
        this.fullscreenLoading = true;
        setTimeout(() => {
          this.result =
            this.filterDatas[
              Math.floor(Math.random() * this.filterDatas.length)
            ];
          this.fullscreenLoading = false;
          this.$alert(this.result, "당첨자!", {
            confirmButtonText: "OK",
            callback: (action) => {},
          });
          this.luckys.push({
            title: this.drawItem,
            value: this.result,
          });
        }, 1000);
      }
    },
  },
};
</script>
<style>
body,
h1,
h2 {
  margin: 0;
  padding: 0;
}

body {
  background: radial-gradient(
    circle,
    rgba(238, 174, 202, 1) 0%,
    rgba(148, 187, 233, 1) 100%
  );
}

.modal {
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
}

.dim {
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.7);
}

.modal-content {
  position: absolute;
  width: 400px;
  height: auto;
  min-height: 200px;
  background-color: #fff;
  border-radius: 16px;
  margin: 0 auto;
  left: 50%;
  top: 50%;
  transform: translateX(-50%) translateY(-50%);
  box-shadow: 0px 4px 9px rgba(0, 0, 0, 0.05);
}

.list {
  margin: 0;
  padding: 0;
  list-style: none;
  height: 350px;
  overflow: scroll;
}

.lucky-item {
  padding: 0;
  display: flex;
  align-items: center;
}

.lucky-item-first {
  border-bottom: 1px solid #d9d9d9;
}

.item-title {
  width: 50%;
  text-align: center;
  font-size: 18px;
  border-right: 1px solid #d9d9d9;
}

.item-value {
  width: 50%;
  text-align: center;
  font-size: 18px;
}

.container {
  position: relative;
  width: 100%;
  height: 100vh;
}

.content {
  position: absolute;
  width: 350px;
  height: auto;
  min-height: 200px;
  background-color: #fff;
  border-radius: 16px;
  margin: 0 auto;
  left: 50%;
  top: 50%;
  transform: translateX(-50%) translateY(-50%);
  box-shadow: 0px 4px 9px rgba(0, 0, 0, 0.05);
}

.header {
  width: 100%;
  height: auto;
  text-align: center;
}

.header-title {
  padding: 15px 0;
}

.body2 {
  padding: 0 30px 60px;
}

.search {
  display: flex;
}

.search2 {
  display: flex;
  padding-bottom: 10px;
}

.el-input__inner {
  border-top-right-radius: 0;
  border-bottom-right-radius: 0;
}

.el-button--primary {
  border-top-left-radius: 0;
  border-bottom-left-radius: 0;
}

.list-title {
  padding-bottom: 10px;
  font-size: 14px;
  color: #b5b1b2;
}

.item {
  font-size: 16px;
  font-weight: 600;
  padding-bottom: 5px;
}

.item:last-of-type {
  padding-bottom: 30px;
}

.el-button--success {
  width: 100%;
}

._more {
  max-height: 300px;
  overflow-y: scroll;
}

.footer {
  width: 100%;
  position: absolute;
  bottom: 0;
}

.el-button--danger {
  width: 100%;
  border-top-left-radius: 0 !important;
  border-top-right-radius: 0 !important;
}

.draw-title {
  margin: 0;
  padding-bottom: 10px;
}

.bold {
  font-weight: 700;
}

.lucky-button {
  width: 100%;
  margin-top: 10px;
}

/* ===== Scrollbar CSS ===== */
/* Firefox */
* {
  scrollbar-width: auto;
  scrollbar-color: #e3e3e3 #ffffff;
}

/* Chrome, Edge, and Safari */
*::-webkit-scrollbar {
  width: 11px;
}

*::-webkit-scrollbar-track {
  background: #ffffff;
}

*::-webkit-scrollbar-thumb {
  background-color: #e3e3e3;
  border-radius: 10px;
  border: 1px solid #ffffff;
}
</style>
