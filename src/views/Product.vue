<template>
  <div class="product" v-if="post">
    <h1>{{ post.title }}</h1>
    <p>{{ post.desc }}</p>

  </div>
</template>
<script>
export default {
  data() {
    return {
      product: null,
      error: null,
      loading: true,
      post : null
    };
  },
  created() {
    // fetch the data when the view is created and the data is
    // already being observed
    this.fetchData();
  },
  watch: {
    // call again the method if the route changes
    $route: 'fetchData',
  },
  methods: {
    async fetchData() {
      this.error = null;
      this.post = null;
      this.loading = true;
      // replace `getPost` with your data fetching util / API wrapper
      try {
        const response = await fetch(
          'https://api-eu-central-1.graphcms.com/v2/ckihde7o9k1nh01xmclrv9cy6/master',
          {
            method: 'POST',
            body: JSON.stringify({
              query: `{values: simple(where: {id: $id}) { desc id title }}`,
              variables: {
                id: this.$route.params.id,
              },
            }),
          }
        );
        const data = await response.json();
        console.log(data);
        this.post = data.data.values;
        console.log(this.post);
        this.loading = false;
        this.error = data.error;
        this.product = data.product;
      } catch (e) {
        // handle error
      }
    },
  },
};
</script>
