<template>
  <section class="container">
    <div class="content2">
      <header class="header">
        <h2 class="header-title2">랜덤뽑기의 제목을<br />입력해주세요 !</h2>
      </header>
      <div class="body">
        <div class="search">
          <el-input placeholder="제목 입력" v-model="input1" />
          <el-button type="primary" @click="invite()" @keydown.enter="invite()"
            >입력!</el-button
          >
        </div>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: "IndexPage",
  data() {
    return {
      input1: "",
      result: "",
      fullscreenLoading: false,
    };
  },
  methods: {
    async invite() {
      if (this.input1 === "") {
        this.$alert("이름을 입력해주세요", "알림", {
          confirmButtonText: "OK",
        });
        return false;
      } else {
        await this.$router.push(`/play?title=${this.input1}`);
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

.container {
  position: relative;
  width: 100%;
  height: 100vh;
}

.content2 {
  position: absolute;
  width: 350px;
  height: 200px;
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

.header-title2 {
  padding: 30px 0;
}

.body {
  padding: 0 30px;
}

.search {
  display: flex;
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
</style>
