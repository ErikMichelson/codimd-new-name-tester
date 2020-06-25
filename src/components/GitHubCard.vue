<template>
  <div class="github-card" :data-href="data['html_url']" onclick="window.open(this.getAttribute('data-href'));">
    <img :src="data['avatar_url']" :alt="data['name']">
    <div class="details">
      <span class="username">{{ data['name'] }}</span>
      <span class="handle">@{{ data['login'] }}</span>
      <br>
      <span class="description">{{ data['bio'] }}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'GitHubCard',
  props: {
    data: Object
  }
}
</script>

<style scoped>
.github-card {
  background-color: #d6d6d6;
  padding: 0.25rem;
  display: flex;
  flex-flow: row nowrap;
  cursor: pointer;
  color: #000000;
  width: min-content;
  margin: 0.5rem 1rem 0.5rem 0;
}

img {
  width: 64px;
  height: 64px;
}

.details {
  margin-left: 0.5rem;
}

.username {
  font-weight: bold;
  margin-right: 0.5rem;
}

.handle {
  font-weight: lighter;
  color: #5b5656;
}

.description {
  font-style: italic;
  color: #5b5656;
}
</style>
