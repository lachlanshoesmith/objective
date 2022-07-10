<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Roboto+Slab:wght@400;500;700;800;900&display=swap');

#app {
  font-family: 'Roboto Slab', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  background: #E5FFF0;
  padding: 30px;
  h1 {
    color: #1B4E2F;
    font-size: 3rem;
  }
}
p {
  color: #2A3F32;
  font-size: 1.5rem;
}

.small {
  color: #66997A;
  font-size: 1rem;
}

a {
  color: #47BD76;
  transition: color .2s linear;
  text-decoration: none;
  &:hover {
    color: #1B4E2F;
  }
}

main {
  max-width: 40vw;
  margin-left: 30vw;
  margin-right: 30vw;
}

label {
  margin-right: 8px;
}

section {
  margin-top: 2vh;
}

button {
  background: #E5FFF0;
  color: #1B4E2F;
  border: none;
  padding: 32px;
  border-radius: 32px;
  font-family: 'Roboto Slab', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  font-size: 2rem;
  font-weight: 700;
  transition: all 0.2s linear;
  &:hover {
    background: #1B4E2F;
    color: #E5FFF0;
    cursor: pointer;
  }
}
</style>
