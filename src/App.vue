<template>
  <div id="app">
    <div id="chart-containner"> <org-chart :records="records"></org-chart></div>
  </div>
</template>

<script>
import OrgChart from './components/OrgChart.vue';
import recordsData from '../data/records.json';

export default {
  components: {
    'org-chart': OrgChart,
  },
  data() {
    return {
      formattedRecords: [],
    };
  },
  created: function () {
      this.formattedRecords = this.transformObject(recordsData);
  },
  methods: {
  requiredFormat: function (data) {
    const result = {
      id: data.type.id,
      name: data.name,
      title: data.type.name,
      avatar: data.type.image,
      children: [],
    };

    if (data.accesses && data.accesses.length > 0) {
      // Separate children into groups based on role
      const childrenByRole = {};

      data.accesses.forEach((access) => {
        const child = {
          name: access.user.name,
          title: access.role.name,
          avatar: access.user.image,
          id: access.user.id,
        };

        if (!childrenByRole[access.role.name]) {
          childrenByRole[access.role.name] = [];
        }

        childrenByRole[access.role.name].push(child);
      });

      const adminChildren = childrenByRole['PROVIDER_LEVEL_ADMIN'] || [];

      const otherChildren = Object.keys(childrenByRole)
        .filter((role) => role !== 'PROVIDER_LEVEL_ADMIN')
        .reduce((acc, role) => acc.concat(childrenByRole[role]), []);

      // Check if there are "PROVIDER_LEVEL_ADMIN" nodes
      if (adminChildren.length > 0) {
        adminChildren.forEach((admin) => {
          result.children.push({
            name: admin.name,
            title: 'PROVIDER_LEVEL_ADMIN',
            avatar: admin.avatar,
            id: admin.id,
            children: [...otherChildren],
          });
        });
      } else {
        result.children = otherChildren;
      }
    }

    return result;
  },
  transformObject: function (arr) {
    return arr.map((item) => this.requiredFormat(item));
  },
},
  computed: {
      records() {
    return {'id': 0, children: this.formattedRecords};
  },
  },
};
</script>

<style>
/* Add global CSS styles here if needed */
</style>
