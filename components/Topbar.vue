<template>
    <ul class="holder-topbar">
        <li class="topbar-list">one</li>
        <li class="topbar-list">two</li>
        <li class="topbar-list">three</li>
        <li class="topbar-list">four</li>
    </ul>
</template>

<style>
    .holder-topbar{
        display: flex;
        background-color: #1d1b26 ;
        text-align: center;
        text-decoration: none;
        list-style: none;
        width: 100%;
        justify-content: center;
        padding:0px;
        margin:0px;
    }
    .topbar-list{
        padding: 8px 30px 8px 30px;
    }
    .topbar-list:hover{
        background-color: #292736;
    }
</style>