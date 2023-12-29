<template>
    <div class="nav">
      <div class="nav_li">
        <ul class="daohang3">
          <li
            v-for="(item, index) in daohang"
            :key="index"
          >
            <span>
              <!-- :class="c" -->
              <router-link
                :to="{ path: item.path, query: { name: item.name } }" class="classA" :class="{ 'curClass':isActive(item.path) }"
                >{{ item.name }}</router-link
              >
            </span>
          </li>
        </ul>
      </div>
    </div>
</template>

<script>
export default {
  data() {
    return {
      //居中导航
      daohang: [],
      daohang1:[{  // 7.4 测试数据
          name: '首页',
          path: '/shouye',
        },
        {
          name: '发现',
          path: '/gitDiscoverca',
        },{
          name: '处置',
          path: '/disposePage',
        },{
          name: '警情',
          path:'/getWarningfourg'
        },{
          name: '特办',
          path: '/specialPage'
        },{
          name: '关联',
          path: '/relevancePage'
        },{
          name: '流转',
          path: '/lzPage',
        },
        {
          name: '管理',
          path: '/dept',
        }],
        daohang2: [{  // 7.4 测试数据
          name: '首页',
          path: '/shouye',
        },
        {
          name: '发现',
          path: '/gitDiscoverca',
        },{
          name: '处置',
          path: '/disposePage',
        },{
          name: '警情',
          path:'/getWarningfourg'
        },{
          name: '特办',
          path: '/specialPage'
        },{
          name: '关联',
          path: '/relevancePage'
        },{
          name: '流转',
          path: '/lzPage',
        }],
      namenav: '',
      navnumlist: [],
    }
  },
  // created() {
  //   this.daohangqx();
  // //  console.log( this.$store.state.navlist);

  // },
  mounted() {
    // this.daohang=   JSON.parse(window.localStorage.getItem("nav"))
  },
  created() {  // 7.4 测试暂时关闭接口
    this.dh()
    // this.daohangzhiding()
  },
  methods: {
    isActive(path){
      // console.log(path +'?=='+this.$router.currentRoute.path)
      if(this.$router.currentRoute.path == '/findUser'&&path =='/dept'||this.$router.currentRoute.path == '/findRole'&&path =='/dept'){
        // console.log('true');
        return true
      }else{
        return path == this.$router.currentRoute.path
      }
      
    },
    dh() {
      let curRole = window.sessionStorage.getItem('role')

      // let cj = 'CD_SUPER_ADMIN'

      if (curRole == 'OPERATOR') {
        this.daohang = this.daohang2
      }else{
        this.daohang = this.daohang1
      }
    },
    //导航
    // async daohangqx() {
    //   const that = this
    //   const { data: res } = await this.$http.get('/menu/queryUserNavList')
    //   //  console.log(res.data);
    //   if (res.code == 200) {
    //     res.data = res.data.map((item) => {
    //       return {
    //         name: item.menuName,
    //         path: item.menuUrl,
    //       }
    //     })

    //     // 排序
    //     // for (var i = 0; i < res.data.length; i++) {
    //     //   if (res.data[i].name == this.namenav) {

    //     //     let num = res.data.splice(i - 1, res.data.length).reverse()

    //     //     for (var j = 0; j < num.length; j++) {
    //     //       res.data.unshift(num[j])
    //     //     }
    //     //   }
    //     // }

    //     this.daohang = res.data
    //     console.log(this.daohang)
    //   }

    //   // this.items = res.data;
    // },
    // async daohangzhiding() {  // 7.4 测试暂时关闭接口
    //   const { data: res } = await this.$http.post('/menu/queryUserMenuList')
    //   // console.log('daohangzhiding');
    //   if (res.code == 200) {
    //     let navres = this.$route.path.slice(1, this.$route.path.length)

    //     for (var i = 0; i < res.data.length; i++) {
    //       if (res.data[i].children != null) {
    //         for (var j = 0; j < res.data[i].children.length; j++) {
    //           if (navres == res.data[i].children[j].name) {
    //             this.namenav = res.data[i].menuName
    //           }
    //         }
    //       } else {
    //         if (navres == res.data[i].name) {
    //           this.namenav = res.data[i].menuName
    //         }
    //       }
    //     }
    //   }
    //   // this.daohangqx()
    // },
    // 点击上下方法
    remove(array, index) {
      if (index <= array.length - 1) {
        for (var i = index; i < array.length; i++) {
          array[i] = array[i + 1]
        }
      }
      array.length = array.length - 1

      return array
    },
    // 模块————导航上
    up() {
      if (this.daohang.length == 1) {
        this.$message('当前您的权限只有一项')
      } else {
        var arr = this.daohang.slice(0, 1)
        this.daohang = this.remove(this.daohang, 0)
        this.daohang = [...this.daohang, ...arr]
      }

      // console.log(this.daohang);
    },
    // 模块————导航下
    low() {
      if (this.daohang.length == 1) {
        this.$message('当前您的权限只有一项')
      } else {
        var arr = this.daohang.slice(
          this.daohang.length - 1,
          this.daohang.length
        )
        this.daohang = this.remove(this.daohang, this.daohang.length)
        // console.log( this.daohang);
        this.daohang = [...arr, ...this.daohang]
      }
    },
  },
}
</script>

