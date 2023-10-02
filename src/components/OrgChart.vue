<template>
  <div>
   <div v-if="!showChild">
      <h1>Click On Client To See Provider</h1>
      <organization-chart :datasource="modifiedRecords" id="chart-container" @node-click="handleNodeClick"></organization-chart>
    </div>
    <ChildrenView v-if="showChild" :children-data="childrenData" @back="closeFunc"></ChildrenView >
  </div>
  </template>

  <script>
  import OrganizationChart from 'vue-organization-chart';
  import 'vue-organization-chart/dist/orgchart.css';
  import ChildrenView from './ChildChart.vue';

  export default {
    components: {
      OrganizationChart,
      ChildrenView,
    },
    props: {
    records: {
      type: Object,
      required: true,
      },
    },
    data() {
    return {
      showChild: false,
      childrenData: null,
      clientData: null,
    };
  },
    methods: {
      handleNodeClick(nodeData) {
        if (nodeData) {
        this.showChild = true;
        const record = this.records.find(record => record.name === nodeData.name);
          if (record) {
            this.childrenData = record.children[0];
          }
         } // else {
        //   console.log('Node clicked:', nodeData)
        // }
      },
      closeFunc() {
      this.showChild = false;
      this.childrenData = null;
    }
    },
    mounted() {
     this.$nextTick(() => {
       const divElements = document.querySelectorAll('.node');

        divElements.forEach(divElement => {
          // const titleDiv = divElement.querySelector('.title');
          // const imgElement = document.createElement('img');
          // imgElement.src = this.records.avatar;
          // imgElement.style.width = '13px';
          // titleDiv.appendChild(imgElement);

          const contentElement = divElement.querySelector('.content');
            if (contentElement && contentElement.textContent.trim() === 'PROVIDER_LEVEL_2') {
            const titleElement = divElement.querySelector('.title');
            if (titleElement) {
                titleElement.style.backgroundColor = '#983365';
            }
          } else if (contentElement && contentElement.textContent.trim() === 'PROVIDER_LEVEL_1') {
            const titleElement = divElement.querySelector('.title');
            if (titleElement) {
                titleElement.style.backgroundColor = '#006699';
            }
          } else if (contentElement && contentElement.textContent.trim() === 'PROVIDER_LEVEL_ADMIN') {
            const titleElement = divElement.querySelector('.title');
            if (titleElement) {
                titleElement.style.backgroundColor = 'green';
            }
        }
        });
     });
     },
  computed: {
    //display only client level
    modifiedRecords() {
      const newObjects = this.records.map(obj => {
      const { id, name, title } = obj;
      return { id, name, title };
    });
    return { children: newObjects};
    },
  },
  //     created() {
  //   console.log('This is a log message', this.records.avatar);
  // }
        };
  </script>

  <style scoped>
  /* Add CSS styles for node content and conditional styling here */
  </style>
