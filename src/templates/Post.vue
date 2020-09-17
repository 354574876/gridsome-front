<template>
  <Layout>
    <header class="masthead" :style="{'background-image': `url(http://58.87.112.54:1337${post.cover.url})`}">
      <div class="overlay"></div>
      <div class="container">
        <div class="row">
          <div class="col-lg-8 col-md-10 mx-auto">
            <div class="site-heading">
              <h1>{{post.title}}</h1>
            </div>
          </div>
        </div>
      </div>
    </header>
    <article>
      <div class="container">
        <div class="row">
          <div class="col-lg-8 col-md-10 mx-auto" v-html="mdToHtml(post.content)">
          </div>
        </div>
      </div>
    </article>
  </Layout>
</template>
<page-query>
query ($id: ID!) {
  strapiPost(id: $id) {
    title
    content
    cover {
      url
      name
    }
    created_by {
      firstname
      lastname
    }
    types {
      id,
      title
    }
  }
}
</page-query>
<script>
var MarkdownIt = require('markdown-it'),
    md = new MarkdownIt();
export default {
  name: 'Post',
  computed: {
    post (state) {
      return state.$page.strapiPost
    }
  },
  methods: {
    mdToHtml (value) {
      return md.render(value)
    }
  }
}
</script>

<style>

</style>