<template>
  <div>
    <mu-back-top :height="50" :bottom="100" :right="15" style="z-index: 5">
      <mu-float-button icon="flight_takeoff"/>
    </mu-back-top>
    <mu-drawer :open="drawer_open" :docked="docked" @close="toggle()" :width="200"
               style="background-color:rgba(61,73,85,0.9)">

      <mu-list @itemClick="docked ? '' : toggle()" style="margin-top: 100px" :value="activeTab"
               @change="handleTabChange">
        <mu-list-item value="colligate" title="综合" titleClass="myListItem">
          <mu-icon slot="left" value="inbox" color="#f7f7f7"/>
          <!--<mu-icon slot="right" value="keyboard_return" color="#ea5545"/>-->
        </mu-list-item>
        <mu-list-item value="seekrent" title="求租" titleClass="myListItem">
          <mu-icon slot="left" value="grade" color="#f7f7f7"/>
          <!--<mu-icon slot="right" value="keyboard_return" color="#ea5545"/>-->
        </mu-list-item>
        <mu-list-item value="Arent" title="出租" titleClass="myListItem">
          <mu-icon slot="left" value="send" color="#f7f7f7"/>
          <!--<mu-icon slot="right" value="keyboard_return" color="#ea5545"/>-->
        </mu-list-item>
        <mu-list-item value="roommate" title="室友" titleClass="myListItem">
          <mu-icon slot="left" value="drafts" color="#f7f7f7"/>
          <!--<mu-icon slot="right" value="keyboard_return" color="#ea5545"/>-->
        </mu-list-item>
        <!--标签-->
        <mu-list-item :value="LaBel_Type" :title="'#'+LaBel_Name" titleClass="myListItem" @click="open('bottom')">
          <mu-icon slot="left" :value="LaBel_Icon" color="#f7f7f7"/>
          <!--<mu-icon slot="right" value="keyboard_return" color="#ea5545"/>-->
        </mu-list-item>
        <mu-list-item v-if="docked" @click.native="open = false" title="Close">
        </mu-list-item>
      </mu-list>
    </mu-drawer>

    <!--头部bar-->
    <mu-appbar style="background-color: #3d4955;">
      <mu-icon-button icon="menu" slot="left" @click="toggle(true)"/>
      <mu-flat-button label="校园租房" slot="right"/>
    </mu-appbar>
    <!--下面弹出——标签选择-->
    <mu-popup popupClass="home-popup-bottom" position="bottom" :open="bottomPopup" @close="close('bottom')">
      <mu-flat-button label="关闭" icon="clear" class="Login-Bottom-Close" @click="close('bottom')"/>
      <mu-content-block>
        <mu-row>
          <mu-col width="50" tablet="33" desktop="33" class="Label-border" v-for="item in LaBel_Data" :key="item.id"
                  :value="activeTab">
            <mu-raised-button :label="item.name" class="demo-raised-button" :background-color="item.color"
                              @click="LaBelName(item.name,item.type,item.icon)"/>
          </mu-col>
        </mu-row>
      </mu-content-block>
    </mu-popup>
    <!--下拉刷新-->
    <mu-refresh-control :refreshing="refreshing" :trigger="trigger" @refresh="PullTop"/>
    <!--综合-->
    <div v-if="activeTab === 'colligate'" class="seize-seat-top">
      <mu-card v-for="item in colligate" :key="item.id" >
        <mu-list-item class="Top-list-item" :title="item.nickName + ' | ' + '#' + item.info.type" :describeText="item.info.releaseDate">
          <mu-avatar src="http://localhost:8081/icon.png" slot="leftAvatar"/>
          <mu-icon value="keyboard_arrow_right" slot="right" @click="go_details(item)"/>
        </mu-list-item>

        <mu-card-text >
          {{item.info.introduction}}
          <mu-badge class="demo-badge-content">#{{item.info.label}}</mu-badge>
        </mu-card-text>

        <mu-flexbox>
          <mu-flexbox-item class="flex-demo" v-for="items in item.pictureList" :key="items.id" >
            <img class="mu-col-img" v-lazy="'http://localhost:8081/'+items.picturePath"  :zDepth="1" >
          </mu-flexbox-item>
        </mu-flexbox>

        <mu-row class="card-bottom">
          <mu-col width="60" tablet="50" desktop="50">
            <div class="card-bottom-data">
              <mu-icon value="remove_red_eye" class="card-bottom-icon" :size="20"/>
              <p>{{item.info.visitNum}}</p>
            </div>
          </mu-col>
          <mu-col width="20" tablet="25" desktop="25">
            <div class="card-bottom-data">
              <mu-icon value="favorite_border" class="card-bottom-icon" :size="20"/>
              <p>{{item.info.likeNum}}</p>
            </div>
          </mu-col>
          <mu-col width="20" tablet="25" desktop="25">
            <div class="card-bottom-data">
              <mu-icon value="chat" class="card-bottom-icon" :size="20"/>
              <p>{{item.info.commentNum}}</p>
            </div>
          </mu-col>
        </mu-row>
      </mu-card>
    </div>
    <!--求租-->
    <div v-if="activeTab === 'seekrent'" class="seize-seat-top">
      <mu-card v-for="item in seekrent" :key="item.id" >
        <mu-list-item class="Top-list-item" :title="item.nickName + ' | ' + '#' + item.info.type" :describeText="item.info.releaseDate">
          <mu-avatar src="http://localhost:8081/icon.png" slot="leftAvatar"/>
          <mu-icon value="keyboard_arrow_right" slot="right" @click="go_details(item)"/>
        </mu-list-item>

        <mu-card-text >
          {{item.info.introduction}}
          <mu-badge class="demo-badge-content">#{{item.info.label}}</mu-badge>
        </mu-card-text>

        <mu-flexbox>
          <mu-flexbox-item class="flex-demo" v-for="items in item.pictureList" :key="items.id" >
            <img class="mu-col-img" v-lazy="'http://localhost:8081/'+items.picturePath"  :zDepth="1" >
          </mu-flexbox-item>
        </mu-flexbox>

        <mu-row class="card-bottom">
          <mu-col width="60" tablet="50" desktop="50">
            <div class="card-bottom-data">
              <mu-icon value="remove_red_eye" class="card-bottom-icon" :size="20"/>
              <p>{{item.info.visitNum}}</p>
            </div>
          </mu-col>
          <mu-col width="20" tablet="25" desktop="25">
            <div class="card-bottom-data">
              <mu-icon value="favorite_border" class="card-bottom-icon" :size="20"/>
              <p>{{item.info.likeNum}}</p>
            </div>
          </mu-col>
          <mu-col width="20" tablet="25" desktop="25">
            <div class="card-bottom-data">
              <mu-icon value="chat" class="card-bottom-icon" :size="20"/>
              <p>{{item.info.commentNum}}</p>
            </div>
          </mu-col>
        </mu-row>
      </mu-card>
    </div>
    <!--出租-->
    <div v-if="activeTab === 'Arent'" class="seize-seat-top">
      <mu-card v-for="item in Arent" :key="item.id" >
        <mu-list-item class="Top-list-item" :title="item.nickName + ' | ' + '#' + item.info.type" :describeText="item.info.releaseDate">
          <mu-avatar src="http://localhost:8081/icon.png" slot="leftAvatar"/>
          <mu-icon value="keyboard_arrow_right" slot="right" @click="go_details(item)"/>
        </mu-list-item>

        <mu-card-text >
          {{item.info.introduction}}
          <mu-badge class="demo-badge-content">#{{item.info.label}}</mu-badge>
        </mu-card-text>

        <mu-flexbox>
          <mu-flexbox-item class="flex-demo" v-for="items in item.pictureList" :key="items.id" >
            <img class="mu-col-img" v-lazy="'http://localhost:8081/'+items.picturePath"  :zDepth="1" >
          </mu-flexbox-item>
        </mu-flexbox>

        <mu-row class="card-bottom">
          <mu-col width="60" tablet="50" desktop="50">
            <div class="card-bottom-data">
              <mu-icon value="remove_red_eye" class="card-bottom-icon" :size="20"/>
              <p>{{item.info.visitNum}}</p>
            </div>
          </mu-col>
          <mu-col width="20" tablet="25" desktop="25">
            <div class="card-bottom-data">
              <mu-icon value="favorite_border" class="card-bottom-icon" :size="20"/>
              <p>{{item.info.likeNum}}</p>
            </div>
          </mu-col>
          <mu-col width="20" tablet="25" desktop="25">
            <div class="card-bottom-data">
              <mu-icon value="chat" class="card-bottom-icon" :size="20"/>
              <p>{{item.info.commentNum}}</p>
            </div>
          </mu-col>
        </mu-row>
      </mu-card>
    </div>
    <!--室友-->
    <div v-if="activeTab === 'roommate'" class="seize-seat-top">
      <mu-card v-for="item in roommate" :key="item.id" >
        <mu-list-item class="Top-list-item" :title="item.nickName + ' | ' + '#' + item.info.type" :describeText="item.info.releaseDate">
          <mu-avatar src="http://localhost:8081/icon.png" slot="leftAvatar"/>
          <mu-icon value="keyboard_arrow_right" slot="right" @click="go_details(item)"/>
        </mu-list-item>

        <mu-card-text >
          {{item.info.introduction}}
          <mu-badge class="demo-badge-content">#{{item.info.label}}</mu-badge>
        </mu-card-text>

        <mu-flexbox>
          <mu-flexbox-item class="flex-demo" v-for="items in item.pictureList" :key="items.id" >
            <img class="mu-col-img" v-lazy="'http://localhost:8081/'+items.picturePath"  :zDepth="1" >
          </mu-flexbox-item>
        </mu-flexbox>

        <mu-row class="card-bottom">
          <mu-col width="60" tablet="50" desktop="50">
            <div class="card-bottom-data">
              <mu-icon value="remove_red_eye" class="card-bottom-icon" :size="20"/>
              <p>{{item.info.visitNum}}</p>
            </div>
          </mu-col>
          <mu-col width="20" tablet="25" desktop="25">
            <div class="card-bottom-data">
              <mu-icon value="favorite_border" class="card-bottom-icon" :size="20"/>
              <p>{{item.info.likeNum}}</p>
            </div>
          </mu-col>
          <mu-col width="20" tablet="25" desktop="25">
            <div class="card-bottom-data">
              <mu-icon value="chat" class="card-bottom-icon" :size="20"/>
              <p>{{item.info.commentNum}}</p>
            </div>
          </mu-col>
        </mu-row>
      </mu-card>
    </div>
    <!--标签-->
    <div v-if="activeTab === LaBel_Type" class="seize-seat-top">
      <mu-card v-for="item in type_Data" :key="item.id" >
        <mu-list-item class="Top-list-item" :title="item.nickName + ' | ' + '#' + item.info.type" :describeText="item.info.releaseDate">
          <mu-avatar src="http://localhost:8081/icon.png" slot="leftAvatar"/>
          <mu-icon value="keyboard_arrow_right" slot="right" @click="go_details(item)"/>
        </mu-list-item>

        <mu-card-text >
          {{item.info.introduction}}
          <mu-badge class="demo-badge-content">#{{item.info.label}}</mu-badge>
        </mu-card-text>

        <mu-flexbox>
          <mu-flexbox-item class="flex-demo" v-for="items in item.pictureList" :key="items.id" >
            <img class="mu-col-img" v-lazy="'http://localhost:8081/'+items.picturePath"  :zDepth="1" >
          </mu-flexbox-item>
        </mu-flexbox>

        <mu-row class="card-bottom">
          <mu-col width="60" tablet="50" desktop="50">
            <div class="card-bottom-data">
              <mu-icon value="remove_red_eye" class="card-bottom-icon" :size="20"/>
              <p>{{item.info.visitNum}}</p>
            </div>
          </mu-col>
          <mu-col width="20" tablet="25" desktop="25">
            <div class="card-bottom-data">
              <mu-icon value="favorite_border" class="card-bottom-icon" :size="20"/>
              <p>{{item.info.likeNum}}</p>
            </div>
          </mu-col>
          <mu-col width="20" tablet="25" desktop="25">
            <div class="card-bottom-data">
              <mu-icon value="chat" class="card-bottom-icon" :size="20"/>
              <p>{{item.info.commentNum}}</p>
            </div>
          </mu-col>
        </mu-row>
      </mu-card>
    </div>
    <!--<div v-if="activeTab === LaBel_Type" class="seize-seat-top">-->
      <!--<mu-card v-for="item in type_Data" :key="item.id">-->
        <!--<mu-card-header :title="item.name + ' | ' + '#' + item.type" :subTitle="item.date">-->
          <!--<mu-avatar :src="item.icon + item.see" slot="avatar"/>-->
        <!--</mu-card-header>-->
        <!--<mu-card-text>-->
          <!--{{item.source}}-->
          <!--<mu-badge class="demo-badge-content">#{{item.label}}</mu-badge>-->
        <!--</mu-card-text>-->
        <!--<mu-flexbox>-->
          <!--<mu-flexbox-item class="flex-demo" v-for="items in item.url" :key="items.id">-->
            <!--<img class="mu-col-img" v-lazy="items.name + item.urlid" :zDepth="1">-->
          <!--</mu-flexbox-item>-->
        <!--</mu-flexbox>-->
        <!--<mu-row class="card-bottom">-->
          <!--<mu-col width="60" tablet="50" desktop="50">-->
            <!--<div class="card-bottom-data">-->
              <!--<mu-icon value="remove_red_eye" class="card-bottom-icon" :size="20"/>-->
              <!--<p>{{item.see}}</p>-->
            <!--</div>-->
          <!--</mu-col>-->
          <!--<mu-col width="20" tablet="25" desktop="25">-->
            <!--<div class="card-bottom-data" @click="CardFavorite(item.fabulous)">-->
              <!--<mu-icon v-if="card_bottom_favorite === item.fabulous" value="favorite" class="card-bottom-icon"-->
                       <!--:size="20"/>-->
              <!--<mu-icon v-else value="favorite_border" class="card-bottom-icon" :size="20"/>-->
              <!--<p>{{item.fabulous}}</p>-->
            <!--</div>-->
          <!--</mu-col>-->
          <!--<mu-col width="20" tablet="25" desktop="25">-->
            <!--<div class="card-bottom-data">-->
              <!--<mu-icon value="chat" class="card-bottom-icon" :size="20"/>-->
              <!--<p>{{item.comment}}</p>-->
            <!--</div>-->
          <!--</mu-col>-->
        <!--</mu-row>-->
      <!--</mu-card>-->
    <!--</div>-->
  </div>
