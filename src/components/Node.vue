<template>
  <div class="row node">
    <div class="col-md-12">
      <div class="row">
        <div class="col-md-12">
          <button v-on:click="renderChildren(info.parent_username, 'parent')" v-if="info.parent_username != null" class="btn btn-danger float-left">View Parent Referrals</button>
          <b-btn v-b-modal="'modal' + info.distributor_id" class="float-right">View Info</b-btn>
           <b-modal v-bind:id="'modal' + info.distributor_id" title="Information" ok-only>
              Name:<br/>
              <label>{{ info.full_name }}</label>
              Category:<br/>
              <label>{{ info.category_name || 'Not found' }}</label>
              Product:<br/>
              <label>{{ info.product_name || 'Not found' }}</label>
              Status:<br/>
             <label>{{ info.status|| 'Not found' }}</label>
          </b-modal>
        </div>
      </div>
      <div class="row">
        <div class="col-md-12">
          <h2 class="name">{{ info.full_name }}</h2>
        </div>
      </div>
      <div class="row">
        <div class="col-md-12">
          <button class="btn btn-primary float-right" v-on:click="renderChildren(info.username, 'child')" v-if="info.children != undefined">View Referrals</button>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'Node',
  props: [
    'info'
  ],
  methods: {
    renderChildren: function (username, option) {
      this.$emit('renderParent', username, option)
    },
  }
}
</script>
<style>
body{
  background-color: lightgray;
}
.row{
  margin-left: 0px !important;
  margin-right: 0px !important;
}
h1{
  display: block;
  width: 100%
}
.node {
  margin-top: 3em;
  background-color: lightslategrey;
}
.name{
  padding: 0.25em 0;
  color: white;
}
.btn {
  border-radius: 0 !important;
}
label{
  width: 100%;
  font-weight: bold;
}
</style>

