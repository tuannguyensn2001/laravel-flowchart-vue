<template>
  <div>
    <a-modal
        title="Title"
        :visible="isVisible"
        @ok="handleOk"
        @cancel="handleCancel"
    >
      <div>
        <label for="time-process">Thời gian</label>
        <a-input id="time-process" placeholder="Basic usage" v-model="content" />
      </div>
    </a-modal>
  </div>
</template>

<script>
  import {Modal} from 'ant-design-vue';
  import {Input} from 'ant-design-vue';
  import Vue from 'vue';
  import eventBus from "../plugins/eventBus";

  Vue.use(Modal);
  Vue.use(Input);

  export default {
    props: {
      node: {
        type: Object,
        // eslint-disable-next-line vue/require-valid-default-prop
      }
    },
    data(){
      return {
        isVisible: false,
      }
    },
    mounted() {
      eventBus.$on('editNode',() => {
        this.isVisible = true;
      })
    },
    methods: {
      handleOk(){
        this.$emit('handleOk');
      },
      handleCancel(){
        this.isVisible = false;
      }
    },
    computed:{
      content:{
        get(){
          return this.node?.name;
        },
        set(val){
          this.node.name = val;
        }
      }
    }

  }



</script>
