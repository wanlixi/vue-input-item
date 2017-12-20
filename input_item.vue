<template>
  <p class="cont_form_input">
    <small class="label">{{title}}</small>
    <input 
      v-model="value" 
      :class="[(tip && !value_is_right) ? 'error' : '']"
      @blur="blur"
      :placeholder="placeholder" 
      :maxlength="max_length" />
    <small :class="['mark', value_is_right ? 'mark_right' : '']" v-show="!!tip">
      <span>{{tip}}</span>
      <Icon :type="value_state"></Icon>
    </small>
  </p>
</template>
<script>
import { reg } from '@/assets/js/utils'
export default {
  props: ['title', 'reg', 'id_card_type', 'max_length'],
  data () {
    return {
	value: '',
	value_is_right: false,
	value_state: '',
	tip: '',
	placeholder: `请输入${this.title}`
    }
  },
  methods: {
    blur () {
      this.value_is_right = this.reg !== 'CERTIFICATION' ? reg[this.reg].test(this.value) : this.id_card_type === 6 ? this.value_is_right = reg.CERTIFICATION.every(item => this.id_card_type == item.ID && item.REG.test(this.value)) : reg.CERTIFICATION.some(item => this.id_card_type == item.ID && item.REG.test(this.value));
      this.value_state = this.value_is_right ? 'checkmark-circled' : 'alert-circled';
      this.tip = this.value_is_right ? `${this.title}输入正确！` : `${this.title}输入错误！`;
    }
  }
}
</script>
<style lang="stylus" scoped>
@import '../assets/css/common/var'
.label
  display block
  color $commonYellow
.cont_form_input
  margin-bottom .26rem; // 13px
.cont_form_input input
  width 100%
  height .6rem; // 30px
  border 0
  border-bottom 2px solid #b3b3b3
  outline none
  &.error
    border-color #D50000
	
// 正则提示
.mark
  display flex
  justify-content space-between
  align-items center
  padding .2rem/* 10px */ 0
  font-size .24rem/* 12px */
  color #f00
.mark_right
  color $commonTipRight !important
</style>
