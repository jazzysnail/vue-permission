<template>
  <div v-if="show">
    <slot></slot>
  </div>
</template>

<script>
  export default {
    name: 'permissionScope',
    props: {
      pass: {
        type: String,
        default: ''
      },
      nonPass: {
        type: String,
        default: ''
      },
      scope: [String, Array, Object, Number]
    },
    data() {
      return {
        show: false
      }
    },
    methods: {
      getPermission(name) {
        return this.$root.$options.permissions.getPermission(name);
      },
      computeShow() {
        if (this.nonPass) {
          return !this.getPermission(this.nonPass);
        } else {
          return this.getPermission(this.pass);
        }
      },
      update() {
        this.show = this.computeShow();
        this.$emit('changed', this.show, this.scope);
      }
    },
    created() {
      const vm =this;
      this.update();
      // 注册码表更新事件，更新当前权限模块的显示
      this.$root.$options.permissions.$on('tokenSetUpdated', function() {
        vm.update();
      });
    },
  }
</script>