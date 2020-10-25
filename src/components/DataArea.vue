<template>
    <div class="dataArea">
        <DetailInfoArea :detailInfo="detailInfo"/>
        <table class="abc_data">
        <tbody>
            <tr v-for="problemInfo in problemsInfoTable" :key="problemInfo.contest_id">
                <td class='problemIdTableCell tableCell'>
                    <p :class="problemInfo.p0_status"><a :href="problemInfo.contest_url" target="_blank" rel="noopener noreferrer">{{problemInfo.contest_id}}</a></p>
                </td>
                <td :class="problemInfo.p0_class"><a :href="problemInfo.p0_url" target="_blank" rel="noopener noreferrer">{{problemInfo.p0_title}}</a>
                    <p v-if="problemInfo.p0_hasResult" v-tooltip.bottom="problemInfo.p0_results_tooltip" class="resultP">
                        <a :href="problemInfo.p0_resultURL" target="_blank" rel="noopener noreferrer" class="resultLink">{{problemInfo.p0_results}}</a>
                    </p>
                </td>
                <td :class="problemInfo.p1_class"><a :href="problemInfo.p1_url" target="_blank" rel="noopener noreferrer">{{problemInfo.p1_title}}</a>
                    <p v-if="problemInfo.p1_hasResult" v-tooltip.bottom="problemInfo.p1_results_tooltip" class="resultP">
                        <a :href="problemInfo.p1_resultURL" target="_blank" rel="noopener noreferrer" class="resultLink">{{problemInfo.p1_results}}</a>
                    </p>
                </td>
                <td :class="problemInfo.p2_class"><a :href="problemInfo.p2_url" target="_blank" rel="noopener noreferrer">{{problemInfo.p2_title}}</a>
                    <p v-if="problemInfo.p2_hasResult" v-tooltip.bottom="problemInfo.p2_results_tooltip" class="resultP">
                        <a :href="problemInfo.p2_resultURL" target="_blank" rel="noopener noreferrer" class="resultLink">{{problemInfo.p2_results}}</a>
                    </p>
                </td>
                <td :class="problemInfo.p3_class"><a :href="problemInfo.p3_url" target="_blank" rel="noopener noreferrer">{{problemInfo.p3_title}}</a>
                    <p v-if="problemInfo.p3_hasResult" v-tooltip.bottom="problemInfo.p3_results_tooltip" class="resultP">
                        <a :href="problemInfo.p3_resultURL" target="_blank" rel="noopener noreferrer" class="resultLink">{{problemInfo.p3_results}}</a>
                    </p>
                </td>
                <td :class="problemInfo.p4_class"><a :href="problemInfo.p4_url" target="_blank" rel="noopener noreferrer">{{problemInfo.p4_title}}</a>
                    <p v-if="problemInfo.p4_hasResult" v-tooltip.bottom="problemInfo.p4_results_tooltip" class="resultP">
                        <a :href="problemInfo.p4_resultURL" target="_blank" rel="noopener noreferrer" class="resultLink">{{problemInfo.p4_results}}</a>
                    </p>
                </td>
                <td :class="problemInfo.p5_class"><a :href="problemInfo.p5_url" target="_blank" rel="noopener noreferrer">{{problemInfo.p5_title}}</a>
                    <p v-if="problemInfo.p5_hasResult" v-tooltip.bottom="problemInfo.p5_results_tooltip" class="resultP">
                        <a :href="problemInfo.p5_resultURL" target="_blank" rel="noopener noreferrer" class="resultLink">{{problemInfo.p5_results}}</a>
                    </p>
                </td>
            </tr>
        </tbody>
        </table>

    </div>
</template>

<script>
import Vue from 'vue'
import VTooltip from 'v-tooltip'
import DetailInfoArea from './DetailInfoArea'
Vue.use(VTooltip)

