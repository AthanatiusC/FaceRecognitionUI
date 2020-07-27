<template>
    <div class="fluid-container" style="margin:20px">
        <div class="stream-display" v-for="cam in online_cameras" :key="cam.index">
            <NLink :to="{ path: '/streaming', query: { id: cam }}">
                <div class="stream-holder" v-if="Object.keys(streams).length" >
                        <img class="img" draggable="false" v-bind:src="'data:image/gif;base64,'+ streams[cam].frame" alt="Stream">
                        <div class="divider"></div>
                        <div class="stream-name">
                            <b>Address : {{cam.index}}</b>
                            <b v-if="cam==0">Webcam</b>
                        </div>
                </div>
            </NLink>
        </div>
        <transition-group name="list" tag="p">
            <div class="pop-up" v-for="mess in message" v-bind:key="mess">
                <div class="message-title">
                    <b>Message</b>
                </div>
                <div class="divider"></div>
                <div class="message-body">
                    <span class="list-item">
                        {{ mess }}
                    </span>
                </div>
            </div>
        </transition-group>
    </div>
</template>
<style>
    .img{
        border-radius: 5px 1px 5px 1px;
        border-radius: 10px;
        height:300px;
        width:400px;
        margin:10px;
        border: solid 1px #312f40;
    }
    .stream-display{
        display:inline-block;
        margin: 10px;
    }
    .stream-holder{
        background-color: #1d1b26;
        text-align: center;
        border-radius: 5px;
        box-shadow: 0px 0px 20px 0px rgba(0,0,0,0.75);
    }
    .stream-name{
        display:block;
        margin: 5px;
    }
    .pop-up{
        background-color:#292736;
        padding:5px 10px 5px 10px;
        margin:20px;
        display: block;
        position: fixed;
        top:0px;
        left:50%;
    }
    .divider{
        border-bottom:solid #47445c;
        border-width: thin;
        margin: 5px 0px 5px 0px;
    }
    .message-title{
        text-align: center;
    }
    .list-enter-active, .list-leave-active {
        transition: all 1s;
    }
    .list-enter, .list-leave-to /* .list-leave-active below version 2.1.8 */ {
        opacity: 0;
        transform: translateY(30px);
    }
</style>
<script>
export default {
    data(){
        return{
            online_cameras:[],
            streams:{},
            message : []
        }
    },
    mounted(){
        this.socket = this.$nuxtSocket({
            name:"main"
        })
        this.initSocket()
    },
    methods:{
        initSocket(){
            this.socket.emit('multi-stream')
            this.socket.on('multi-stream-recieve',(response)=>{
                this.streams =response
                // console.log(this.streams)
                // console.log()
            })
            this.socket.on("online_cameras",(cam_list)=>{
                for(var cam in cam_list){
                    this.online_cameras.splice(0,0,Number(cam))
                }
            })
            console.log(this.streams)
            this.socket.on('message',(result)=>{
                this.message.push(result)
                setTimeout(() => {
                    this.message.pop()
                }, 3000);
            })
        },isUndefined(item){
            if(typeof(item) == "undefined"){
                return true
            }else{
                false
            }
        }
    },beforeDestroy(){
        this.socket.emit("multi-stream-stop")
        console.log("closed")
    }

}
</script>