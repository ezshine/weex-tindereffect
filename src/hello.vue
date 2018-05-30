<!--  竞选首页  -->
<template>
    <div class="wrapper">
		<div class="slider">
			<div class="sliderlist" ref="card3" style="position:absolute;" :style="card3TransformStyle">
				<div class="Player_image" >
				
				</div>
			</div>
			<div class="sliderlist" ref="card2" style="position:absolute;" :style="card2TransformStyle">
				<div class="Player_image" >
				
				</div>
			</div>
			<div class="sliderlist" ref="card" style="position:absolute;"  :style="card1TransformStyle" @click="clickCard(item)" @panstart="onpanstart" @panmove="onpanmove" @panend="onpanend">
				<div class="Player_image" >

				</div>
			</div>
		</div>
    </div>
</template>

<script>

    export default {
        data () {
            return {
                item:{},
                item2:{},
                item3:{},
                index: 0,
                
                ElectionList: [],
                cardOriginX:0,
                cardOriginY:0,
                moveX:0,
                moveY:0,
                touchDownX:0,
                touchDownY:0,
                touchDownTime:0,
                card1TransformStyle:{
                    transform:"translate(0,0)",
                    transformOrigin: 'center center'
                },
                card2TransformStyle:{
                    transform:"scale(.9,.9) translate(0,60)",
                    transformOrigin: 'center center'
                },
                card3TransformStyle:{
                    transform:"scale(.8,.8) translate(0,120)",
                    transformOrigin: 'center center'
                },
                inAnimation:false,
				screenHeight:0
            }
        },
        mounted () {
            var that=this;
            that.screenHeight=weex.config.eros.deviceHeight/(weex.config.eros.scale/2);
            this.cardOriginX=this.moveX=this.moveY=0;
            this.cardOriginY=(that.screenHeight-1334)/2;
        },
        methods: {
			clickCard:function(item){

			},
            onpanstart:function(e) {
                if(this.inAnimation)return;
         		// console.log(e);
                 this.touchDownX=e.changedTouches[0].pageX;
                 this.touchDownY=e.changedTouches[0].pageY;
                 this.touchDownTime=new Date().getTime();
			},
			onpanmove:function(e){
                if(this.inAnimation||this.touchDownTime<=0)return;
				// this.moveX=JSON.stringify(e);
				this.moveX=e.changedTouches[0].pageX-this.touchDownX;
				this.moveY=e.changedTouches[0].pageY-this.touchDownY;

                this.card1TransformStyle={transform:"translate("+this.moveX+","+this.moveY+")"};
                
                var distance=this.getDistance(e.changedTouches[0].pageX,e.changedTouches[0].pageY,this.touchDownX,this.touchDownY);
                // this.search_text=distance;
                var scale=0.9+distance/10000;
                if(scale>1){
                    scale=1;
                }
                // this.card2TransformStyle={transform:"scale("+scale+","+scale+")",top:120*(1-distance/1000)};
			},
			onpanend:function(e){
                if(this.inAnimation||this.touchDownTime<=0)return;

                this.moveX=e.changedTouches[0].pageX-this.startX;
				this.moveY=e.changedTouches[0].pageY-this.startY;

                // common.showToast(this.moveX);
                var distance=this.getDistance(e.changedTouches[0].pageX,e.changedTouches[0].pageY,this.touchDownX,this.touchDownY);

                var velocity=distance/(new Date().getTime()-this.touchDownTime);
                this.touchDownTime=0;
                // this.search_text=velocity;

                if(velocity<=1.5&&distance<320)this.backCardToOrigin();
                else{
                    var angle=Math.atan2(e.changedTouches[0].pageY-this.touchDownY, e.changedTouches[0].pageX-this.touchDownX)/(3.14159/180);
	                var direction={x:Math.cos(angle * (3.14159/180)),y:Math.sin(angle * (3.14159/180))};

                    this.throwCard(direction);
                }
			},
            getDistance:function(x1, y1, x2, y2) {
                var _x = Math.abs(x1 - x2);       
                var _y = Math.abs(y1 - y2);       
                return Math.sqrt(_x * _x + _y * _y);   
            },
            throwCard:function(direction){
                var that=this;
                var card = this.$refs.card;
                var  animation = weex.requireModule('animation');
                this.inAnimation=true;
                animation.transition(card, {
                    styles: {
                        transform: 'translate('+(direction.x*2000)+', '+(direction.y*1500)+')',
                        transformOrigin: 'center center',
                        opacity:0,
                    },
                    duration: 300, //ms
                    timingFunction: 'ease',
                    needLayout:false,
                    delay: 0 //ms
                }, 
                function () {
                    
                });

                var card2 = this.$refs.card2;
                var animation2 = weex.requireModule('animation');
                animation2.transition(card2, {
                    styles: {
                        transform: 'scale(1,1) translate(0,0)',
                        transformOrigin: 'center center'
                    },
                    duration: 150, //ms
                    timingFunction: 'ease',
                    needLayout:false,
                    delay: 0 //ms
                    }, function () {
                    });

                var card3 = this.$refs.card3;
                var animation3 = weex.requireModule('animation');
                animation3.transition(card3, {
                    styles: {
                        transform: 'scale(.9,.9) translate(0,60)',
                        transformOrigin: 'center center'
                    },
                    duration: 300, //ms
                    timingFunction: 'ease',
                    needLayout:false,
                    delay: 0 //ms
                    }, function () {
                        that.inAnimation=false;
                        that.resetAllPostion();
                    });
            },
            resetAllPostion:function(){
                var that=this;
                // this.$router.refresh();
                var item=JSON.parse(JSON.stringify(this.item));
                //将卡片的内容对调
				this.item=JSON.parse(JSON.stringify(this.item2));
                
                that.card1TransformStyle={
                        transform:"translate(0,0)",
                        transformOrigin: 'center center',
                        opacity:1
                    }

                    that.inAnimation=true;

                    setTimeout(do23Animation,100);

                function do23Animation(){
                    that.cardOriginX=0;
                that.moveX=0;
                that.cardOriginY=(that.screenHeight-1334)/2;
                that.moveY=0;

                that.touchDownX=0;
                that.touchDownY=0;

				//更新下一张卡片的内容
                //that.item2=item;

                        var card = that.$refs.card;
                var  animation = weex.requireModule('animation');
                that.inAnimation=true;
                animation.transition(card, {
                    styles: {
                        transform:"translate(0,0)",
                        transformOrigin: 'center center',
                        opacity:1
                    },
                    duration: 300, //ms
                    timingFunction: 'ease',
                    needLayout:false,
                    // delay: 0 //ms
                    }, function () {
                        that.inAnimation=false;
                    });

                    var card2 = that.$refs.card2;
                var  animation2 = weex.requireModule('animation');

                animation2.transition(card2, {
                    styles: {
                        transform:"scale(.9,.9) translate(0,60)",
                        transformOrigin: 'center center',
                    },
                    duration: 150, //ms
                    timingFunction: 'ease',
                    needLayout:false,
                    delay: 0 //ms
                    }, function () {
                    });

                    var card3 = that.$refs.card3;
                var  animation3 = weex.requireModule('animation');
                animation3.transition(card3, {
                    styles: {
                        transform:"scale(.8,.8) translate(0,120)",
                        transformOrigin: 'center center',
                    },
                    duration: 300, //ms
                    timingFunction: 'ease',
                    needLayout:false,
                    delay: 0 //ms
                    }, function () {
                        
                    });
                }
            },
            backCardToOrigin:function(){
                var that=this;
                var card = this.$refs.card;
                var  animation = weex.requireModule('animation');
                this.inAnimation=true;
                animation.transition(card, {
                    styles: {
                        transform: 'translate(0,0)',
                        transformOrigin: 'center center'
                    },
                    duration: 300, //ms
                    timingFunction: 'ease',
                    needLayout:false,
                    // delay: 0 //ms
                    }, function () {
                        that.inAnimation=false;
                    });
            }
        }
    }