export default {
    components: {
        DetailInfoArea
    },
    props: {
        userInfo: Array,
        dateInfo: Object
    },
    data() {
        return {
            // APIから取得した素のデータ
            problemsInfo: [],

            // 画面描画用に加工したデータ
            problemsInfoTable: [],

            // コンテストIdと、画面描画用に加工したデータの行番号のマッピング
            mappingConstestAndIndex: [],


            detailInfo: {
                // 画面表示用AC数
                countAC: 0,

                // 問題総数
                countProblems: 0,

                
                totalAC:0,
                totalWA:0,
                totalTLE:0,
                totalRE:0,
                totalSolved:0,
                totalSubmit:0

            },
            
            // データエリア用日付情報
            dateInfoDataArea: {
                from: {
                    selectedDate: new Date(2013, 10, 12, 21, 0, 0)
                },
                to: {
                    selectedDate: new Date()
                }
            }
        }
    },
    methods: {
        // テーブルエリア初期化
        initDataArea: function(){
            const url = 'https://kenkoooo.com/atcoder/resources/problems.json'

            // APIからデータ取得
            fetch(url)
                .then((data) => data.json())
                .then(obj => {

                    this.problemsInfo = obj;

                    let problemsInfoTable = [];
                    let currContestId = this.problemsInfo[0].contest_id;
                    let problemsInfoRecord = {};

                    let problemsInfo = this.problemsInfo;
                    problemsInfo.sort(function(a,b){
                        if(a.contest_id<b.contest_id) return -1;
                        if(a.contest_id > b.contest_id) return 1;
                        return 0;
                    });

                    let currProblemNum = 0

                    // テーブル作成
                    for (let i = 0; i < problemsInfo.length; i++){

                        // ABCだけ対象にする
                        if(problemsInfo[i].contest_id.indexOf('abc') !== -1){

                            // 問題数の追加
                            this.detailInfo.countProblems += 1


                            // 違うコンテストになったときの処理
                            if (currContestId !== problemsInfo[i].contest_id){

                                // コンテストIdを代入
                                problemsInfoRecord['contest_id'] = currContestId;

                                // コンテストURLを代入
                                problemsInfoRecord['contest_url'] = 'https://atcoder.jp/contests/' + currContestId;

                                // コンテストIdを更新
                                currContestId = problemsInfo[i].contest_id;

                                // 問題情報を追加
                                problemsInfoTable.push(problemsInfoRecord);

                                // 初期化
                                problemsInfoRecord = {};
                                currProblemNum = 0;
                            }

                            // 問題情報の代入
                            problemsInfoRecord['p' + currProblemNum] = problemsInfo[i].id;

                            // 問題のタイトルを代入
                            problemsInfoRecord['p' + currProblemNum + '_title'] = problemsInfo[i].title;

                            // 問題のURLを代入
                            let url = 'https://atcoder.jp/contests/' + problemsInfo[i].contest_id + '/tasks/' + problemsInfo[i].id;
                            problemsInfoRecord['p' + currProblemNum + '_url'] = url;

                            // 問題の回答状況を代入
                            problemsInfoRecord['p' + currProblemNum + '_results'] = '';

                            // ツールチップ（空）を代入
                            problemsInfoRecord['p' + currProblemNum + '_results_tooltip'] = '';

                            // セルのClassを設定
                            problemsInfoRecord['p' + currProblemNum + '_class'] = 'problemTableCell tableCell';

                            // 問題の回答済みフラグを初期化
                            problemsInfoRecord['p' + currProblemNum + '_hasResult'] = false

                            // AC済みフラグを初期化
                            problemsInfoRecord['p' + currProblemNum + '_hasAC'] = false


                            currProblemNum += 1;
                        }
                    }

                    this.problemsInfoTable = problemsInfoTable.reverse();

                    // マッピングの更新
                    const len_probInfo = this.problemsInfoTable.length;
                    for (let i=0; i<len_probInfo; i++){

                        const contest_id = this.problemsInfoTable[i].contest_id;
                        this.mappingConstestAndIndex[contest_id] = i;
                    }

            })

            

        },

        // ユーザー変更時, 日付情報変更時アクション
        createTableData: function(){

            // 表示用テーブルを初期化
            this.initProblemsInfoTable();

            // userInfoの複製
            let userInfo_dup = Object.assign([], this.userInfo);

            // userInfoテーブルの降順sort(key: epoch_second)
            function desSortByEpochSecond(){
                userInfo_dup.sort((a, b) => {
                    return a.epoch_second -b.epoch_second;
                });
            }
            desSortByEpochSecond();


            const len_userInfo = userInfo_dup.length;
            for (let i=0; i<len_userInfo; i++){

                // コンテストId
                const curr_constest_id = userInfo_dup[i].contest_id;

                // ABCの情報のみ抽出
                if(curr_constest_id.indexOf('abc') === -1){ continue; }


                // 回答日時(UnixTime)
                const curr_solved_time = userInfo_dup[i].epoch_second;

                
                // 日付カラ⇒UnixTime変換
                const search_time_from = this.dateInfoDataArea.from.selectedDate.getTime() / 1000;


                // 日付マデ⇒UnixTime変換
                const search_time_to = this.dateInfoDataArea.to.selectedDate.getTime() / 1000;


                // 回答日時が日付カラ以前である場合は処理しない
                if(curr_solved_time < search_time_from){
                    continue;
                }

                // 回答日時が日付マデ以降である場合は処理しない
                if(curr_solved_time > search_time_to){
                    continue;
                }

                const curr_table_index = this.mappingConstestAndIndex[curr_constest_id];
                const curr_result = userInfo_dup[i].result;
                const curr_problem_id = userInfo_dup[i].problem_id;
                // const curr_problem_timestamp = userInfo_dup[i].

                const curr_epoch_second = userInfo_dup[i].epoch_second;
                const curr_datetime = this.getDateTimeByEpochSecond(curr_epoch_second);

                // 6問（A～F）を確認
                for (let j=0; j<6; j++){
                    const problemType = 'p' + j;
                    const problemId = this.problemsInfoTable[curr_table_index][problemType];

                    // ProblemIdと一致するか確認
                    if (problemId !== curr_problem_id){ continue; }


                    // problemIdと一致する場合、結果を更新する
                    const problemResults = problemType + '_results';
                    let currResults = this.problemsInfoTable[curr_table_index][problemResults];
                    this.problemsInfoTable[curr_table_index][problemResults] = currResults + ' ' + curr_result;

                    // 回答情報を更新
                    const AC = 'AC'
                    const WA = 'WA'
                    const TLE = 'TLE'
                    const RE = 'RE'
                    if(curr_result === AC){
                        this.detailInfo.totalAC += 1;
                    }
                    else if(curr_result === WA){
                        this.detailInfo.totalWA += 1;
                    }
                    else if(curr_result === TLE){
                        this.detailInfo.totalTLE += 1;
                    }
                    else if(curr_result === RE){
                        this.detailInfo.totalRE += 1;
                    }
                    this.detailInfo.totalSubmit += 1;

                    // 回答済み問題総数
                    const hasResult = problemType + '_hasResult';
                    if(!this.problemsInfoTable[curr_table_index][hasResult]){
                        this.detailInfo.totalSolved += 1;
                    }

                    // 回答済みフラグを立てる
                    this.problemsInfoTable[curr_table_index][hasResult] = true;

                    const resultURL = problemType + '_resultURL';
                    const userId = userInfo_dup[i].user_id
                    const resultURLText = 'https://atcoder.jp/contests/' + curr_constest_id + '/submissions?f.Task='
                                            + curr_problem_id + '&f.Language=&f.Status=&f.User=' + userId
                    this.problemsInfoTable[curr_table_index][resultURL] = resultURLText


                    // ツールチップの表示
                    const problemResultsTooltip = problemType + '_results_tooltip';
                    let currResultsTooptip = this.problemsInfoTable[curr_table_index][problemResultsTooltip];

                    const toolTipConetnts = this.makeToolTipContent(currResultsTooptip, curr_result + ':' + curr_datetime);
                    this.problemsInfoTable[curr_table_index][problemResultsTooltip] = toolTipConetnts;


                    // セルの色を変える
                    const defaultCellClass = 'problemTableCell tableCell'
                    const cellClass = problemType + '_class';
                    const hasAC = problemType + '_hasAC'

                    // 未ACならnotACのクラスを付与
                    if (!this.problemsInfoTable[curr_table_index][hasAC]){
                        const notACClassName = ' notAC'
                        this.problemsInfoTable[curr_table_index][cellClass] = defaultCellClass + notACClassName;
                    }

                    // ACが一つでもあった場合の処理
                    if (curr_result === 'AC'){

                        // その問題で出現した初めてのACだったとき、AC総数に1をプラスする
                        if(!this.problemsInfoTable[curr_table_index][hasAC]){

                            this.detailInfo.countAC += 1
                        }

                        // セルのスタイルを設定する
                        const acClassName = ' td_ac'
                        this.problemsInfoTable[curr_table_index][cellClass] = defaultCellClass + acClassName;
                        this.problemsInfoTable[curr_table_index][hasAC] = true

                    }

                }
            }
        
        },



        // テーブル情報初期化
        initProblemsInfoTable: function(){
            const len_problemsInfoTable = this.problemsInfoTable.length;
            for (let i = 0; i < len_problemsInfoTable; i++){

                for (let j=0; j<6; j++){
                    const problemType = 'p' + j;
                    const problemResult = problemType + '_results';
                    this.problemsInfoTable[i][problemResult] = '';
                    const problemClass = problemType + '_class';
                    this.problemsInfoTable[i][problemClass] = 'problemTableCell tableCell';
                    const problemResultTooltip = problemType + '_results_tooltip';
                    this.problemsInfoTable[i][problemResultTooltip] = '';
                    const hasResult = problemType + '_hasResult';
                    this.problemsInfoTable[i][hasResult] = false;
                    const hasAC = problemType + '_hasAC';
                    this.problemsInfoTable[i][hasAC] = false;
                }
            }

            // AC数の初期化
            this.detailInfo.countAC = 0;
            // this.detailInfo.countProblems = 0;
            this.detailInfo.totalAC = 0;
            this.detailInfo.totalWA = 0;
            this.detailInfo.totalTLE = 0;
            this.detailInfo.totalRE = 0;
            this.detailInfo.totalSolved = 0;
            this.detailInfo.totalSubmit = 0;

        },
        getDateTimeByEpochSecond: function(epoch_second){
            const dateTime = new Date(epoch_second * 1000);
            return dateTime.toLocaleDateString() + ' ' + dateTime.toLocaleTimeString('ja-JP');
        },
        makeToolTipContent: function(currentContent, addingContent){
            if(currentContent === ''){return addingContent;}
            else{return currentContent + '<br>' + addingContent;}
        },
        // 検索期間再設定
        resetDateFromTo: function(){

            // 検索期間カラ設定
            if(this.dateInfo.from.selectedDateText === ''){
                this.dateInfoDataArea.from.selectedDate = new Date(2013, 10, 12, 21, 0, 0);

            } else {
                this.dateInfoDataArea.from.selectedDate = this.dateInfo.from.selectedDate;
            }
            
            // 検索期間マデ設定
            if(this.dateInfo.to.selectedDateText === ''){
                this.dateInfoDataArea.to.selectedDate = new Date();

            } else{     
                this.dateInfoDataArea.to.selectedDate = this.dateInfo.to.selectedDate;
            }



        }
    },

    created() {
        this.initDataArea();
    },
    watch: {
        userInfo: function () {
            this.createTableData();

        },
        dateInfo: {
            handler: function(){
                this.resetDateFromTo();
                this.createTableData();
            },
            deep: true
        }
    }
}
</script>

