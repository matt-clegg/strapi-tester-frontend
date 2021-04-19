<template>
  <div class="modal-backdrop">
    <div class="modal">
      <slot name="header" />
      <div class="modal__body">
        <slot name="body" />
      </div>
      <slot name="footer" />
    </div>
  </div>
</template>

<script>
export default {
  name: 'Modal',
  methods: {
    close () {
      this.$emit('close')
    }
  }
}
</script>

<style lang="less" scoped>
.modal {
  background-color: #fff;
  display: flex;
  flex-direction: column;
  border-radius: 10px;
  box-shadow:
    0 0.4px 0.8px rgba(0, 0, 0, 0.02),
    0 0.9px 1.9px rgba(0, 0, 0, 0.028),
    0 1.8px 3.6px rgba(0, 0, 0, 0.035),
    0 3.1px 6.5px rgba(0, 0, 0, 0.042),
    0 5.8px 12.1px rgba(0, 0, 0, 0.05),
    0 14px 29px rgba(0, 0, 0, 0.07)
  ;

  &-backdrop {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: rgba(0, 0, 0, .3);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1000;
  }

  &__body {
    padding: 30px;
  }
}
</style>
