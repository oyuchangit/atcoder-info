<template>
  <div id="app">
    <div id="app">
      <font-awesome-icon icon="user-secret" />
    </div>
    <HeaderArea/>
    <SearchArea @getUserInfo="getUserInfo" @getDateInfo="getDateInfo"/>
    <SelectArea/>
    <DataArea :userInfo="userInfo" :dateInfo="dateInfo" class="dataArea"/>

  </div>
</template>


<script>
import HeaderArea from './components/HeaderArea'
import SearchArea from './components/SearchArea'
import SelectArea from './components/SelectArea'
import DataArea from './components/DataArea'


export default{
  name: 'App',
  components: {
    HeaderArea,
    SearchArea,
    SelectArea,
    DataArea
  },
  data(){
    return{
      userInfo: [],
      dateInfo:{}
    }
  },
  methods: {

    getUserInfo(userInfo){
      this.userInfo = userInfo
    },
    getDateInfo(dateInfo){
      this.dateInfo = dateInfo
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #504548;
  /* margin-top: 60px; */
}


body{
    background-color: white;
    color: #504548;
}
header{
    background-color: #AEAFAF;
    color: #504548;
}
table{
    width: 100%;
    border-collapse: collapse;
}
table tr *{
    background-color: white;
}
table th,table td{
    color: #4A150D;
    border:solid 1px #bbb4b8;
    text-align: center;
    padding: 5px 0;
    width: 13vw;
}

a{
    text-decoration: none;
    color: #504548;
}
a:visited{
  color:#3a262c;
}
a:hover{
    color : #867e80;

}

.input{
    width: 100%; /*親要素いっぱい広げる*/
    padding: 10px 15px; /*ボックスを大きくする*/
    font-size: 16px;
    /* border-radius: 4px; ボックス角の丸み */
    border: 1px solid #756D74; /*枠線*/
    background-color: white;
    box-sizing: border-box; /*横幅の解釈をpadding, borderまでとする*/
    color: #504548;
    border-radius: 2px;
}
.input::placeholder {
  color: #D8D6DE;
}
.input:focus {
    z-index: 10;
    outline: 0;
    box-shadow: 0 0 3px 0 #7F6C68;
}




/* tooltipの表示 */
.tooltip {
  display: block !important;
  z-index: 10000;
}
.tooltip .tooltip-inner {
  background: #7F6C68;
  color: #E3E6E1;
  border-radius: 5px;
  padding: 5px 10px 4px;
}
.tooltip .tooltip-arrow {
  width: 0;
  height: 0;
  border-style: solid;
  position: absolute;
  margin: 5px;
  border-color: #7F6C68;
  z-index: 1;
}
.tooltip[x-placement^="top"] {
  margin-bottom: 5px;
}
.tooltip[x-placement^="top"] .tooltip-arrow {
  border-width: 5px 5px 0 5px;
  border-left-color: transparent !important;
  border-right-color: transparent !important;
  border-bottom-color: transparent !important;
  bottom: -5px;
  left: calc(50% - 5px);
  margin-top: 0;
  margin-bottom: 0;
}
.tooltip[x-placement^="bottom"] {
  margin-top: 5px;
}
.tooltip[x-placement^="bottom"] .tooltip-arrow {
  border-width: 0 5px 5px 5px;
  border-left-color: transparent !important;
  border-right-color: transparent !important;
  border-top-color: transparent !important;
  top: -5px;
  left: calc(50% - 5px);
  margin-top: 0;
  margin-bottom: 0;
}
.tooltip[x-placement^="right"] {
  margin-left: 5px;
}
.tooltip[x-placement^="right"] .tooltip-arrow {
  border-width: 5px 5px 5px 0;
  border-left-color: transparent !important;
  border-top-color: transparent !important;
  border-bottom-color: transparent !important;
  left: -5px;
  top: calc(50% - 5px);
  margin-left: 0;
  margin-right: 0;
}
.tooltip[x-placement^="left"] {
  margin-right: 5px;
}
.tooltip[x-placement^="left"] .tooltip-arrow {
  border-width: 5px 0 5px 5px;
  border-top-color: transparent !important;
  border-right-color: transparent !important;
  border-bottom-color: transparent !important;
  right: -5px;
  top: calc(50% - 5px);
  margin-left: 0;
  margin-right: 0;
}
/* .tooltip.popover .popover-inner {
  background: #f9f9f9;
  color: black;
  padding: 24px;
  border-radius: 5px;
  box-shadow: 0 5px 30px rgba(black, .1);
}
.tooltip.popover .popover-arrow {
  border-color: #f9f9f9;
} */
.tooltip[aria-hidden='true'] {
  visibility: hidden;
  opacity: 0;
  transition: opacity .15s, visibility .15s;
}
.tooltip[aria-hidden='false'] {
  visibility: visible;
  opacity: 1;
  transition: opacity .15s;
} 


</style>
