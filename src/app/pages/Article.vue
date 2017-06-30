<template>
<article v-if="article" class="blog-article-container">
  <div v-if="article.photo" class="post-banner">
    <img :src="article.photo">
  </div>

  <div class="container mt-3">
    <div class="row">
      <article class="blog-article col-xs-12 col-lg-8 offset-lg-2">
        <small class="blog-article-links">
          <small class="text-uppercase">
            <nuxt-link :to="{ name: '@nuxtjs/blog:index' }">Back to blog</nuxt-link>
          </small>

          <small class="medium text-uppercase text-right" v-if="article.attributes.medium">
            <a :href="article.attributes.medium" target="_blank" rel="noreferrer noopener">Read on Medium</a>
          </small>
        </small>

        <h1 class="blog-article-title">{{ article.attributes.title }}</h1>

        <div class="blog-article-meta">
          <nuxt-link v-if="article.attributes.collection"
                     :to="{ name: '@nuxtjs/blog:collection', params: { id: article.attributes.collection.id }}"
                     :title="`This article is part of '${article.attributes.collection.name}' collection.`"
          >{{ article.attributes.collection.name }}
          </nuxt-link>
          <span v-if="article.attributes.collection"> | </span>
          <time :datatime="article.attributes.date">Published on {{ article.attributes.date | formatDate }} </time>
          <time
              v-if="article.attributes.updated_at && article.attributes.date !== article.attributes.updated_at"
              :datatime="article.attributes.updated_at"
          >| Last updated on {{ article.attributes.updated_at | formatDate }}
          </time>

          <div class="blog-article-tags-wrapper d-inline" v-if="article.attributes.tags">
            <TagPreview v-for="tag in article.attributes.tags"
                        :key="tag"
                        :id="tag.id"
                        :name="tag.name"
            />
          </div>
        </div>

        <div class="blog-article-content" v-html="article.rendered.replace(/<h1[^>]*>([^<]*)<\/h1>/i, '')"></div>
      </article>

      <div v-if="comments === true && disqus" class="col-xs-12 col-lg-8 offset-lg-2 blog-article-comments">
        <DisqusComments v-bind="disqus"/>
      </div>
    </div>
  </div>

  <div
      v-if="article.next"
      class="blog-article-next mt-3"
      :style="{ 'background-image': article.next.attributes.image ? `url(${article.next.attributes.image})` : 'rgba(255, 255, 255, 0)' }"
  >
    <div class="title">
      <small>Up next</small>
      <router-link :to="article.next.id">{{ article.next.attributes.title }}</router-link>
    </div>
  </div>
</article>

<div v-else>
  Loading....
</div>
</template>

<script>
import Article from '../mixins/article'
import TagPreview from '../components/Tag.vue'
import DisqusComments from 'vue-disqus/VueDisqus.vue'

export default {
  extends: Article,
  components: { TagPreview, DisqusComments }
}
</script>

<style lang="scss">
$brand-primary: #ff574a !default;
$default-color: #2b2b2b !default;
$font-family-serif: inherit !default;

.blog-article {
  font-size: 1.2rem;
  line-height: 1.618;
  text-rendering: optimizeLegibility;

  p {
    color: $default-color;
  }

  p:not(.skip) {
    font-family: $font-family-serif;
  }

  blockquote {
    background: #f9f9f9;
    border-left: 10px solid #ccc;
    margin: 1.5em 10px;
    padding: 0.5em 10px;
    quotes: "\201C""\201D""\2018""\2019";
  }

  blockquote > * {
    opacity: 0.75;
  }
}

.blog-article-banner {
  width: 100%;
  overflow: hidden;
  img {
    width: 100%;
  }
}

.blog-article-links {
  display: flex;
  > * {
    flex: 1;
  }
}

.blog-article-meta {
  font-size: .75rem;
  opacity: .9;
  padding-left: 1px;
}

.blog-article-next {
  padding: 3em 1em;
  text-align: center;

  background-color: white;
  background-position: center;
  background-size: cover;

  display: flex;
  align-items: center;
  justify-content: center;

  position: relative;
  min-height: 320px;

  &:after {
    content: '';
    display: block;
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;

    background: rgba(0, 0, 0, 0.75);
  }

  .title {
    position: relative;
    font-size: 2.5rem;
    z-index: 2;
  }

  a {
    color: white;
  }

  small {
    display: block;
    text-align: left;
    font-size: .8rem;
    letter-spacing: 3px;
    text-transform: uppercase;
    color: $brand-primary;
  }
}

.blog-article-container {
  .collection-list .active:before {
    margin-left: -1rem;
    margin-right: 0.4rem;
    content: "◦";
  }
}

.blog-article-comments {
  margin-top: 2rem;
}

.blog-article-tags-wrapper {

}
</style>