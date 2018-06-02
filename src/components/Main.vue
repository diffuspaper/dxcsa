<template>
  <div class="content">
    <div class="header">Daxcsa</div>
    <div class="alert alert-info" role="alert" style="text-align:center">
      Welcome to the Referrer Program Tree!
    </div>
    <div class="alert alert-warning" role="alert">
      You can navigate trought all the Referrer tree by clicking on <b>"View Referrals"</b> and <b>"View Parent Referrals"</b>
    </div>
    <div class="alert alert-danger" role="alert" v-if="error">
      <b>Hey!</b> Something happened, we can't provide you such information at this time.
    </div>
    <Tree :items="treeDataRender" @changeTree="changeNodes"></Tree>
  </div>
</template>
<script>
import Tree from '@/components/Tree'
import axios from 'axios'

export default {
  name: 'Main',
  components: {
    Tree
  },
  mounted () {
    axios.get('/static/Daxcsa.json').then(function (response) {
      this.treeData = response.data.data.attributes
      this.treeDataRender = response.data.data.attributes
    }.bind(this))
    .catch(function(error){
      console.log(error)
    })
  },
  methods: {
    changeNodes: function (username, option) {
      this.treeDataRender = []
      if(option == 'parent'){
        let result = this.searchParentParent(this.treeData[0], username)
        if(result != null)
          username = result.parent_username
        else{
          this.error = true
          setTimeout(function(){
            this.error = false
          }.bind(this), 3000)
        }
      }
      this.searchTree(this.treeData[0], username)
    },
    searchTree: function(element, parentUsername){
      if(element.parent_username == parentUsername){
            this.treeDataRender.push(element)
      }else if (element.children != null){
            let i;
            let result = null;
            for(i=0; i < element.children.length; i++){
              this.searchTree(element.children[i], parentUsername);
            }
            return true;
      }
      return true;
    },
    searchParentParent: function (element, username) {
      if(element.username == username){
            return element;
      }else if (element.children != null){
            let i;
            let result = null;
            for(i=0; result == null && i < element.children.length; i++){
              result = this.searchParentParent(element.children[i], username);
            }
            return result;
      }
      return null
    }
  },
  data () {
    return {
      treeData: {},
      treeDataRender: [],
      error: false
    }
  }
}
</script>
<style>
.header{
  width: 100%;
  font-size: 2em;
  color: white;
  background-color: #6c757d;
  text-align: center;
}
.legend{
  font-size: 0.5em
}
</style>
