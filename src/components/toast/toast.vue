<template>
<div :class="{'weui_loading_toast': type === 'loading'}">
  <div class="weui_mask_transparent"></div>
  <div class="weui_toast">
    <div class="weui_loading" v-if="type === 'loading'">
      <div v-for="(n, index) in 12" :key="index" class="weui_loading_leaf" :class="'weui_loading_leaf_' + index"></div>
    </div>
    <i class="weui_icon_toast" v-else></i>
    <div class="weui_toast_content">
      <slot></slot>
    </div>
  </div>
</div>
</template>

<script>
export default {
  props: {
    type: {
      type: String,
      required: false,
      default: 'icon'
    }
  }
}
</script>
