<template>
  <div id="app" class="app">
    <div class="hw-container">
      <h1 class="hw-page-title">House wizard</h1>
      <Wizard />
    </div>
  </div>
</template>

<script>
import Wizard from './components/Wizard';
console.log(Wizard)

export default {
  name: 'App',
  components: {
    Wizard
  }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Source+Sans+Pro&family=Ubuntu:wght@700&display=swap');

:root {
  --white: #fff;
  --lightgrey: #f8f8f8;
  --mediumgrey: #dddddd;
  --darkgrey: #495057;
  --darkblue: #25315a;
  --lightblue: #90a5f9;
  --pink: #ff385c;
  --red: #ff123d;
}

body {
  margin: 0;
  background: var(--lightgrey);
}

* {
  box-sizing: border-box;
}

#app {
  font-family: 'Source Sans Pro', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.app {
  background-image: url(https://images.pexels.com/photos/106399/pexels-photo-106399.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=750&w=1260);
  background-size: cover;
  background-position: center center;
  height: 100vh;
  margin: 0;


  > * {
    position: relative;
    z-index: 1;
  }

  &:after {
    content: "";
    display: block;
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    z-index: 0;
    background: rgba(22,35,70,.3);
  }
}

h1,
h2,
h3,
h4,
h5,
h6 {
  font-family: 'Ubuntu', sans-serif;
  margin: 0;
  color: var(--darkblue);
}

.hw-page-title {
  font-size: 3rem;
  text-align: center;
  text-shadow: 0 2px 4px rgba(0,0,0,0.2);
  color: var(--white);
  margin: 0 0 2rem 0;
  padding-top: 2rem;

  &:after{
    content: '.';
    color: var(--pink);
  }
}

button,
.btn {
  color: #fff;
  background-color: var(--pink);
  border: 0;
  border-radius: 2rem;
  padding: .75rem 3rem;
  font-size: 1rem;
  -webkit-appearance: none;
  transition: ease-in-out .2s background-color;
  cursor: pointer;
  display: inline-block;

  &:hover,
  &:focus {
    background-color: var(--red);
    outline: none;
  }
}

.hw-container {
  max-width: 768px;
  margin: 0 auto;
  padding: 0 1rem;
}
</style>
