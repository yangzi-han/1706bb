<template>
    <div class="timeline">
        <div class="timeline-top">
            <!-- <router-link to="/post" tag="span"> -->
                <img src="../../static/imgs/1.jpg" alt="" class="send">
            <!-- </router-link> -->
            <input type="text" placeholder="试试搜索你的好友名字" class="inp">
        </div>
        <div class="center">
            <transition-group name="scale">
            <li class="cont" v-for="item in list" :key="item.dynamicid">
                <div class="Ttop">
                    <img src="../../static/imgs/1.jpg" alt="" class="img">
                    
                    <div class="name">
                        <p>{{item.userName}}</p>
                        <p>2019-06-01</p>
                    </div>
                </div>
                <div class="Tmain">
                    {{item.dynamicContent}}
                </div>
                <div class="Tfooter">
                    <P class="action">
                    <span>点赞</span>
                    <span @click="reply(item)">{{`评论${item.comments.length}`}}</span>
                    </P>
                    
                </div>
                <Reply :reply="item.comments"/>
            </li>
            </transition-group>
        </div>
        <reply-modal />
    </div>
</template>
<script>

import {mapState,mapActions,mapMutations} from 'vuex';
import Reply from '@/components/reply.vue'
import ReplyModal from '@/components/replyModal.vue'
export default {
    components:{
         Reply,
         ReplyModal
    },
    computed:{
        ...mapState({
            list: state=>state.timeline.list
        }),
        
    },

    methods:{
        
        ...mapActions({
            getTimeline:"timeline/getTimeline"
        }),
        ...mapMutations({
            showModal: "replyModal/showModal"
        }),
        reply(value){
            this.replyInfo={
                type:"comment",
                dynamicid:value.dynamicid,
                content:"",
                title:`评论:${value.userName}`,
                
            }
            this.showModal({
                info: this.replyInfo,
                show: true
            })
        }
    },
    created(){
        console.log(this.list)
        this.getTimeline()
    }
}
</script>
<style lang="scss" scoped>
.timeline{
    display: flex;
    height: 100%;
    flex-direction: column;
}
.timeline-top{
    font-size: 0;
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin: .15rem .3rem;
    img{
        width: .7rem;
    }
    input{
        flex: 1;
        margin-left: .2rem;
        border-radius: .15rem;
        padding: 0 .2rem;
        line-height: .6rem;
        border: none;
        background: #eee;  
        color:#000; 
    }
}
.center{
    flex: 1;
    overflow: scroll;
    padding: .2rem .3rem;
    &>li{
        margin-bottom: .5rem;
    }
}
.timeline-center-top-top{
    font-size: .4rem;
}
.cont{
    width: 100%;
    font-size: .28rem;
    border-bottom: 1px solid #666;
    margin-top: .2rem;
}
.Ttop{
    width: 100%;
    height: 1rem;
//    background: #ccc
}
.img{
    width: 40px;
    height: 40px;
    float: left;
    margin-right: .2rem;
    border-radius: 50%;
    // background: gold;
}
.Tfooter{
    width: 100%;
    // height: .5rem;
    font-size: .28rem;
    // line-height: .5rem;
    .action{
        display: flex;
        align-items: center;
        justify-content: space-between;
        margin: .2rem 0;
    }
}
.image{
    margin: .2rem;
}
li:last-child{
    border: none;
}
.inp{
    color:#000;
}
</style>