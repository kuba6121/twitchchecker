<template>
  <div id="app">
    <Header />
    <Main />
  </div>
</template>

<script>
import Header from '@/components/Header';
import Main from '@/components/Main';
import axios from 'axios';
export default {
  name: 'App',
  components: {
    Header,
    Main
  },
  created() {
    axios.defaults.headers.common['Client-ID'] = '1b9zd7ak000gejcksy12h3vizswl7z';
    axios.defaults.headers.common['Accept'] = 'application/vnd.twitchtv.v5+json';
  }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Nerko+One&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Poppins&display=swap');
body {
  margin: 0;
  padding: 0;
  font-family: 'Poppins', sans-serif;
  background-color: #16161a;
}
button,
input,
textarea,
select {
  font-family: "Poppins", sans-serif;
}
p, a {
  color: #94a1b2;
}
a {
  text-decoration: none;
}
h1, h2, h3, h4, h5, h6 {
  color: #fffffe;
}
button {
  background-color: #7f5af0;
  color: #fffffe;
  border: none;
  cursor: pointer;
}
</style>
