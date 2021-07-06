<template>
  <div>
     <div>单号1234</div>  
     <button style="margin:20px">显示下拉</button>
      <button style="margin:20px">添加下拉父节点</button>
         <button style="margin:20px">添加下拉子节点</button>
            <button style="margin:20px">删除下拉父节点</button>
               <button style="margin:20px">删除下拉子节点</button>
     <treeselect v-model="value" :multiple="true" :options="options" :defaultExpandLevel="10"/></div>
 </div>
</template>

<script>

import Treeselect from "@riophae/vue-treeselect";
// import the styles
import "@riophae/vue-treeselect/dist/vue-treeselect.css";
export default {
  components: { Treeselect },
  data() {
    return {
      value: null,
      // define options
      options: [
        {
          id: "a",
          label: "a",
          children: [
            {
              id: "aa",
              label: "aa",
            },
            {
              id: "ab",
              label: "ab",
            },
          ],
        },
        {
          id: "b",
          label: "b",
        },
        {
          id: "c",
          label: "c",
        },
      ],
    }
  },
  methods: {
    jumpTo() {
      this.$router.push(`/test-link/index/${Date.now()}`);
    },
  },
};
</script>

<style>
</style>
