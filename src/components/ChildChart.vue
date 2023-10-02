<template>
    <div class="modal">
      <button class="back-button" @click="goBack">&lt; Back</button>
      <organization-chart :datasource="childrenData" id="chart-container"></organization-chart>
    </div>
  </template>

  <script>
  import OrganizationChart from 'vue-organization-chart';

  export default {
    components: {
        OrganizationChart,
    },
    props: {
    childrenData: {
      type: Object,
      required: true,
      },
    },
    methods: {
    goBack() {
      this.$emit('back');
    }
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
/* Add necessary styling */
</style>
