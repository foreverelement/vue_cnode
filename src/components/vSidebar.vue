<template>
	<div class="offcanvas" @click="hide">

    <nav class="offcanvas-bar" @click.stop>
      <div class="user">
        <div v-if="isLogin" @click="goUser">
          <img :src="avatar_url" alt="">
          <p>Jchehe</p>
        </div>
        <a v-else @click="goEnter" class="login"><i class="iconfont">&#xe604;</i>登录</a>


      </div>
      <ul v-for="(index, items) in itemSet">
      	<li v-for="item in items" v-link="{path: '/'+item.view, query:{'tab': item.tab}}" :class="{'active': item.tab == tab, 'tab': item.tab, 'unread': (item.view === 'message' && unreadCount > 0)}" @click="change(item.tab)">
      		<a href="javascript:;">
      			<i class="iconfont">{{{item.icon}}}</i>
	      		{{item.name}}

            <span class="unreadTip" v-if="item.view==='message' && unreadCount !== 0">( {{unreadCount}} )</span>
      		</a>
      	</li>
        <li v-if="isLogin && index==itemSet.length -1"><a @click="logout"><i class="iconfont">&#xe60d;</i> 注销</a></li>
        
      </ul>
      <div class="logo">
        <img src="//o4j806krb.qnssl.com/public/images/cnodejs_light.svg" alt="">
      </div>
    </nav>
  </div>
</template>

<script>
export default{
	data(){
		return {
      tab: this.$route.query.tab,
			itemSet:[
				[{
					icon: "&#xe602;",
					name: "全部",
          view: "list",
          tab: "all"
				},{
					icon: "&#xe605;",
					name: "精华",
          view: "list",
          tab: "good"
				},{
					icon: "&#xe600;",
					name:"分享",
          view: "list",
          tab: "share"
				},{
					icon: "&#xe608;",
					name: "问答",
          view: "list",
          tab: "ask"
				},{
					icon: "&#xe606;",
					name: "招聘",
          view: "list",
          tab: "job"
				}],[{
					icon: "&#xe607;",
					name: "消息",
          view: "message"
				},{
					icon: "&#xe608;",
					name: "关于",
          view: "about"
				}]/*,[{
          icon: "&#xe608;",
          name: "个人信息",
          view: "perinfo"
        },{
          icon: "&#xe608;",
          name: "发帖",
          view: "newtopic"
        }]*/
			]
		}
	},
 
	props:{
		isShowSidebar:{
			type: Boolean,
			required: true,
      twoWay: true
		},
    isShowConfirm:{
      type: Boolean,
      required: true,
      twoWay: true
    },
    isLogin: {
      type: Boolean,
      required: true,
      twoWay: true
    },
    avatar_url: {
      type: String,
      required: true,
      twoWay: true
    },
    loginname: {
      type: String,
      required: true,
      twoWay: true
    },
    title: {
      type: String,
      required: true,
      twoWay: true
    },
    unreadCount: {
      type: Number,
      required: true,
      twoWay: true,
      default: 0
    }
	},
	methods:{
		hide(){
			this.isShowSidebar = false
		},
    change(tab){
      if(tab == undefined && event.currentTarget.dataset.view=="logout"){
         this.isShowConfirm = true        
      }
      this.tab = tab
    },
    goEnter(){
      var link = ""
      var redirect = this.$route.query.redirect
      if(this.$route.path!=="login" && !redirect){
        link = "/login?redirect=" + encodeURIComponent(this.$route.path)
        this.$route.router.go(link)
      }else{
        this.isShowSidebar = false
      }
    },
    goUser(){
      this.$route.router.go({path: "/perinfo/"+this.loginname})
      this.hide()
    },
    logout(event){
      this.isShowConfirm = true
    }
	}
}
</script>

<style scoped>
	.offcanvas {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    visibility: hidden;
    z-index: 1001;
  }
  .user{
    text-align: center;
    padding: 60px 0 25px;
  }

  .user img, .user p{
    cursor: pointer;

  }
  .user img{
    width: 64px;
    height: 64px;
    border-radius: 50%;
    box-shadow: 0 0 1px 5px #3d4a5d;

  }
  
  .user img+p{
    color: #fff;
    line-height: 32px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    max-width: 80%;
    margin: 5px auto;
  }
  .offcanvas .logo {
    text-align: center;
    padding: 14px 0;
    position: absolute;
    bottom: 0;
  }

  .offcanvas .logo img {
    width: 60%;
  }
  @media all and (max-width: 435px){
    .user{
      padding: 20px 0 0;
    }
    .user img+p{
      margin: 0 auto;
    }
    .offcanvas .logo{
      padding: 5px 0;
    }
    .offcanvas .logo img {
      width: 45%;
    }
  }
  .offcanvas-bar {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    width: 235px;
    background-color: #24344B;
    max-width: 100%;
    -webkit-transform: translateX(-100%);
    -ms-transform: translateX(-100%);
    -o-transform: translateX(-100%);
    transform: translateX(-100%);
    -webkit-transition: all .3s;
    -o-transition: all .3s;
    transition: all .3s;
  }
  
  .offcanvas-bar ul {
    margin-top: 14px;
    background-color: #2b3c58;
  }
  
  .offcanvas-bar ul li {
    line-height: 1.8;
    padding: 4px 24px;
  }
  .offcanvas-bar ul li a{
  	display: block;
  }
  .offcanvas .iconfont {
    margin-right: 8px;
  }
  
  .offcanvas a {
    color: #7f8c8d;
    text-decoration: none;
  }
  

  
  .v-link-active.tab.active, .v-link-active:not(.tab),.offcanvas-bar .tab.active{
    background-color: #3B4F6A;
  }
  .user .login{
    font-size: 16px;
  }

  .message .unreadTip{

  }

  li.unread .iconfont{
    color: #b1b15c;
    display: inline-block;
    transform-origin: 50% 0;
    animation: shake 2.5s linear infinite;
  }

  @keyframes shake{
    0%,100%{
      transform: rotateZ(0deg);
    }
    20%{
      transform: rotateZ(15deg);
    }
    80%{
      transform: rotateZ(-15deg);
    }
  }

</style>
