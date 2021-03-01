<template>
  <div class="component">
    <h1 class="title">メモアプリ</h1>
    <div class=memo-component>
      <div class=memo-index>
        <li v-for="(memo) in memos" :key="memo.key">
          <a href="#" @click="editMemo(memo)">{{memo.oneLine}}</a>
        </li>

        <button @click="addNewMemo" class="new-memo-button btn btn-primary">+</button>
      </div>
      <div class="text-box">
        <textarea class="textarea" placeholder="新規メモ" v-model="memoValue" rows="10"></textarea>
        <button @click="updateMemos" class="done btn btn-success">完了</button>
        <button @click="deleteMemo" class="delete btn btn-danger">削除</button>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'App',
    data() {
      return {
        memos: [],
        oneLineMemos: [],
        edit: [],
        memoValue: "",
        memoState: "new",
      }
    },
    methods: {
      addNewMemo() {
        this.memoState = "new"
        this.newMemo = ""
      },
      editMemo(memo) {
        this.memoValue = memo.content
        this.memoState = memo.id
      },
      updateMemos() {
        if (this.memoValue === "") {
          return;
        }
        if (this.memoState === "new") {
          let uuid = new Date().getTime().toString(16) + Math.floor(1000 * Math.random()).toString(16)
          this.memos.push({
            id: uuid,
            content: this.memoValue,
            oneLine: this.memoValue.split(/\r\n|\r|\n/)[0]
          })
        } else {
          const num = this.memos.findIndex(item => item.id === this.memoState)
          this.memos[num].content = this.memoValue
          this.memos[num].oneLine = this.memoValue.split(/\r\n|\r|\n/)[0]
        }
        this.memoValue = ""
        this.memoState = "new"
        this.saveMemo(); 
      },
      deleteMemo() {
        const num = this.memos.findIndex(item => item.id === this.memoState)
        this.memos.splice(this.memos[num - 1], 1);
        this.memoValue = ""
        this.memoState = "new"
        this.saveMemo(); 
      },
      saveMemo(){
        localStorage.setItem('memos', JSON.stringify(this.memos));
      },
      loadMemos: function(){
          this.memos = JSON.parse( localStorage.getItem('memos') );
          if( !this.memos ){
            this.memos = [];
          }
        },
    },
    mounted: function(){
      this.loadMemos();
  },
  }
</script>

<style>
  .component {
    margin: 30px auto;
    width: 1000px;
  }

  .memo-component {
    max-width: 1000px;
    margin: 30px auto;
    display: flex;
    position: relative;
  }

  .title {
    display: inline-block;
    width: 100%;
  }

  .memo-index {
    width: 30%;
    background: antiquewhite;
  }

  .new-memo-button {
    margin: 20px;
  }

  .textarea {
    width: 100%;
  }
</style>
