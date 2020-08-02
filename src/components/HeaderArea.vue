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
    background: #3A3C4C;
    color: #D2DDF1;
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
}
.menuItem{
    list-style: none;
    display: inline-block;
    padding: 10px;
    margin: 0 0 0 auto;

}


a{
    color:#D8D6DE;
}


a:hover{
    color:#D8D6DE;
    text-decoration: underline;
}

a:visited{
    color:#D8D6DE;
}

</style>