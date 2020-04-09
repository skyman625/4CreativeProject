<template>
<div id="app">
  <div class="header">
    <h1>Music Recommendations</h1>
    <div id="nav">
    <router-link to="/admin">Upload/Edit</router-link> |
    <router-link to="/">Home</router-link> |
  </div>
  </div>

  <div class="content">

    <router-view />
  </div>
  <div class="footer">
    <a href="https://github.com/BYU-CS-260-Winter-2020/lab-4-museum-of-ordinary-objects-skyman625.git"> GitHub Repository</a>
  </div>
</div>
</template>

<style>
html {
  box-sizing: border-box;
}

.header img {
  height: 80px;
  width: auto;
  margin-right: 20px;
}

#side {
  gravity: right;
  display: flex;
  justify-content: right;
}

body {
  font-family: 'Montserrat', sans-serif;
  font-size: 16px;
  background: #fff;
  padding: 0px;
  margin: 0px;
}

/* Header */
.header {
  display: flex;
  justify-content: space-between;
  background: #4169E1;
  padding: 10px 100px;
  color: white;
}

.title {
  margin-top: 5px;
}

.header h1 {
  font-size: 30px;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #fff;
}

#nav a.router-link-exact-active {
  color: #FFC300;
}

.content {
  padding: 20px 100px;
  min-height: 500px;
}

/* Footer */
.footer {
  height: 50px;
  padding: 20px 100px 0px 100px;
  font-size: 12px;
}

.footer a {
  color: #000;
}

h1 {
  font-size: 20px;
}

h2 {
  font-size: 14px;
}
</style>
