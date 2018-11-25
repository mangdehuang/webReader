<template>
    <div class="ebook">
        <title-bar :ifTitleAndMenuShow='ifTitleAndMenuShow'></title-bar>       
        <div class="read-wrapper">
            <div id="read"></div>
            <div class="mask">
                <div class="left" @click="prePage"></div>
                <div class="center" @click="toggelTitleAndMenu"></div>
                <div class="right" @click="nextPage"></div>
            </div>
        </div>
        <menu-bar
         :ifTitleAndMenuShow='ifTitleAndMenuShow'
         :fontSizeList = "fontSizeList"
         :defaultFontSize = "defaultFontSize"
         ref="menuBar"
         @setFontSize = 'setFontSize'
         :themeList = 'themeList'
         :defaultTheme = 'defaultTheme'
         @setTheme = 'setTheme'
         ></menu-bar>
    </div>
</template>
<script>
import Epub from "epubjs";
import TitleBar from "@/components/TitleBar";
import MenuBar from "@/components/MenuBar";
const DOWNLOAD_URL = "/static/2018_Book_AgileProcessesInSoftwareEngine.epub";
export default {
  data() {
    return {
      ifTitleAndMenuShow: false,
      fontSizeList: [
        { fontSize: 12 },
        { fontSize: 14 },
        { fontSize: 16 },
        { fontSize: 18 },
        { fontSize: 20 },
        { fontSize: 22 },
        { fontSize: 24 }
      ],
      defaultFontSize: 16,
      themeList: [
        {
          name: "default",
          style: {
            body: {
              color: "#000",
              background: "#fff"
            }
          }
        },
        {
          name: "eye",
          style: {
            body: {
              color: "#000",
              background: "#ceeaba"
            }
          }
        },
        {
          name: "night",
          style: {
            body: {
              color: "#fff",
              background: "#000"
            }
          }
        },
        {
          name: "gold",
          style: {
            body: {
              color: "#000",
              background: "rgb(241,236,226)"
            }
          }
        }
      ],
      defaultTheme: 0
    };
  },
  components: {
    TitleBar,
    MenuBar
  },
  methods: {
    setTheme(index) {
      this.themes.select(this.themeList[index].name);
      this.defaultTheme = index;
    },
    registerTheme() {
      this.themeList.forEach(theme => {
        this.themes.register(theme.name, theme.style);
      });
    },
    showEpub() {
      this.book = new Epub(DOWNLOAD_URL);
      this.rendition = this.book.renderTo("read", {
        width: window.innerWidth,
        height: window.innerHeight
      });
      this.rendition.display();
      this.themes = this.rendition.themes;
      this.setFontSize(this.defaultFontSize);
      this.registerTheme(); //注册主题
      this.setTheme(this.defaultTheme);
    },
    prePage() {
      if (this.rendition) {
        this.rendition.prev();
      }
    },
    nextPage() {
      if (this.rendition) {
        this.rendition.next();
      }
    },
    toggelTitleAndMenu() {
      this.ifTitleAndMenuShow = !this.ifTitleAndMenuShow;
      if (!this.ifTitleAndMenuShow) {
        //通过调用父组件的refs来调用子组件的方法 来修改子组件里面的属性
        this.$refs.menuBar.hideSetting();
      }
    },
    setFontSize(fontSize) {
      this.defaultFontSize = fontSize;
      if (this.themes) {
        this.themes.fontSize(fontSize + "px");
      }
    }
  },
  mounted() {
    this.showEpub();
  }
};
</script>
<style lang='scss' scoped>
@import "assets/style/global.scss";
.ebook {
  .read-wrapper {
    position: relative;
    .mask {
      position: absolute;
      top: 0;
      left: 0;
      height: 100%;
      width: 100%;
      display: flex;
      z-index: 100;
      .left {
        flex: 0 0 px2rem(100);
      }
      .center {
        flex: 1;
      }
      .right {
        flex: 0 0 px2rem(100);
      }
    }
  }
}
</style>


