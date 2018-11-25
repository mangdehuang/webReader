<template>
    <div class='menu-bar'>
        <transition name='slide-up'>
                <div class="menu-wrapper" 
                    v-show="ifTitleAndMenuShow"
                    :class="{'hide-box-shadow':ifSettingShow || !ifTitleAndMenuShow}" >
                    <div class="icon-wrapper">
                        <span class="icon-menu icon"></span>
                    </div>
                    <div class="icon-wrapper">
                        <span class="icon-progress icon"></span>
                    </div>
                    <div class="icon-wrapper">
                        <span class="icon-bright icon" @click="showSetting(1)"></span>
                    </div>
                    <div class="icon-wrapper">
                        <span class="icon-a icon" @click='showSetting(0)'>A</span>
                    </div>
                </div>
        </transition>
        <transition name='slide-up'>
            <div class="setting-wrapper"  v-show="ifSettingShow">
                <div class="setting-font-size" v-if='showTag === 0'>
                    <div class="preview" :style="{fontSize:fontSizeList[0].fontSize+'px'}">A</div>
                    <div class="select">
                        <div class="select-wrapper" 
                            v-for="(item,index) of fontSizeList" 
                            :key="index"
                            @click="setFontSize(item.fontSize)"
                            >
                            <div class="line"></div>
                            <div class="point-wrapper">
                                <div class="point" v-show="item.fontSize == defaultFontSize">
                                    <div class="small-point"></div>
                                </div>
                            </div>
                            <div class="line"></div>
                        </div>
                    </div>
                    
                    <div class="preview" :style="{fontSize:fontSizeList[fontSizeList.length - 1].fontSize + 'px'}">A</div>
                </div>
                <div class="setting-theme" v-else-if='showTag === 1'>
                  <div class="setting-theme-item" 
                      v-for="(item,index) of themeList" 
                      :key="index"
                      @click="setTheme(index)">
                    <div class="preview" 
                      :style="{background:item.style.body.background}"
                      :class="{'no-border':item.style.body.background !== '#fff'}"></div>
                    <div class="text"
                      :class="{'selected':defaultTheme == index}">{{item.name}}</div>
                  </div>
               </div>
            </div>
        </transition>
        
    </div>
</template>
<script>
// hide-box-shadow 用来隐藏同时出现两个阴影
//在隐藏menubar的时候 menu-wrapper和setting-wrapper不同步  只需要将隐藏高度设置一致即可
//设置预览字体的大小 可以用fontsizelist的数组中取到设置范围
export default {
  data() {
    return {
      ifSettingShow: false,
      showTag: 0
    };
  },
  props: {
    ifTitleAndMenuShow: {
      type: Boolean,
      default: false
    },
    fontSizeList: {
      type: Array
    },
    defaultFontSize: Number,
    themeList: Array,
    defaultTheme: Number
  },
  methods: {
    setTheme(theme) {
      this.$emit("setTheme", theme);
    },
    showSetting(tag) {
      this.ifSettingShow = true;
      this.showTag = tag;
    },
    hideSetting() {
      this.ifSettingShow = false;
    },
    setFontSize(fontSize) {
      this.$emit("setFontSize", fontSize);
    }
  }
};
</script>
<style lang='scss' scoped>
@import "@/assets/style/global.scss";
.menu-bar {
  .menu-wrapper {
    position: absolute;
    bottom: 0;
    left: 0;
    z-index: 101;
    width: 100%;
    height: px2rem(48);
    display: flex;
    background: white;
    box-shadow: 0 px2rem(-8) px2rem(8) rgba(0, 0, 0, 0.15);
    .icon-wrapper {
      flex: 1;
      @include center;
    }
    &.hide-box-shadow {
      box-shadow: none;
    }
  }
  .setting-wrapper {
    z-index: 101;
    position: absolute;
    bottom: px2rem(48);
    left: 0px;
    width: 100%;
    height: px2rem(60);
    background: white;
    box-shadow: 0 px2rem(-8) px2rem(8) rgba(0, 0, 0, 0.15);
    .setting-font-size {
      display: flex;
      height: 100%;
      .preview {
        flex: 0 0 px2rem(40);
        @include center;
      }
      .select {
        display: flex;
        height: 100%;
        flex: 1;
        .select-wrapper {
          flex: 1;
          display: flex;
          align-items: center;
          &:first-child {
            .line {
              &:first-child {
                border-top: none;
              }
            }
          }
          &:last-child {
            .line {
              &:last-child {
                border-top: none;
              }
            }
          }
          .line {
            flex: 1;
            height: 0;
            border-top: px2rem(1) solid #ccc;
          }
          .point-wrapper {
            position: relative;
            flex: 0 0 0;
            height: px2rem(7);
            border-left: px2rem(1) solid #ccc;
            .point {
              position: absolute;
              top: px2rem(-8);
              left: px2rem(-10);
              width: px2rem(20);
              height: px2rem(20);
              border-radius: 50%;
              background: white;
              border: px2rem(1) solid#ccc;
              @include center;
              .small-point {
                width: px2rem(5);
                height: px2rem(5);
                background: black;
                border-radius: 50%;
              }
            }
          }
        }
      }
    }
    .setting-theme {
      display: flex;
      height: 100%;
      width: 100%;
      @include center;
      .setting-theme-item {
        flex: 1;
        height: 100%;
        display: flex;
        flex-direction: column;
        padding: px2rem(5);
        box-sizing: border-box;
        .preview {
          flex: 1;
          border: px2rem(1) solid #ccc;
          box-sizing: border-box;
          &.no-border {
            border: none;
          }
        }
        .text {
          flex: 0 0 px2rem(30);
          font-size: px2rem(16);
          @include center;
          color: #ccc;
          &.selected {
            color: #000;
          }
        }
      }
    }
  }
}
</style>

