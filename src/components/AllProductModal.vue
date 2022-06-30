<template lang='pug'>
#delModal.modal.fade.modal-xl(tabindex='-1' role='dialog'
aria-labelledby='exampleModalLabel' aria-hidden='true' ref='modal')
  .modal-dialog(role='document')
    .modal-content.border-0
      .modal-header.bg-black.text-white
        h5.modal-title
          span 訂單資訊
        button.btn-close(type='button' data-bs-dismiss='modal' aria-label='Close')
      .modal-body
        .row
          .col-4
            h5 用戶資訊
            .order_box
              span.tital 名稱
              span 詳細名稱
            .order_box
              span.tital 名稱
              span 詳細名稱
            .order_box
              span.tital 名稱
              span 詳細名稱
            .order_box
              span.tital 名稱
              span 詳細名稱
          .col-8
            .order_detall_box
              h5 訂單細節
              .order_box
                span 訂單編號
                span 詳細名稱
              .order_box
                span 下單時間
                span 詳細名稱
              .order_box
                span 付款時間
                span 詳細名稱
              .order_box
                span 付款狀態
                span 詳細名稱
              .order_box
                span 總金額
                span 詳細名稱
            .order_product_box
              h5 訂單詳細
              .shop_order_box
                h6 好看的衣服
                p 1/建
                p price
      .modal-footer
        button.btn.btn-outline-secondary(type='button' data-bs-dismiss='modal') 取消
        button.btn.btn-danger(type='button') 刪除
</template>
<script>
import modalMixin from '@/mixin/modalMixins';

export default {
  props: {
    order: {},
  },
  data() {
    return {
      modal: {},
      tempProduct: {},
    };
  },
  mixins: [modalMixin],
};
</script>
<style lang="sass">
*
  box-sizing: border-box
  // border: solid 1px
  .modal-body
    h5
      font-weight: bold
      font-size: 24px
    .order_box
      margin-bottom: 10px
      border-bottom: 1px solid rgba(black,0.2)
      padding: 5px
      span
        margin-right: 20px
      .tital
        font-weight: bold
    .shop_order_box
      display: flex
      justify-content: space-between
</style>