</script>
<style scoped>
    .slider{
        width: 750px;
		height:1334px;
		background-color: #f2f2f2;
    }
    .sliderlist{
        width: 650px;
		height:750px;
		margin-left: 50px;
		margin-top: 200px;
		background-color:#fff;
		box-shadow: 0 0 30px 0 rgba(36,54,79,0.20);
        /*position: relative;*/
    }
    
    .Player_image{
        width: 580px;
        margin-top: 70px;
        margin-left: 80px;
        margin-right: 80px;
        box-shadow: 0 0 30px 0 rgba(36,54,79,0.20);
        align-items: center;
        background-color: white;
        /*height: 770px;*/
        position: absolute;
        top: 0;
        transition: 1s all;
    }
    .Player_image_one{
        width: 440px;
        height: 440px;
        margin-top: 80px;
    }
    .Player_image_two{
        width: 60px;
        height: 60px;
        /*margin-top: 102px;*/
        /*margin-top: 162px;*/
    }
    .Player{
        width: 580px;
        margin-top: 70px;
        margin-left: 80px;
        margin-right: 80px;
        margin-bottom: 50px;
        box-shadow: 0 0 30px 0 rgba(36,54,79,0.20);
        transition: 1s all;
    }
    .Player_top{
        height: 314px;
        background-color: #24364f;
    }
    .Player_top_div_one{
        margin-top: 40px;
        margin-left: 40px;
        margin-right: 40px;
        margin-bottom: 30px;
        flex-direction: row;
    }
    .Player_top_div_avatar{
        width: 120px;
        height: 120px;
        border-radius: 100px;
        overflow: hidden;
        border-style: solid;
        border-color: white;
        border-width: 4px;
        box-sizing: border-box;
    }
    .Player_top_div_two{
        margin-left: 30px;
    }
    .Player_top_div_text_one{
        font-size: 34px;
        color: #FFFFFF;
        font-weight: bold;
        margin-top: 10px;
    }
    .Player_top_div_text_two{
        font-size: 28px;
        color: #FFFFFF;
        margin-top: 20px;
    }
    .Player_top_div_one_text{
        padding-top:5px;
        padding-bottom:5px;
        font-size: 30px;
        color: #FFFFFF;
        margin-left:40px;
        margin-right: 40px;
    }
    .Player_bottom_div{
        height:80px;
        flex-direction: row;
        align-items: center;
        padding-left:40px;
        padding-right:40px;
        justify-content: space-between;
        background-color: white;
    }
    .Player_bottom
    {
pointer-events: none;
background-color:#ffffff;
    }
    .Player_bottom_div_bottom{
        flex-direction: row;
        padding-left:40px;
        padding-right:40px;
        padding-top: 24px;
        padding-bottom:40px;
        height: 100px;
        justify-content: space-between;
        background-color: white;
    }
    .active{
        background-color: #F6F6F6;
    }
    .Player_bottom_div_text{
        font-size: 24px;
        color: #24364F;
        text-align: right;
    }
    .Player_bottom_div_text2
    {
        font-size: 24px;
        color: #24364F;
        text-align: right;
        height:100px;
    }
    .Player_bottom_div_bottom_div{
        width: 360px;
    }
    .nolist{
        width: 750px;
        flex-direction: column;
        align-items: center;
    }
    .nolist_image{
        width: 400px;
        height: 400px;
        margin-top: 60px;
    }
    .nolist_text{
        font-size: 34px;
        color: #24364F;
    }
</style>
