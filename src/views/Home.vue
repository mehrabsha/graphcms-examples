<template>
  <div class="home" id="products">
    <img alt="Vue logo" src="../assets/logo.png" />
    <router-link
      v-for="post in edges"
      :to="{ name: 'Product', params: { slug: post.title } }"
      :key="post.title"
    >
      <article>
        <h1>{{ post.title }}</h1>
        <p>{{ post.desc }}</p>
      </article>
    </router-link>
  </div>
</template>

<script>
// @ is an alias to /src
export default {
  name: 'Home',
  components: {},
  data() {
    return {
      edges: [],
      loading: true,
      errors: [],
    };
  },
  async created() {
    const response = await fetch(
      'https://api-eu-central-1.graphcms.com/v2/ckihde7o9k1nh01xmclrv9cy6/master',
      {
        method: 'POST',
        body: JSON.stringify({
          query: `{  page: simplesConnection() {edges {node {id title}}}}`,
        }),
      }
    );

    const data = await response.json();
    console.log(data);
    this.edges = data.data.page.edges;
    console.log(this.edges);
    
    
    this.errors = data.errors;
    this.loading = false;
    this.products = data.products;
  },
};
</script>
