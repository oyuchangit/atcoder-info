<template>
    <div class="serarchArea">
        <div class="inputArea">
        <input class="input" v-model="userId" ref="textBox" v-on:keyup.enter="submit" placeholder="User ID">
        </div>
    </div>
</template>

<script>
export default {
    data(){
        return{
            userId: '',
            userInfo: {}
        }
    },
    methods: {
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
        }

    }
}
</script>
<style>
.inputArea{
    width: 13em;
    /* margin-left: auto;
    margin-right: auto; */
    padding-top: 5px;

}
.serarchArea{
    position: fixed;
    width: 100%;
    top: 40px;
    background: white;
    height: 50px;
}
</style>
