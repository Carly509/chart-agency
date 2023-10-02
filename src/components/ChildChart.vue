<template>
    <div>
      <organization-chart :datasource="parent" id="chart-container"></organization-chart>
    </div>
  </template>

  <script>
  import OrganizationChart from 'vue-organization-chart';
  import 'vue-organization-chart/dist/orgchart.css';

  export default {
    components: {
      OrganizationChart,
    },
    parent: {
    records: {
      type: Object,
      required: true,
      },
    },
    mounted() {
     this.$nextTick(() => {
       const divElements = document.querySelectorAll('.node');

        divElements.forEach(divElement => {

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
        };
  </script>

  <style scoped>
  /* Add CSS styles for node content and conditional styling here */
  </style>
