<template lang='pug'>
LoadIng(:active="isLoading")

.button_top_box
  button(type='button' @click="DiscountModalShow(true)") new discount
.container-fluid
  table
    thead
      tr
        th 名稱
        th 折扣百分比
        th 到期日
        th 是否啟用
        th 編輯
    tbody
      tr(v-for="item in discount_count" :key="item.id")
        td {{item.title}}
        td {{item.percent}}
        td {{$filters.date(item.due_date)}}
        td
          span(v-if="item.is_enabled") 啟用
          span(v-else) 不啟用
        td
          button.button_gr(@click="DiscountModalShow(false, item)") 更新
          button.button_red(@click="openDelProductModal(item)") 刪除
Pagination(:pages='pagination' @emit-pages="getDiscountData")
DiscountMidal(ref='DiscountMidal' :count_data="tempcount" @updata-count="updateData")
DelModal(ref="delModal" :item="tempcount"  @del-item="delProduct")
</template>
<script>
import DiscountMidal from '@/components/DiscountModal.vue';
import Pagination from '@/components/PaginatIon.vue';
import DelModal from '@/components/DelModal.vue';

export default {
  data() {
    return {
      discount_count: [],
      pagination: {},
      tempcount: {
        title: '',
        is_enabled: 0,
        percent: 100,
        code: '',
      },
      isNew: false,
      isLoading: false,
    };
  },
  components: {
    DiscountMidal,
    Pagination,
    DelModal,
  },
  methods: {
    getDiscountData(page = 1) {
      const api = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/admin/coupons/?page=${page}`;
      this.isLoading = true;
      this.$http.get(api)
        .then((res) => {
          this.isLoading = false;
          if (res.data.success) {
            console.log(res.data, 'count_yes');
            this.discount_count = res.data.coupons;
            this.pagination = res.data.pagination;
          }
        });
    },
    updateData(item) {
      const productComponent = this.$refs.DiscountMidal;
      if (this.isNew) {
        const httpMethod = 'post';
        const api = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/admin/coupon`;
        this.$http[httpMethod](api, { data: item })
          .then((response) => {
            console.log(response, item);
            this.getDiscountData();
            productComponent.hideModal();
            this.$httpMessageState(response, '更新狀態');
          });
      } else {
        const api = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/admin/coupon/${item.id}`;
        const httpMethod = 'put';
        this.$http[httpMethod](api, { data: item })
          .then((response) => {
            console.log(response, item);
            this.getDiscountData();
            productComponent.hideModal();
            this.$httpMessageState(response, '更新狀態');
          });
      }
    },
    DiscountModalShow(isNew, item) {
      this.isNew = isNew;
      console.log(item, isNew);
      if (this.isNew) {
        this.tempcount = { due_date: new Date().getTime() / 1000 };
      } else {
        this.tempcount = { ...item };
      }
      const productComponent = this.$refs.DiscountMidal;
      productComponent.showModal();
    },
    openDelProductModal(item) {
      this.tempcount = { ...item };
      const delComponent = this.$refs.delModal;
      delComponent.showModal();
    },
    delProduct() {
      const url = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/admin/coupon/${this.tempcount.id}`;
      this.$http.delete(url).then((response) => {
        console.log(response.data, 6666);
        const delComponent = this.$refs.delModal;
        delComponent.hideModal();
        this.$httpMessageState(response, '更新狀態');
        this.getDiscountData();
      });
    },
  },
  created() {
    this.getDiscountData();
  },
};
</script>
<style lang="sass">
*
  // border: 1px solid #000
.button_top_box
  float: right
  button
    padding: 10px 15px
    border-radius: 5px
    transition: 0.5s
    border: none
    font-size: 16px
    color: #333
    &:hover
      background-color: #333
      color: #eee
table
  width: 100%
  thead
    border-bottom: 1px solid rgba(black,0.2)
  tbody
    td
      width: 200px
      button
        padding: 10px 15px
        margin: 0px 15px
        border-radius: 5px
        transition: 0.5s
        border: none
        &.button_red
          background-color: #CC543A
          color: #333
        &.button_gr
          background-color: #B1B479
          color: #333
        &:hover
          background-color: #333
          color: #eee
</style>
