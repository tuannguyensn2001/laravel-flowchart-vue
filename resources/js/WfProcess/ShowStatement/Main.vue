<template>
  <div id="root">
    <button type="button"
            @click="$refs.chart.add({id: +new Date(), x: 10, y: 10, name: 'New', type: 'operation', approvers: []})">
      Add
    </button>
    <button type="button" @click="$refs.chart.remove()">
      Del
    </button>
    <button type="button" @click="$refs.chart.editCurrent()">
      Edit
    </button>
    <button type="button" @click="$refs.chart.save()">
      Save
    </button>
    <flowchart :nodes="nodes" :connections="connections" @editnode="handleEditNode"
               @dblclick="handleDblClick" @editconnection="handleEditConnection"
               @connect="connect" @disconnect="disconnect"
               @save="handleChartSave" ref="chart">
    </flowchart>

    <Dialog :node="currentNode.approvers[0]"  @handleOk="handleOkModal"/>

  </div>
</template>
<script>
import Vue from 'vue';
import FlowChart from 'flowchart-vue';
import Dialog from './components/Dialog';
import eventBus from "./plugins/eventBus";

Vue.use(FlowChart);

export default {
  components: {
    Dialog
  },
  name: 'App',
  data: function () {
    return {
      nodes: [
        // Basic fields
        {id: 1, x: 140, y: 270, name: 'Start', type: 'start'},

        {
          id: 4,
          x: 240,
          y: 220,
          name: "Tên quy trình",
          type: "operation",
          width: 200,
          height: 50,
          approvers: [{id: 5, name: 'Allens\nTuấn bla bla bla', age: 18},{id: 6, name: 'Allens\nTuấn bla bla bla', age: 18}],
        },
        {id: 2, x: 540, y: 270, name: 'End', type: 'end', description: 'Im here'},
      ],
      connections: [
        {
          source: {id: 1, position: 'right'},
          destination: {id: 2, position: 'left'},
          id: 1,
          type: 'pass',
        },
      ],
      currentNode: {
        id: null,
        x: null,
        y: null,
        name: '',
        type: '',
        approvers: [],
      },
    };
  },
  methods: {
    disconnect() {
      console.log('disconnect');
    },
    connect(conn) {
      this.$refs.chart.removeConnection(conn);
      alert('Không chỉnh sửa được');
    },
    // eslint-disable-next-line no-unused-vars
    handleChartSave(nodes, connections) {
      // axios.post(url, {nodes, connections}).then(resp => {
      //   this.nodes = resp.data.nodes;
      //   this.connections = resp.data.connections;
      //   // Flowchart will refresh after this.nodes and this.connections changed
      // });
    },
    handleEditNode(node) {
      if (node.id === 1 || node.id === 2) return;
      this.currentNode = node;
      eventBus.$emit('editNode');
    },
    // eslint-disable-next-line no-unused-vars
    handleEditConnection(connection) {
      console.log('edit connection', connection);
    },
    handleDblClick(position) {
      this.$refs.chart.add({
        id: +new Date(),
        x: position.x,
        y: position.y,
        name: 'New',
        type: 'operation',
        approvers: [],
      });
    },
    handleOkModal() {
      const index = this.nodes.findIndex(node => node.id === this.currentNode.id);
      this.nodes[index].name = 'change';

      console.log(this.nodes[index]);

    }
  }
};
</script>
