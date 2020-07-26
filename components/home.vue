<template>
    <div class="fluid-container" style="margin:20px">
        <div class="stream-display" v-for="(stream,key) in streams" :key="stream">
                <div class="stream-holder" >
                        <img class="img" draggable="false" v-bind:src="'data:image/gif;base64,'+ stream" alt="Stream">
                        <h3 class="stream-name">Camera : {{key}}</h3>
                </div>
                
                <NLink :to="{ path: '/streaming', query: { id: '0' }}">
                GO
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
    }
    .stream-holder{
        display:inline-block;
        border: solid 1px #312f40;
        text-align: center;
        border-radius: 5px;
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
            online_cameras:"",
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
            this.socket.on("online_cameras",(cam_list)=>{
                for(var cam in cam_list){
                    this.socket.emit('stream',cam)
                    this.socket.on('serverstream'+cam,(response)=>{
                        this.streams = {}
                        this.streams = {[cam]:response}
                        // console.log("responded")
                    })
                }
            })
            this.online_cameras = this.streams.length
            // this.socket.emit('recognized')
            this.socket.on('result',(result)=>{
                // console.log(result)
            })
            this.socket.on('message',(result)=>{
                this.message.push(result)
                setTimeout(() => {
                    this.message.pop()
                }, 3000);
            })
        },
        test(){
            console.log('test')
        }
    }

}
</script>