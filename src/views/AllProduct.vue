<template lang='pug'>
LoadIng(:active="isLoading")
table
  thead
    tr
      th 購買時間
      th Email
      th 購買款項
      th 應付金額
      th 是否付款
      th 編輯
  tbody
    tr
      td 200/1022/1
      td 616189@gmail.com
      td 大衣1111111
      td 9000元
      td 是
      td
        button(@click="AllModlashow()") 編輯
        button(@click="All_DelModla_show()") 刪除
AllProductModal(ref="allproductModal" :order="tempProduct")
Pagination(:pages='All_pagination')
</template>
<script>
import AllProductModal from '@/components/AllProductModal.vue';
import Pagination from '@/components/PaginatIon.vue';

export default {
  data() {
    return {
      All_products: [],
      All_pagination: {},
      tempProduct: {},
      isNew: false,
      isLoading: false,
    };
  },
  components: {
    AllProductModal,
    Pagination,
  },
  methods: {
    getAllProducts(page = 1) {
      const api = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/admin/orders/?page=${page}`;
      this.isLoading = true;
      this.$http.get(api)
        .then((res) => {
          this.isLoading = false;
          if (res.data.success) {
            console.log(res.data, 'order_yes');
            this.All_priducts = res.data.orders;
            this.All_pagination = res.data.pagination;
          }
        });
    },
    AllModlashow() {
      const productComponent = this.$refs.allproductModal;
      productComponent.showModal();
    },
    All_DelModla_show() { // not yet
      const productComponent = this.$refs.delproductModal;
      productComponent.showModal();
    },
    Del_product() { // not yet
      const api = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/admin/order/${this.tempProduct.id}`;
      this.$http.delete(api).then((response) => {
        console.log(response.data, 'pr');
        const delComponent = this.$refs.delModal;
        delComponent.hideModal();
        this.$httpMessageState(response, '更新狀態');
      });
    },
    DelAll_product() { // not yet
      const api = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/admin/orders/all}`;
      this.$http.delete(api).then((response) => {
        console.log(response.data, 'all');
        const delComponent = this.$refs.delModal;
        delComponent.hideModal();
        this.$httpMessageState(response, '更新狀態');
      });
    },
  },
  created() {
    this.getAllProducts();
  },
};
</script>
<style lang="sass">
table
  width: 100%
</style>
