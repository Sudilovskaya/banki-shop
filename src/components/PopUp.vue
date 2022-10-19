<template>
  <div class="dialog" @click="hideDialog">
    <div @click.stop class="popup">
      <button class="popup__close" @click="hideDialog">
        <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
          <g opacity="0.3">
            <path d="M18 6L6 18" stroke="#433D39" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
            <path d="M6 6L18 18" stroke="#433D39" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
          </g>
        </svg>
      </button>
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    hideDialog() {
      this.$emit('hideDialog');
    }
  }
};
</script>