</template>

<script>
  export default {
    data() {
      return {
        activeTab: '',
        activeList: '',
        bottomNav: 'Home',
        bottomNavColor: 'Home',
        colligate: {},
        seekrent: {},
        Arent: {},
        roommate: {},
        type_Data: {},
        bottomPopup: false,
        LaBel_Data: {},
        LaBel_Name: '',
        LaBel_Type: '',
        LaBel_Icon: '',
        refreshing: false,
        trigger: null,
        card_bottom_favorite: '',
        drawer_open: false,
        docked: true,
      }
    },
    created() {
      axios.get('label')
        .then(res => {
          if (res.status === 200) {
            this.LaBel_Data = res.data.data.list;
            sessionStorage.setItem("Lab_Type", JSON.stringify(this.LaBel_Data));
          }
        })
    },
    computed: {
      PhoneValue() {
        return this.$store.state.UserPhone;
      }
    },
    mounted() {
      this.trigger = this.$el;
      this.activeTab = JSON.parse(sessionStorage.getItem("Tab_Label")) || 'colligate';
      if (this.activeTab !== 'colligate' && this.activeTab !== 'seekrent' && this.activeTab !== 'Arent' && this.activeTab !== 'roommate') {
        this.LaBel_Type = this.activeTab;
      }
      this.LaBel_Name = (this.LaBel_Type === 'singwei' && '独卫') || (this.LaBel_Type === 'manchum' && '长租') || (this.LaBel_Type === 'girlchum' && '短租')
        || (this.LaBel_Type === 'sumroom' && '主卧') || (this.LaBel_Type === 'feedback' && '反馈') || (this.LaBel_Type === 'gossip' && '空调')
        || (this.LaBel_Type === 'lieidle' && '闲置') || (this.LaBel_Type === 'makefriends' && '交友') || '标签';
      this.LaBel_Icon = {
        'singwei': 'accessibility',
        'manchum': 'sentiment_satisfied',
        'girlchum': 'face',
        'sumroom': 'account_balance',
        'feedback': 'library_books',
        'gossip': 'public',
        'lieidle': 'remove_shopping_cart',
        'makefriends': 'supervisor_account'
      }[this.LaBel_Type] || 'view_comfy';
      // this.LoginData();
    },
    watch: {
      activeTab(curVal, oldVal) {
        sessionStorage.setItem("Tab_Label", JSON.stringify(this.activeTab));
        this.LoginData();
      },
      topPopup(val) {
        if (val) {
          setTimeout(() => {
            this.topPopup = false
          }, 2000)
        }
      }
    },
    methods: {
      toggle(flag) {
        this.drawer_open = !this.drawer_open
        this.docked = !flag
      },
      PullTop() {
        this.refreshing = true;
        setTimeout(() => {
          this.LoginData()
        }, 1000)
      },
      LoginData() {
        const active_Tab = this.activeTab;

        // if (active_Tab !== '') {
        //   this.$loading(active_Tab_CN + '话题正在' + '载入中...');
        // }
        const home = this.activeTab;
        let url="http://localhost:8081/api/colligate";
        axios.get(url)
          .then(res => {
            if (res.status === 200) {
              if (home === 'colligate') {
                this.colligate = res.data.data;
              } else if (home === 'seekrent') {
                this.seekrent = res.data.data.filter(x=>x.info.type==="求租");
              } else if (home === 'Arent') {
                this.Arent = res.data.data.filter(x=>x.info.type==="出租");
              } else if (home === 'roommate') {
                this.roommate = res.data.data.filter(x=>x.info.type==="室友");
              } else {
                this.type_Data = res.data.data.filter(x=>x.info.label===this.LaBel_Name);
              }
              this.refreshing = false;
            }
            let self = this;
            // setTimeout(function () {
            //   self.closeLoading()
            // }, 1000)
          })
      },
      handleTabChange(val) {
        this.activeTab = val;
      },
      handleListChange(val) {
        this.activeList = val;
      },
      // closeLoading() {
      //   this.$loading.close();
      // },
      handleChange(val) {
        this.bottomNav = val;
      },
      open(position) {
        this[position + 'Popup'] = true;
      },
      close(position) {
        this[position + 'Popup'] = false;
      },
      LaBelName(name, type, icon) {
        this.LaBel_Name = name;
        this.LaBel_Type = type;
        this.activeTab = type;
        this.LaBel_Icon = icon;
        this['bottom' + 'Popup'] = false;
      },
      CardFavorite(fabulous) {
        this.card_bottom_favorite = fabulous;
      },
      go_details(item) {
        sessionStorage.setItem("item",JSON.stringify(item))
        this.$router.push('/details')
      }
    }
  }
