<template>
    <div class="serarchArea">
        <div class="inputArea">
            <!-- ユーザー検索エリア -->
            <div class="searchIdArea">
                <input class="input" v-model="userId" ref="textBox" v-on:keyup.enter="submit" placeholder="User ID">
            </div>
            <!-- 期間絞り込みエリア -->
            <div class="dateArea">
                <datepicker :format="datepicker.format" v-model="datepicker.from.selectedDate" :typeable="true" placeholder="YYYY-MM-DD"></datepicker>
            </div>
            ~ 
            <div class="dateArea">
                <datepicker :format="datepicker.format" v-model="datepicker.to.selectedDate" :typeable="true" placeholder="YYYY-MM-DD"></datepicker>
            </div>
        </div>
    </div>
</template>

<script>
import Datepicker from 'vuejs-datepicker';

export default {
    components: {
        Datepicker
    },

    data(){
        return{
            userId: '',
            userInfo: {},
            datepicker: {
                format: 'yyyy-MM-dd',
                from: {
                    selectedDate: '',
                    selectedDateText: ''
                    },
                to: {
                    selectedDate: new Date(),
                    selectedDateText: ''
                    }
                
            }         
        }
    },
    methods: {

        // userIdで検索
        submit(){

            const url = 'https://kenkoooo.com/atcoder/atcoder-api/results?user=' + this.userId

            // APIからデータ取得
            fetch(url)
                .then((data) => data.json())
                .then(obj => {

                    // 取得データが空では無い時、親コンポーネントに値を渡す
                    if (obj.length !== 0){

                        this.userInfo = obj
                        this.$emit("getUserInfo", this.userInfo)

                        // テキストボックスのフォーカスを外す
                        this.$refs.textBox.blur()

                    }
                })
            // const textbox = this.$refs.textBox.blur()
        },

        initDatePicker(){
            let newDate = new Date()
            this.datepicker.to.selectedDateText = this.getDateText(newDate);
        },

        // 日付を返却する
        getDateText(date){
            return date.getFullYear() + '-' + (date.getMonth()+1) + '-' + date.getDate();
        }

    },
    created() {
        this.initDatePicker();
    },
    watch: {

        // 日付変更時アクション
        datepicker: {
            handler: function(){
                let fromDate = this.datepicker.from.selectedDate
                let toDate = this.datepicker.to.selectedDate

                // 取得日時FromがNullの場合は空を設定
                if(fromDate === null){
                    this.datepicker.from.selectedDateText = '';
                    this.datepicker.from.selectedDate = '';
                // 取得日時FromがNullではない場合は、テキストの日時を設定
                } else if(fromDate !== ''){
                    this.datepicker.from.selectedDateText = this.getDateText(fromDate);
                }

                // 取得日時ToがNullの場合は空を設定
                if(toDate === null){
                    this.datepicker.to.selectedDateText = '';
                    this.datepicker.to.selectedDate = '';
                // 取得日時FromがNullではない場合は、テキストの日時を設定
                } else if(toDate !== ''){
                    this.datepicker.to.selectedDateText = this.getDateText(toDate);
                }

                
                this.$emit("getDateInfo", this.datepicker)
            },
            deep: true
        }
    }
}
</script>
<style>
.serarchArea{
    width: 100%;
    position: fixed;
    top: 40px;
    background: white;
    height: 50px;
    text-align: left;
}
.inputArea{
    padding-top: 5px;
    width: 100%;
}
.searchIdArea{
    display: inline-block;
    width: 13em;
    padding-right: 5px;
}
.dateArea{
    display: inline-block;
    padding-right: 5px;
    padding-left: 5px;
}

</style>
