<template>
    <div class="fluid-container" style="margin:20px">
        <img class="img" v-bind:src="'data:image/gif;base64,'+ imgbase64" alt="Stream">
        <div class="rec">
            <div class="result">

            </div>
        </div>
    </div>
</template>
<script>
export default {
    data :{
        stream:Array,
        online_cameras:Array
    },
    mounted(){
        this.socket = this.$nuxtSocket({
            name:"main"
        })
        this.initSocket()
    },
    methods:{
        initSocket(){
            console.log(this.getOnlineCam())
            for(cam in this.online_cameras){
                this.socket.emit('stream',cam)
                this.socket.on('serverstream'+cam,(response)=>{
                    this.stream[cam] = response
                })
            }
            this.socket.emit('recognized')
            this.socket.on('result',(result)=>{
                // console.log(result)
            })
            this.socket.on('message',(result)=>{
                console.log(result)
            })
        },
        getOnlineCam(){
            var a
            this.socket.on("online_cameras",(resp)=>{
                a = resp
            })
            setInterval()
        }
    }

}
</script>
<style>
    .img{
        border-radius: 10px;
        height:300px;
        width:400px;
        margin:0px 10px 0px 10px;
        border: solid 1px #312f40;
        padding:3px;
    }
</style>