<template>
  <div id="app" class="border-1px">
    <v-header :seller="seller"></v-header>
    <div class="tab">
      <div class="tab-item"><router-link to="/goods">商品</router-link></div>
      <div class="tab-item"><router-link to="/ratings">评论</router-link></div>
      <div class="tab-item"><router-link to="/seller">商家</router-link></div>
    </div>
    <div class="content">
      <router-view :seller="seller"></router-view>
    </div>
  </div>
</template>

<script>
import header from "components/header/header.vue";
import {urlParse} from 'common/js/until';

const ERR_OK = 0;
export default {
  components: {
    "v-header": header
  },
  data() {
    return {
      seller: {
        id: (() => {
          let queryParam = urlParse()
          return queryParam.id
        })()
      }
    };
  },
  created() {
    this.$http.get("/api/seller? id=" + this.seller.id).then(response => {
      response = response.body;
      if (response.errno === ERR_OK) {
        this.seller = Object.assign({}, this.seller, response.data);
      }
    });
  }
};
</script>

<style lang="stylus" scoped>
@import 'common/stylus/mixin'

#app
  .tab
    display flex
    width 100%
    height 40px
    line-height 40px
    border-1px(rgba(7, 17, 21, 0.1))
    .tab-item
      flex 1
      text-align center
      &>a
        display block
        width 100%
        font-size 14px
        color rgb(77, 85, 93)
        &.active
          color rgb(240, 20, 20)
</style>




