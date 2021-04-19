<template>
  <div class="loading-spinner">
    <font-awesome-icon :icon="['far', 'life-ring']" spin class="icon" />
    Loading
  </div>
</template>

<script>
export default {
  name: 'LoadingSpinner',
  props: {
    text: {
      type: String,
      default: 'Loading'
    }
  }
}
</script>

<style lang="less" scoped>
.loading-spinner {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  font-size: 32px;
  width: 100%;
  height: 100%;
  padding: 100px;

  .icon {
    color: #FDC521;
  }
}
</style>
