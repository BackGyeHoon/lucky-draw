<template>
  <section class="container">
    <div class="content">
      <header class="header">
        <h2 class="header-title">양말을 가져가보자</h2>
      </header>
      <div class="body">
        <div class="search">
          <el-input
            placeholder="이름 입력"
            v-model="input1" />
          <el-button type="primary" @click="invite()" @keydown.enter="invite()">등록</el-button>
        </div>
        <div class="list" v-if="datas.length !== 0">
          <el-card class="box-card" :class="{'_more': datas.length > 9}">
            <h2 class="list-title">이름 (총 인원: {{ datas.length }} 명 )</h2>
            <div v-for="(item, index) in datas" :key="item.index" class="text item">
              {{ index + 1 }}. {{ item }}
            </div>
            <div style="display: flex; padding-top: 30px;">
              <el-button type="success" icon="el-icon-search" @click="start()" v-loading.fullscreen.lock="fullscreenLoading">추첨하기</el-button>
              <el-button type="danger" icon="el-icon-delete" @click="reset()"></el-button>
            </div>
          </el-card>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      input1: '',
      datas: [],
      result: '',
      fullscreenLoading: false,
    }
  },
  methods: {
    async invite() {
      await this.datas.push(this.input1)
      this.input1 = ''
    },
    reset() {
      this.$confirm('정말 삭제하시겠습니까?', '알림', {
          confirmButtonText: '네',
          cancelButtonText: '아니오',
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            message: '삭제 되었습니다.'
          });
          this.datas = []
        }).catch(() => {
        });
    },
    start() {
      if (this.datas.length === 1) {
        alert('2명이상 입력해주세요 !')
        return false
      } else {
        this.fullscreenLoading = true;
        setTimeout(() => {
          this.result = this.datas[Math.floor(Math.random() * this.datas.length)]
          this.fullscreenLoading = false;
          this.$alert(this.result, '당첨자!', {
            confirmButtonText: 'OK',
            callback: action => {
              this.datas = []
            }
          });
        }, 5000);
      }
    }
  }
}
</script>
<style>
body,
h1,
h2 {
  margin: 0;
  padding: 0;
}

body {
  background-color: #e6e6e6;
}

.container {
  position: relative;
  width: 100%;
  height: 100vh;
}

.content {
  position: absolute;
  width: 400px;
  height: 500px;
  background-color: #fff;
  border-radius: 16px;
  margin: 0 auto;
  left: 50%;
  top: 50%;
  transform: translateX(-50%) translateY(-50%);
}

.header {
  width: 100%;
  height: auto;
  text-align: center;
}

.header-title {
  padding: 15px 0;
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