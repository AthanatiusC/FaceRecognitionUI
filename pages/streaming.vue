<template>
    <div>
        <div align="center" v-if="Object.keys(streams).length">
            Camera : {{$route.query['id']}}
            <div v-bind:class="{success : streams[$route.query['id']].fps>20,}">
                FPS:{{streams[$route.query['id']].fps}}
            </div> 
            <img class="img-large" v-if="streams[$route.query['id']].frame" draggable="false" v-bind:src="'data:image/gif;base64,'+ streams[$route.query['id']].frame" alt="Stream">
        </div>
        <div class="divider"></div>
        <b>Detection list:</b>
        <div class="recognized-list">
            <transition-group name="slide-fade" >
            <div class="recognized-holder" v-for="(recog,index) in result" :key="index">
                <div>
                    <div v-if="recog['data'] != undefined && recog['camera']==$route.query['id'] ">
                        <img class="recognized-image" v-bind:src="'data:image/gif;base64,'+ recog['image']" alt="Stream">
                        <div class="title">
                            <b>{{recog["data"]["name"]}}</b>
                        </div>
                        <div class="data">
                            Validation : {{Precentify(recog["data"]["accuracy"])}}%
                            <br>Time used : {{recog['data']['elapsed']}}
                            <br>User ID : {{recog['data']['user_id']}}
                        </div>
                    </div>
                </div>
            </div>
            </transition-group>
        </div>
    </div>
</template>
<style>
img{
    max-height: 100%;
    max-width: 100%;
}
.success{
    color: green;
}
.img-large{
    margin:10px 20px 10px 20px;
    height:500px;
    width:700px;
    border-radius: 5px;
    border: solid 1px #312f40;
    box-shadow: 0px 0px 20px 0px rgba(0,0,0,0.75);
    display: block;
}
.recognized-image{
    height:170px;
    width:130px;
    border-radius: 5px;
    margin:5px 0px 5px 0px;
}
.recognized-list{
    display: block;
    padding: 5px 10px 5px 10px;
    overflow-x: auto;
    width: 90%;
    white-space: nowrap;
}

.recognized-holder{
    display: inline-block;
    background-color: #1d1b26;
    text-align: center;
    border-radius: 5px;
    box-shadow: 0px 0px 20px 0px rgba(0,0,0,0.75);
    max-height: 100%;
    margin: 10px 5px 10px 5px;
    padding:10px;
}
.slide-fade-enter-active,.slide-fade-leave-active  {
  transition: all 1s;
}
.slide-fade-enter
/* .slide-fade-leave-active below version 2.1.8 */ {
  transform: translateX(-30px);
  opacity: 0;
}
.slide-fade-leave-to{
    transform: translateX(30px);
    opacity: 0;
}
</style>
<script>
export default {
    data(){
        return{
            streams : {},
            recognized:[],
            recognizedName:[],
            test:["1","2","3","4","5"]
        }
    },mounted(){
        this.socket = this.$nuxtSocket({
            name:"main"
        })
        var cam = this.$route.query['id']

        this.socket.emit('stream',cam)
        this.socket.on('serverstream'+cam,(response)=>{
            this.streams = {}
            this.streams = {[cam]:response}
        })

        this.socket.on('result',(res)=>{
            if(!this.recognizedName.includes(res[0]["data"]["name"])){
                this.recognizedName.push(res[0]["data"]["name"])
                this.recognized.push(res[0])
                setTimeout(() => {
                    this.recognized.pop()
                }, 10000);
            }
        })
    },computed:{
        result(){
            return this.recognized.reverse()
        }
    },methods:{
        Precentify(value){
            return Number((100-value*100+10).toFixed(1));
        }
    }
}
</script>