<template>
<header>
  <router-link to="/">
    <h1>Ciné<span>Vue</span>
    </h1>
  </router-link>
</header>
<main>
  <router-view/>
</main>
<footer>
  <div>©Copyright Alain ORLUK - 2022</div>
</footer>
</template>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Amatic+SC:wght@700&display=swap');
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Fira Sans', sans-serif;
    font-size: 16px;
    &::selection {
      background: transparentize(#42B883, .5);
    }
  }

  body {
    background-color: #35495E;
  }
  a {
    text-decoration: none;
  }

  header {
    position: sticky;
    top: 0;
    z-index: 3;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: .625rem 1rem;
    background-color: #2C3D4E;
    box-shadow: 0px 0px 6px rgba(0, 0, 0, 0.1);
    h1, span {
      color: #FFF;
      font-size: 2rem;
    }
    span {
      color: #42B883;
    }
  }
  footer {
    position: fixed;
    bottom: 0;
    width: 100%;
    height: 2.5rem;
    text-align: center;
    background-color: #2C3D4E;
    box-shadow: 0px 0px 6px rgba(0, 0, 0, 0.1);

    div {
      position: relative;
      top: 5px;
      font-family: 'Amatic SC', cursive;
      font-size: 1.5rem;
    }
  }
</style>
