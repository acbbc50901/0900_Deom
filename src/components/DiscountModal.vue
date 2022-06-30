<template lang='pug'>
#delModal.modal.fade.modal-xl(tabindex='-1' role='dialog'
aria-labelledby='exampleModalLabel' aria-hidden='true' ref='modal')
  .modal-dialog(role='document')
    .modal-content.border-0
      .modal-header.bg-black.text-white
        h5.modal-title
          span 新增優惠卷
        button.btn-close(type='button' data-bs-dismiss='modal' aria-label='Close')
      .modal-body
        .row
          .col-12
            .box_name
              label(for="name") 優惠卷名稱
              input(type="text" id='name' placeholder="請輸入名稱"
              v-model="tempProduct.title")
            .box_name
              label(for="name") 優惠瑪
              input(type="text" id='name' placeholder="請輸入名稱"
              v-model="tempProduct.code")
            .box_name
              label(for="count") 折扣百分比
              input(type="number" id='count' placeholder="請輸入折扣比"
              v-model="tempProduct.percent")
            .box_name
              label(for="data") 到期日期
              input(type="date" id='data' placeholder="選擇到期日期"
              v-model="due_date")
            .box_name
              label(for="yes")  是否啟用
              input(type="checkbox" id='yes' :true-value="1" :false-value="0"
              v-model="tempProduct.is_enabled")
      .modal-footer
        button.btn.btn-outline-secondary(type='button'
        @click="$emit('updata-count', tempProduct)") 新增
        button.btn.btn-danger(type='button' data-bs-dismiss='modal' ) 取消
</template>
<script>
import modalMixin from '@/mixin/modalMixins';

export default {
  props: {
    count_data: { },
  },
  emits: [
    'updata-count',
  ],
  watch: {
    count_data() {
      this.tempProduct = this.count_data;
      const dateAndTime = new Date(this.tempProduct.due_date * 1000).toISOString().split('T');
      [this.due_date] = dateAndTime;
    },
    due_date() {
      this.tempProduct.due_date = Math.floor(new Date(this.due_date) / 1000);
    },
  },
  data() {
    return {
      modal: {},
      tempProduct: {
        title: '',
        is_enabled: 0,
        percent: 100,
        code: '',
      },
      due_date: '',
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
    .col-12
      display: flex
      flex-direction: row
      justify-content: space-between
      align-items: center
      .box_name
        display: flex
        flex-direction: column
        label
          margin-bottom: 20px
</style>