<style scoped lang='less'>
.nav {
  width: 1027px;
  height: 3.2%;
  margin: 0 auto;
  position: relative;
}
.nav_li {
  height: 100%;
  box-sizing: border-box;
  float: left;
  position: relative;
  left: 50%;

}
.nav_li .daohang3 li {
  font-family: '黑体';
  float: left;
  list-style-type: none;
  text-align: center;
  height: 40px;
  line-height: 40px;
  width: 100px;
  margin-right: 8px;
  background: url(../assets/img/newbg/btnbg.png);
  background-size: cover;
  color:#00fff3;
  // position: relative; 
}
.nav_li .daohang3 li span{
  color:#00fff3
}

.nav_li .daohang3 li:hover {
  color: #fff;
  border-bottom-color: #07e7e6;
}
.curClass{
  color: rgba(226, 241, 255, 0.85) !important;
  // color:  hsl(173, 100%, 80%) !important;
  font-size: 22px !important
}
span .classA{
  color:rgba(0, 255, 255, 0.75); // rgba(0, 255, 120)
  // color: hsl(173, 30%, 95%);
  font-size: 20px 
}
span .classA:hover,
span .classA:active {
  color: rgba(226, 241, 255, 0.85);
  font-size: 22px !important
  // color:  hsl(173, 100%, 80%);
}
.dianl {
  position: absolute;
  width: 50px /* 50/16 */;
  height: 32px;
  cursor: pointer;
  left: 30px;
  top: 0;
}
.dianl img {
  width: 100% /* 50/16 */;
  height: 100%;
}
.dianr {
  position: absolute;
  width: 50px /* 50/16 */;
  height: 32px;
  cursor: pointer;
  right: 30px;
  top: 0;
}
.dianr img {
  width: 100% /* 50/16 */;
  height: 100%;
}
.menu_xuanzhong {
  position: absolute;
  width: 20px /* 50/16 */;
  height: 20px;
  bottom: 0;
  left: 47%;
}
.menu_xuanzhong img {
  width: 100%;
  height: 100%;
}

.three {
  font-size: 22px /* 20/16 */ /* 18/16 */;
  color: #dfe0f7;
  font-weight: 500;
}
.three:hover {
  color: #fff;
  text-shadow: 0 0 20px rgba(0, 121, 255, 0.5);
}

.threes {
  font-size: 20px /* 16/16 */;
  color: #fff;
  font-weight: 500;
}
.four {
  font-size: 20px /* 16/16 */;
  font-weight: 500;
  background-image: linear-gradient(to left, #d6e1e5 0%, #51626d 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  // pointer-events: none;
}
// .five {
//   font-size: 20px /* 16/16 */;
//   font-weight: 500;
//   background-image: linear-gradient(to right, #d6e1e5 0%, #51626d 100%);
//   -webkit-background-clip: text;
//   -webkit-text-fill-color: transparent;
//   // pointer-events: none;
// }
.five {
  font-size: 20px /* 16/16 */;
  font-weight: 500;
  color: #fff;
  border-bottom-color: #07e7e6 !important;
  // pointer-events: none;
}
.six {
  font-size: 20px /* 16/16 */;
  font-weight: 500;
  color: #b5b5d4;
  // text-shadow: 0 0 20px #0079ff;
  // background-image: linear-gradient(to right, #d6e1e5 0%, #51626d 100%);
  // -webkit-background-clip: text;
  // -webkit-text-fill-color: transparent;
  // pointer-events: none;
}
.six:hover {
  color: #fff;
  border-bottom-color: #07e7e6 !important;
}
.daohang3 {
  position: relative;
  left: -50%;
  // left: 11%;
}
.jinzhi1 {
  font-size: 20px /* 16/16 */;
  font-weight: 500;
  color: #b5b5d4;
  cursor: not-allowed !important;
}
</style>