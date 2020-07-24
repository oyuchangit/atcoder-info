<template>
<header>
    <div class="headerArea">
        <h3 class="title">AtCoder Info.</h3>
        <ul class="nav">
            <li class="menuItem"><a href="https://github.com/oyuchangit/atcoder-info">GitHub</a></li>
            <li class="menuItem"><a href="https://atcoder.jp/">Atcoder</a></li>
        </ul>
    </div>
</header>
</template>

<style scoped>
header{
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    background: #C5C8C4;
    color: white;
    width: 100%;
    height: 40px;
}
.headerArea{
    display: flex;
    align-items: center;
    height: 40px;
}
.nav{
    display: inline-block;
}
.title{
    margin-right: auto;
    display: inline-block;
    padding-left: 15px;
    color: #E73825;
}
.menuItem{
    list-style: none;
    display: inline-block;
    padding: 10px;
    margin: 0 0 0 auto;

}


a{
    color:white;
}


a:hover{
    color:white;
    text-decoration: underline;
}

a:visited{
    color:white;
}

</style>