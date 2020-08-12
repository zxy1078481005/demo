<template>
  <div class="pagination">
    <button @click="changeBtn">首页</button>
    <button @click="changeBtn">上一页</button>
    <button v-if="jduge">......</button>
    <button v-for="btn in pagebtns"
    :class="[{currentPage:btn == currentPage},'pagebtn']"
    @click="changeBtn(btn)">
      {{btn}}
    </button>
    <button>......</button>
    <button @click="changeBtn">下一页</button>
  </div>
</template>

<script>
import $ from 'jquery'
export default {
  name:"Pagination",
  data(){
    return {
      pagebtns:[1,2,3,4,5],
      currentPage:1,
      jduge: false
    }
  },
  methods:{
    changeBtn(page){
      if(typeof page != 'number'){
        switch(page.target.innerText){
          case '上一页':
            $('button.currentPage').prev().click();
            break;
          case '下一页':
            $('button.currentPage').next().click();
            break;
          case '首页':
            this.pagebtns = [1,2,3,4,5];
            this.changeBtn(1);
            break;
          default:
            break;
        }
        return;
      }
      this.currentPage = page;
      if(page>4){
        this.jduge = true;
      }else{
        this.jduge = false;
      }
      if(page == this.pagebtns[4]){
        this.pagebtns.shift(); //移除第一个元素
        this.pagebtns.splice(4,0,this.pagebtns[3]+1); //添加最后一个
      }else if(page == this.pagebtns[0] && page != 1){
        this.pagebtns.unshift(this.pagebtns[0]-1);
        this.pagebtns.splice(5,1);
      }
      this.$emit('handleList',this.currentPage);
    }
  }
}
</script>

<style scoped>
.pagination {
  padding: 3rem;
  width: 100%;
  text-align: center;
}
button {
  border: 1px solid #122625;
  background: transparent;
  padding: 0.8rem;
  border-radius: 6px;
  margin-right: 0.8rem;
  font-size: 0.8rem;
  cursor: pointer;
  color: #122625;
}
button:hover {
  background: #445b55;
  color: #fff;
  border: 1px solid #445b55;
}
.pagination button:focus {
  outline: none;
}
button:last-of-type {
  margin-right: 0;
}
button.currentPage {
  border: 1px solid #122625;
  background: #122625;
  color: #fff;
}
</style>