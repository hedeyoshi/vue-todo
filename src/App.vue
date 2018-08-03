<template>
  <!-- templateを指定している -->
  <div id="app">
    <!-- <img src="./assets/check.png"> -->
    <!-- <router-view/> -->
    <div>
      <ul>
        <li v-for="item in items" :key="item.id">
          <label v-bind:class="{ done: item.isChecked }">
            <input type="checkbox"
              v-model="item.isChecked"
              v-bind:class="{ done: item.isChecked }">
            {{ item.title }}
          </label>
        </li>
      </ul>
    </div>
    <div>
      <input type="text"
        placeholder="TODOを入力"
        v-model="newItemTitle"
        v-on:keyup.enter="addTodo(newItemTitle)">
    </div>

    <button v-on:click="deleteTodo()">チェック済みの項目を削除する</button>
  </div>
</template>

<script>
// ここでスクリプトを指定する
export default {
  name: 'App',
  data () {
    return {
      message: 'lorem ipsum',
      isChecked: true,
      items: [
        { title: '領収書を準備する', isChecked: true },
        { title: 'Vue.jsハンズオンの資料を作る', isChecked: true },
        { title: '参加者の人数を確認する', isChecked: false },
        { title: 'ピザを注文する', isChecked: false },
        { title: '参加費のお釣りを準備する', isChecked: false },
        { title: '会場設営をする', isChecked: false }
      ],
      newItemTitle: ''
    }
  },
  methods: {
    addTodo: function (newTitle) {
      this.items.push({
        title: newTitle,
        isChecked: false
      })
      this.newItemTitle = ''
      this.saveTodo()
    },
    deleteTodo: function () {
      this.items = this.items.filter(function (item) {
        return item.isChecked === false
      })
      this.saveTodo()
    },
    saveTodo: function () {
      localStorage.setItem('items', JSON.stringify(this.items))
    },
    loadTodo: function () {
      this.items = JSON.parse(localStorage.getItem('items'))
      if (!this.items) {
        this.items = []
      }
    }

  },
  mounted: function () {
    this.loadTodo()
  }
}
</script>

<style lang="scss" scoped>
/* scssを使うには最初に使うのを宣言にしておいて、
   lang="scss" scopedをstyleに記述する
   とりあえず今は指定していなかったのでやらない方向でいく */
/* cssはここで行う */
@import url(http://yui.yahooapis.com/3.18.1/build/cssreset/cssreset-min.css);
* {
  box-sizing: border-box;
}
#app {
  width: 100vw;
  height: 100vh;
  padding: 0.5rem;
  background-color: #eeeeee;
  color: #1f1f1f;
  font-size: 1rem;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  ul {
    border-top: 1px solid #dddddd;
  }
  li {
    border-bottom: 1px solid #ddd;
  }
  button {
    cursor: pointer;
  }

  label {
    display: block;
    padding: 0.5rem;
    cursor: pointer;
    user-select: none;
    &.done {
      text-decoration: line-through;
    }
  }
  input[type='checkbox'] {
    margin-right: 1rem;
    &:before {
      content: '';
      display: inline-block;
      margin-right: 0.5rem;
      width: 1rem;
      height: 1rem;
      background-color: #995522;
    }
    &.done {
      &:before {
        content: url(./assets/check.png);
        background-color: #009900;
      }
    }
  }
}
</style>