</script>
<
<style lang="less">
  .home-popup-bottom {
    margin-bottom: 56px;
    width: 100%;
    background-color: #fafafa;
  }

  .myListItem {
    color: #f7f7f7;
  }

  .mu-back-top {
    position: fixed;
  }
</style>
<style scoped lang="less">
  .mu-col-img{
    max-height: 130px;
  }

  .mu-tabs {
    position: fixed;
    top: 0;
  }

  .mu-appbar {
    position: fixed;
    width: 100%;
    top: 0;
  }

  .mu-paper {
    position: fixed;
    width: 100%;
    bottom: 0;
    table-layout: fixed;
  }

  .mu-card-text {
    padding-top: 0;
  }

  .mu-flexbox {
    padding-left: 16px;
    padding-right: 16px;
  }

  .mu-flexbox-item img {
    max-width: 100%;
    height: auto;
  }

  .Label-border {
    padding: 15px;
    text-align: center;
  }

  .mu-card {
    margin-top: 0.5em;
  }

  /*上边距*/
  .seize-seat-top {
    margin-top: 55px;
  }

  .demo-badge-content {
    background-color: #ffab00;
    color: #ffffff;
    padding-left: 4px;
    padding-right: 4px;
  }

  .card-bottom {
    border-top: 1px solid #eeeeee;
    padding: 5px 16px 5px 16px;
    color: #9e9e9e;
  }

  .card-bottom-data {
    display: -webkit-box;
    display: -webkit-flex;
    display: flex;
  }

  .card-bottom-data p {
    padding: 0;
    margin: 0px 0px 0px 5px;
    -webkit-box-flex: 1;
    -webkit-flex: 1;
    flex: 1;
    width: 0%;
  }

  .Login-Bottom-Close {
    position: fixed;
    right: 0;
    color: #9e9e9e;
  }

  img {
    transition: all 0.5s;
  }

  img[lazy=loaded] {
    animation: fade 0.5s;
  }
</style>
