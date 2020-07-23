<template>
    <ul class="holder" role="menu" >
        <div class="title"><h4>Athanatius LLC</h4> </div>
        <div class="divider"></div>
        <li ><NLink class="page-list" to="/">Home</NLink></li>
        <li ><NLink class="page-list" to="/users">Users</NLink></li>
        <li ><NLink class="page-list" to="/tools">Tools</NLink></li>
        <li ><NLink class="page-list" to="/settings">Settings</NLink></li>
    </ul>
</template>

<style>
.holder{
    height: auto;
    background-color: #1d1b26;
    margin: 20px;
    padding: 0px 0px 20px 0px;
    border-radius: 5px;
    list-style: none;
    text-decoration: none;
    text-align: center;
    overflow: auto;
}
.title{
    margin:10px
}
.divider{
    border-bottom:solid #312f40;
    border-width: thin;
    margin: 5px 0px 5px 0px;
}

.page-list{
    display: block;
    font-size: 13pt;
    margin:0px;
    padding:10px;
}
.page-list:hover{
    background-color: #292736;
}
</style>

<script>
    export default{
        
    }
</script>