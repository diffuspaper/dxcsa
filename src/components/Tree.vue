<template>
  <div class="tree">
    <Node v-for="item in items" :key="item.distributor_id" :info="item" @renderParent="renderTree"></Node>
  </div>
</template>
<script>
import Node from '@/components/Node'

export default {
  name: 'Tree',
  props: ['items'],
  components:{
    Node
  },
  methods: {
    renderTree (username, option) {
      this.$emit('changeTree', username, option);
    }
  }
}
</script>
