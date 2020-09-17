<template>
  <Layout>
    <!-- Page Header -->
    <header class="masthead" :style="{'background-image': `url(http://58.87.112.54:1337${general.cover.url})`}">
      <div class="overlay"></div>
      <div class="container">
        <div class="row">
          <div class="col-lg-8 col-md-10 mx-auto">
            <div class="site-heading">
              <h1>{{general.created_by.firstname + general.created_by.lastname}}</h1>
              <span class="subheading">{{general.desc}}</span>
            </div>
          </div>
        </div>
      </div>
    </header>
    <div class="container">
      <div class="row">
        <div class="col-lg-8 col-md-10 mx-auto">
          <div class="post-preview" v-for="post in posts" :key="post.node.id">
            <g-link :to="`/post/${post.node.id}`">
              <h2 class="post-title">
                {{ post.node.title }}
              </h2>
            </g-link>
            <p class="post-meta">Posted by
              <a>{{post.node.created_by.firstname + post.node.created_by.lastname}}</a>
              on {{post.node.createdAt}}</p>
            <p>
              <span v-for="tag in post.node.types" :key="tag.id">
                <g-link :to="`/types/`+tag.id">
                  {{ tag.title }}
                </g-link>  &nbsp;
              </span>
            </p>
            <hr>
          </div>
          <!-- Pager -->
          
          <Pager :info="pageInfo"/>
        </div>
      </div>
    </div>

  </Layout>
</template>
<page-query>
query ($page: Int, $id: String) {
  allStrapiPost(perPage: 10, page: $page, filter: {types: {id: {in: [$id]}}}) @paginate {
    pageInfo {
      totalPages
      currentPage
    }
    edges {
      node {
        title
        content
        id
        createdAt
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
  }
  allStrapiGeneral {
    edges {
      node {
        title
        desc
        id
        createdAt
        cover {
          url
          name
        }
        created_by {
          firstname
          lastname
        }
      }
    }
  }
}
</page-query>
<script>
import { Pager } from 'gridsome'
export default {
  components: {
    Pager
  },
  metaInfo: {
    title: 'Hello, world!'
  },
  computed: {
    posts (state) {
      return state.$page.allStrapiPost.edges
    },
    pageInfo (state) {
      return state.$page.allStrapiPost.pageInfo
    },
    general (state) {
      return state.$page.allStrapiGeneral.edges[0].node
    }
  }
}
</script>

<style>
.home-links a {
  margin-right: 1rem;
}
</style>
