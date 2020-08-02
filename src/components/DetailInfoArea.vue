<template>

    <div class="summaryInfo">
        <div class="dataAreaInfo">
            Number of
            <span class="dataAreaInfoAC">AC</span>cepted problems:
            <span class="countAC">{{detailInfo.countAC}}</span>
            <span>/{{detailInfo.countProblems}}</span>
            <span class="angleIcon" v-on:click="clickDetailIcon">
                <span v-if="!this.detailInfoArea.isVisible"><font-awesome-icon :icon="['fas', 'angle-down']" /></span>
                <span v-if="this.detailInfoArea.isVisible"><font-awesome-icon :icon="['fas', 'angle-up']" /></span>
            </span>
        </div>

        <div v-if="this.detailInfoArea.isVisible" class="detailArea">
            <div class="detailDiv">
                <div><h3>Total problems</h3></div>
                <div>
                    <div class="totalProblems"><h3>{{this.detailInfo.countProblems}}</h3></div>
                </div>
            </div>

            <div class="detailDiv">
                <div><h3>Number of submission</h3></div>
                <div class="infoArea">
                    <div>
                        <div class="detailLabel">Total</div> : <span class="number">{{this.detailInfo.totalSubmit}}</span> 
                    </div>
                    <div>
                        <div class="detailLabel">AC</div> : <span class="number">{{this.detailInfo.totalAC}}</span>
                    </div>
                    <div>
                        <div class="detailLabel">WA + TLE + RE</div> : <span class="number">{{this.detailInfoArea.totalNotAC}}</span>
                    </div>

                    <div>
                        <div class="detailLabel">WA</div> : <span class="number">{{this.detailInfo.totalWA}}</span>
                    </div>
                    <div>
                        <div class="detailLabel">TLE</div> : <span class="number">{{this.detailInfo.totalTLE}}</span>
                    </div>
                    <div>
                        <div class="detailLabel">RE</div> : <span class="number">{{this.detailInfo.totalRE}}</span>
                    </div>

                </div>
                <!-- <div class="annotation">* Information based on submission</div> -->


            </div>
            <div class="detailDiv">
                <div><h3>Number of result</h3></div>
                <div>
                    <div class="detailLabel">Total</div> : <span class="number">{{this.detailInfo.totalSolved}}</span>
                </div>

                <div>
                    <div class="detailLabel">AC</div> : <span class="number">{{this.detailInfo.countAC}}</span>
                </div>
                <div>
                    <div class="detailLabel">WA + TLE + RE</div> : <span class="number">{{this.detailInfoArea.countNotAC}}</span>
                </div>
                <!-- <div class="annotation">* Information based on result</div> -->


            </div>

        </div>
      
    </div>

</template>

<script>

export default {
    props: {
        detailInfo: Object
    },
    data() {
        return {
            detailInfoArea: {
                isVisible: false,
                // totalNotAC: 0,
                countNotAC: 0
            }
        }
    },
    methods: {
        clickDetailIcon: function(){

            let detailInfoArea = this.detailInfoArea

            if(detailInfoArea.isVisible){
                this.detailInfoArea.isVisible = false;
            }else{
                this.detailInfoArea.isVisible = true;
            }
        },
        initDetailInfo: function(){
            this.detailInfoArea.totalAC = this.detailInfo.totalAC
            this.detailInfoArea.countNotAC = this.detailInfo.totalSolved - this.detailInfo.countAC;
            this.detailInfoArea.totalNotAC = this.detailInfo.totalSubmit - this.detailInfo.totalAC;
        }
    },
    created() {
        this.initDetailInfo();
    },
    watch: {
        detailInfo: {
            handler: function(){
                this.initDetailInfo();
            },
            deep: true
        }
        
    }
}
</script>

<style>
.summaryInfo{
    margin-bottom: 20px;
}

.dataAreaInfo{
    margin-top: 20px auto;
}
.dataAreaInfoAC{
    font-weight: bold;
    color: #8fabe0;
}
.dataAreaInfo .countAC{
    font-weight: bold;
}
.angleIcon{
    margin-left: 5px;
    font-size: x-large;
}

.angleIcon:hover{
    border-bottom: none;
    color: #867e80;
}


.detailDiv{
    display: inline-block;
    margin: 20px;
    text-align: left;
    vertical-align: top;

}
.detailLabel{
    display: inline-block;
    width:120px;
    text-align: left;
}
.detailDiv .number{
    font-weight: bold;
}
.detailDiv .annotation{
    margin-top: 5px;
    font-size: small;
    width: 180px;
}
.detailDiv .totalProblems{
    text-align: center;
}

</style>