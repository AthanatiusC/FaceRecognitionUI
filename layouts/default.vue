<template>
  <div class="container-fluid main">
    <div class="row">
      <div class="col" style="padding:0px">
        <topbar></topbar>
      </div>
    </div>
    <div class="row">
      <div class="col-2 sidebar">
        <Sidebar></Sidebar>
      </div>
      <div class="col-10">
        <nuxt></nuxt>
      </div>
    </div>
  </div>
</template>

<style>
  html,body{
    height:100%;
    width: 100%;
    background-color:#16151d;
    color: #bababa;
  }

  a,a:visited,a:hover,a:focus{
    color:#bababa ;
    text-decoration: none;
  }
  .main{
    height:100%;
  }
  .sidebar{
    padding:0px;
  }
</style>