<template>
  <div class="artic-item" @click="toDetail(item.originalUrl)">
    <div class="artic-item__info">
      <ul class="artic-item__meta">
        <li class="meta-item mate-item--post" v-if="item.type === 'post'">专栏</li>
        <li class="meta-item">{{ item.user.username }}</li>
        <li class="meta-item">{{ item.createdAt | formatTime }}</li>
        <li class="meta-item">
          <span v-for="(tag) in item.tags" :key="tag.id" class="label">{{ tag.title }}</span>
        </li>
      </ul>
      
      <p class="artic-item__title" v-html="highlight.title || item.title"></p>
      <p v-if="hasDesc" class="artic-item__desc" v-html="highlight.description || highlight.text || item.content"></p>
      <ul class="artic-item__action">
        <li class="action-item">
          <img class="action-item__icon" src="https://b-gold-cdn.xitu.io/v3/static/img/zan.e9d7698.svg">
          {{ item.likeCount || item.collectionCount }}
        </li>
        <li class="action-item">
          <img class="action-item__icon" src="https://b-gold-cdn.xitu.io/v3/static/img/comment.4d5744f.svg">
          {{ item.commentsCount }}
        </li>
      </ul>
    </div>
    <div v-if="item.screenshot" class="artic-item__cover" :style="'background-image: url('+item.screenshot+')'"></div>
  </div>
</template>

<script>
export default {
  props: {
    item: {
      type: Object,
      default: () => ({})
    },
    hasDesc: {
      type: Boolean,
      default: false
    },
    highlight: {
      type: Object,
      default: () => ({})
    }
  },
  methods: {
    toDetail(originalUrl) {
      if (!originalUrl) { return }
      if (originalUrl.includes('juejin')) {
        this.$router.push(`/detail/${originalUrl.split('/').pop()}`)
      } else {
        const a = document.createElement("a")
        a.href = originalUrl
        a.target = "_blank"
        a.rel = "noopener noreferrer"
        a.click()
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.artic-item {
  display: flex;
  align-items: center;
  padding: 20px 25px;
  cursor: pointer;

  &:hover{
    background: rgba(0,0,0,.01);
  }
}

.artic-item__info {
  flex: 1 1 auto;
}

.artic-item__cover {
  flex: 0 0 auto;
  width: 60px;
  height: 60px;
  margin-left: 30px;
  background-color: #f4f4f4;
  background-size: cover;
  border-radius: 2px;
}

.artic-item__meta {
  display: flex;
  align-items: center;
  font-size: 12px;
  color: #b2bac2;

  .meta-item {
    &::after {
      content: "·";
      margin: 0 5px;
      color: #b2bac2;
    }

    &:last-child::after {
      content: "";
    }
  }

  .mate-item--post {
    color: #b71ed7;
  }

  .label {
    &::after {
      content: " / ";
    }

    &:last-child::after {
      content: "";
    }
  }
}

.artic-item__title {
  display: block;
  margin: 10px 0 16px;
  line-height: 1.2;
  font-size: 17px;
  font-weight: 700;
  color: inherit;
  text-decoration: none;

  &:hover{
    text-decoration: underline;
  }

  /deep/ em{
    color: #e8001c;
  }
}

.artic-item__desc {
  width: 800px;
  margin-bottom: 14px;
  font-size: 13px;
  color: #5b6169;
  text-overflow: ellipsis;
  white-space: nowrap;
  overflow: hidden;

  /deep/ em{
    color: #e8001c;
  }
}

.artic-item__action {
  display: flex;
  align-items: center;
  font-weight: 700;
  font-size: 13px;
  color: #b2bac2;

  .action-item {
    display: flex;
    align-items: center;
    height: 26px;
    padding: 0 10px;
    border: 1px solid #edeeef;

    .action-item__icon{
      margin-right: 3px;
    }

    &:last-child {
      border-left: 0;
    }
  }
}
</style>