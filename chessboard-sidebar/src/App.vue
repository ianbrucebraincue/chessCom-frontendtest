<template>
  <ChessBoard/>
</template>

<script>

import ChessBoard from './components/ChessBoard.vue'

export default {
  name: 'App',
  components: {
    ChessBoard,
  }
}
</script>

<style>
@import './assets/styles/normalize.css';
@import './assets/styles/main.css';
</style>