<style>
.dataArea{
    margin-top: 90px;
}

.dataArea a:hover{
    border-bottom: solid;
    border-width: 1px;

}


.td_ac{
    background-color: #D2DDF1;
}
.td_ac .resultLink{
    color:#4b5b7c;
}
.td_ac .resultLink:visited{
    color:#4b5b7c;
}
.td_ac .resultLink:hover{
    color:#8892a7;
    border-color: #8892a7;

}

.td_ac p{
    background-color: #D2DDF1;
}
.td_ac a{
    background-color: #D2DDF1;
}
.tableCell{
    display: table-cell;
    margin: 0px auto;

}
.problemIdTableCell{
    width: 8vh;
    background-color: white;
    font-weight: bold;
}
.problemIdTableCell p {
    background-color: white;
}
.problemIdTableCell a {
    background-color: white;
    color: #7F6C68;
}
.resultLink{
    font-weight: bold;
}

.notAC .resultLink{
    color: #7c4141;
    font-weight: bold;
}
.notAC .resultLink:visited{
    color: #7c4141;
    font-weight: bold;
}
.notAC .resultLink:hover{
    color: #ac8282;
    border-color: #ac8282;

}
.notAC{
    background-color: #f1d2d2;
}
.notAC p{
    background-color: #f1d2d2;
}
.notAC a{
    background-color: #f1d2d2;
}

.resultP{
    margin:5px auto;
}


a.resultLink:visited{
    color: #7F6C68;
}
a.resultLink:hover{
    color: #958a88;
    border-bottom: solid;
    border-width: 1px;

}

.problemTableCell{
    width: 13vw;
}



@media only screen and (max-width: 550px) {
    .abc_data {
        margin: auto;
        }
    .abc_data tr {
        display:block;
        margin-bottom: 10px;
    }
    .abc_data th {
        display:block;
        width: 100%;
    }
    .abc_data td {
        display: list-item;
        list-style-type: none;
        background-image: none;
        padding-left: 0px;
        width: 90%;
        border:none;
    }
    .problemIdTableCell{
        width: 8vh;
        background-color: #AEAFAF;
        font-weight: bold;
    }
    .problemIdTableCell > p {
        background-color: #AEAFAF;
    }
    .problemIdTableCell a {
        background-color: #AEAFAF;
    }


}

</style>