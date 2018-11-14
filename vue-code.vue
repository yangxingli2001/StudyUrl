https://my.oschina.net/woter/blog/835753?hmsr=toutiao.io

1、 Vue 组件代码如下：

Vue.component('timerBtn',{
    template: '<button v-on:click="run" :disabled="disabled || time > 0">{{ text }}</button>',
    props: {
        second: {
            type: Number,
            default: 60
        },
        disabled: {
            type: Boolean,
            default: false
        }
    },
    data:function () {
    	return {
	        time: 0
    	}
    },
    methods: {
        run: function () {
        	this.$emit('run');
        },
        start: function(){
        	this.time = this.second;
        	this.timer();
        },
        stop: function(){
        	this.time = 0;
        	this.disabled = false;
        },
        setDisabled: function(val){
        	this.disabled = val;
        },
        timer: function () {
            if (this.time > 0) {
                this.time--;
                setTimeout(this.timer, 1000);
            }else{
            	this.disabled = false;
            }
        }
        
    },
    computed: {
        text: function () {
            return this.time > 0 ? this.time + 's 后重获取' : '获取验证码';
        }
    }
});
2、使用方式：

<timer-btn ref="timerbtn" class="btn btn-default" v-on:run="sendCode" 
:disabled="disabled" :second="60"></timer-btn>
disabled 建议不要绑定，我们可以通过调用组件的setDisabled方法来切换按钮可用状态；

second 初始值60s 没特别值可以不绑定；

所以我们可以在HTML页面这样：

<timer-btn ref="timerbtn" class="btn btn-default" v-on:run="sendCode" ></timer-btn>
JS这样： 

var vm = new Vue({
    el:'#app',
    methods:{
        sendCode:function(){
            vm.$refs.timerbtn.setDisabled(true); //设置按钮不可用
            hz.ajaxRequest("sys/sendCode?_"+$.now(),function(data){
                if(data.status){
                    vm.$refs.timerbtn.start(); //启动倒计时
                }else{
                    vm.$refs.timerbtn.stop(); //停止倒计时
                }
            });
        },
    }
});
 

OK，到这就全搞定了；
