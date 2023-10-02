<template>
  <div id="app">
    <div id="chart-containner"> <org-chart :records="formattedRecords"></org-chart></div>
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
    const level2Children = childrenByRole['PROVIDER_LEVEL_2'] || [];

    const level1Children = Object.keys(childrenByRole)
      .filter((role) => role !== 'PROVIDER_LEVEL_ADMIN' && role !== 'PROVIDER_LEVEL_2')
      .reduce((acc, role) => acc.concat(childrenByRole[role]), []);

    if (adminChildren.length > 0) {
      adminChildren.forEach((admin) => {
        if (level2Children.length > 0) {
          result.children.push({
            name: admin.name,
            title: 'PROVIDER_LEVEL_ADMIN',
            avatar: admin.avatar,
            id: admin.id,
            children: [
              {
                name: 'PROVIDER_LEVEL_2',
                title: 'PROVIDER_LEVEL_2',
                avatar: null,
                id: null,
                children: [
                  ...level2Children,
                  ...level1Children,
                ],
              },
            ],
          });
        } else {
          result.children.push({
            name: admin.name,
            title: 'PROVIDER_LEVEL_ADMIN',
            avatar: admin.avatar,
            id: admin.id,
            children: [
              ...level1Children,
            ],
          });
        }
      });
    } else if (level2Children.length > 0) {
      result.children.push({
        name: level2Children[0].name,
        title: 'PROVIDER_LEVEL_2',
        avatar: null,
        id: null,
        children: [
          ...level1Children,
        ],
      });
    } else {
      result.children = level1Children;
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
    return { children: this.formattedRecords};
  },
  },
};
</script>

<style>
/* Add global CSS styles here if needed */
</style>
