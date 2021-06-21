<template>
  <div>
    <!-- <button @click="handleFullscreen" style="margin-bottom: 8px">full screen</button> -->
    <p>TODO: add settings bar here</p>
    <div class="chrome-ui" ref="content">
      <vue-tabs-chrome
        ref="itemSection"
        :minHiddenWidth="120"
        v-model="itemSection"
        :tabs="itemSections"
        @contextnav="handleClick"
      >
        <span slot="after" class="btn-add" @click="addSection">
          <i class="iconfont icon-plus"></i>
        </span>
      </vue-tabs-chrome>
      <!-- instead of the iframe each page should display a section 
           in the currently selected  tab -->
      <inventory-section :itemSectionProps="getItemSection">
      </inventory-section>
      <!-- <iframe :src="url" frameborder="0"></iframe> -->
    </div>
  </div>
</template>

<script>
import VueTabsChrome from "vue-tabs-chrome";
import InventorySection from "./InventorySection";

// chrome-ui icon from https://www.iconfont.cn/
export default {
  name: "ChromePage",
  components: {
    VueTabsChrome,
    InventorySection,
  },
  data() {
    let inventoryDataObj = JSON.parse(localStorage.getItem("inventoryData"));
    return {
      tab: "",
      location: "",
      tabs: [
        {
          label: "Bing",
          key: "bing",
          url: "bing.com",
          favicon: "https://bing.com/favicon.ico",
        },
      ],
      // added
      localStorageSupport: true,
      itemSection: "",
      inventoryData: inventoryDataObj,
      itemSections: inventoryDataObj["itemSection"],
    };
  },
  created() {
    let inventoryDataStr =
      "{\n" +
      '\t"itemSection": [{\n' +
      '\t\t\t"label": "First Section",\n' +
      '\t\t\t"key": "section1623506964832",\n' +
      '\t\t\t"url": "",\n' +
      '\t\t\t"favicon": "",\n' +
      '\t\t\t"itemSectionId": "section1623506964832",\n' +
      '\t\t\t"itemSectionName": "First Section",\n' +
      '\t\t\t"itemSectionCategory": [{\n' +
      '\t\t\t\t"itemSectionCategoryId": 0,\n' +
      '\t\t\t\t"itemSectionCategoryName": "Books",\n' +
      '\t\t\t\t"itemSectionCategoryItemList": [{\n' +
      '\t\t\t\t\t\t"itemSectionCategoryItemId": 0,\n' +
      '\t\t\t\t\t\t"itemSectionCategoryItemName": "For whom the Bell tolls",\n' +
      '\t\t\t\t\t\t"itemSectionCategoryItemStatus": "Bought"\n' +
      "\t\t\t\t\t},\n" +
      "\t\t\t\t\t{\n" +
      '\t\t\t\t\t\t"itemSectionCategoryItemId": 1,\n' +
      '\t\t\t\t\t\t"itemSectionCategoryItemName": "Grapes of Wrath",\n' +
      '\t\t\t\t\t\t"itemSectionCategoryItemStatus": "Rented"\n' +
      "\t\t\t\t\t}\n" +
      "\t\t\t\t]\n" +
      "\t\t\t}, {\n" +
      '\t\t\t\t"itemSectionCategoryId": 1,\n' +
      '\t\t\t\t"itemSectionCategoryName": "Movies",\n' +
      '\t\t\t\t"itemSectionCategoryItemList": [{\n' +
      '\t\t\t\t\t\t"itemSectionCategoryItemId": 0,\n' +
      '\t\t\t\t\t\t"itemSectionCategoryItemName": "A Clockwork Orange",\n' +
      '\t\t\t\t\t\t"itemSectionCategoryItemStatus": "Bought"\n' +
      "\t\t\t\t\t},\n" +
      "\t\t\t\t\t{\n" +
      '\t\t\t\t\t\t"itemSectionCategoryItemId": 1,\n' +
      '\t\t\t\t\t\t"itemSectionCategoryItemName": "Cidade des Deus",\n' +
      '\t\t\t\t\t\t"itemSectionCategoryItemStatus": "Bought"\n' +
      "\t\t\t\t\t}\n" +
      "\t\t\t\t]\n" +
      "\t\t\t}]\n" +
      "\t\t},\n" +
      "\t\t{\n" +
      '\t\t\t"label": "Second Section",\n' +
      '\t\t\t"key": "section1623506961241",\n' +
      '\t\t\t"url": "",\n' +
      '\t\t\t"favicon": "",\n' +
      '\t\t\t"itemSectionId": "section1623506961241",\n' +
      '\t\t\t"itemSectionName": "Second Section",\n' +
      '\t\t\t"itemSectionCategory": [{\n' +
      '\t\t\t\t"itemSectionCategoryId": 0,\n' +
      '\t\t\t\t"itemSectionCategoryName": "Books",\n' +
      '\t\t\t\t"itemSectionCategoryItemList": [{\n' +
      '\t\t\t\t\t\t"itemSectionCategoryItemId": 0,\n' +
      '\t\t\t\t\t\t"itemSectionCategoryItemName": "The Count of Monte Cristo",\n' +
      '\t\t\t\t\t\t"itemSectionCategoryItemStatus": "Rented"\n' +
      "\t\t\t\t\t},\n" +
      "\t\t\t\t\t{\n" +
      '\t\t\t\t\t\t"itemSectionCategoryItemId": 1,\n' +
      '\t\t\t\t\t\t"itemSectionCategoryItemName": "Wealth of Nations",\n' +
      '\t\t\t\t\t\t"itemSectionCategoryItemStatus": "Rented"\n' +
      "\t\t\t\t\t}\n" +
      "\t\t\t\t]\n" +
      "\t\t\t}, {\n" +
      '\t\t\t\t"itemSectionCategoryId": 1,\n' +
      '\t\t\t\t"itemSectionCategoryName": "Movies",\n' +
      '\t\t\t\t"itemSectionCategoryItemList": [{\n' +
      '\t\t\t\t\t\t"itemSectionCategoryItemId": 0,\n' +
      '\t\t\t\t\t\t"itemSectionCategoryItemName": "Twelve Angry Men",\n' +
      '\t\t\t\t\t\t"itemSectionCategoryItemStatus": "Rented"\n' +
      "\t\t\t\t\t},\n" +
      "\t\t\t\t\t{\n" +
      '\t\t\t\t\t\t"itemSectionCategoryItemId": 1,\n' +
      '\t\t\t\t\t\t"itemSectionCategoryItemName": "Casablanca",\n' +
      '\t\t\t\t\t\t"itemSectionCategoryItemStatus": "Rented"\n' +
      "\t\t\t\t\t}\n" +
      "\t\t\t\t]\n" +
      "\t\t\t}]\n" +
      "\t\t}\n" +
      "\t]\n" +
      "}";
    console.log("Inventory data: " + inventoryDataStr);
    try {
      localStorage.setItem("inventoryData", inventoryDataStr);
      console.log("Local storage set");
    } catch (e) {
      console.error("Local storage not supported");
      this.localStorageSupport = false;
    }
  },
  mounted() {
    // TODO:
    //      --save last opened section and open it here
    this.tab = "bing";
    this.itemSection = this.itemSections[0].label;
    console.log("mounted: this.itemSection: ");
    console.log(this.itemSection);
  },
  computed: {
    url() {
      let currTab = this.tabs.find((item) => item.key === this.tab);
      let url = "";
      if (currTab) {
        url = currTab.url || "";
      }
      if (!url.startsWith("http://") && !url.startsWith("https://")) {
        url = "https://" + url;
      }
      return url;
    },
    getItemSection() {
      console.log("this.itemSections: ");
      console.log(this.itemSections);
      console.log("this.itemSection: ");
      console.log(this.itemSection);
      let currSection = this.itemSections.find(
        (item) => item.key === this.itemSection
      );
      if (currSection) {
        console.log("not null");
        console.log(currSection);
      } else {
        console.log("null");
      }
      return currSection;
    },
  },
  watch: {
    tab() {
      let tab = this.getCurrTab();
      let location = tab.url || "";
      if (!location) {
        return "";
      }
      if (!location.startsWith("http://") && !location.startsWith("https://")) {
        location = "https://" + location;
      }

      this.location = location;
    },
    itemSection() {
      // tab() seems to only get/set the location which
      // is not needed
    },
  },
  methods: {
    addTab() {
      let item = "tab" + Date.now();
      let newTabs = [
        {
          label: "New Tab",
          key: item,
        },
      ];
      this.$refs.tab.addTab(...newTabs);
      this.tab = item;
      this.location = "";
    },
    addSection() {
      let item = "section" + Date.now();
      let newSections = [
        {
          label: "New Section",
          key: item,
        },
      ];
      this.$refs.itemSection.addTab(...newSections);
      this.itemSection = item;
      this.location = "";
    },
    removeTab() {
      this.$refs.tab.removeTab(this.tab);
    },
    handleClick(e, tab, i) {
      console.log(e, tab, i);
    },
    getCurrTab() {
      return this.tabs.find((item) => item.key === this.tab);
    },
    handleNavClick(msg) {
      alert(msg);
    },
    handleCollection() {
      alert("😆");
    },
    handleMore() {
      alert("🤗");
    },
    handleFullscreen() {
      this.$refs.content.requestFullscreen();
    },
  },
};
</script>

<style lang="less">
.chrome-ui {
  box-shadow: 2px 2px 5px #ddd;
  input[type="search"]::-webkit-search-cancel-button {
    -webkit-appearance: none;
  }
  .btn-add {
    width: 28px;
    height: 28px;
    border-radius: 50%;
    padding: 0 10px;
    color: #333;
    cursor: pointer;
    display: flex;
    justify-content: center;
    align-items: center;
    transition: background 300ms;
    &:hover {
      background-color: rgba(0, 0, 0, 0.1);
    }
  }
  .nav {
    padding: 8px;
    background-color: #fff;
    border-bottom: 1px solid #d5d7db;
    display: flex;
    align-items: center;
    position: relative;
  }
  .nav-btns {
    display: flex;
  }
  .nav-btn {
    width: 28px;
    height: 28px;
    margin-left: 4px;
    border-radius: 14px;
    color: #666;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: background 300ms;
    cursor: pointer;
    &:first-of-type {
      margin-left: 0;
    }
    &:hover {
      background-color: rgba(0, 0, 0, 0.1);
    }
  }
  .nav-location {
    flex: 1;
    height: 28px;
    margin: 0 8px;
    position: relative;
  }
  .nav-location-input {
    width: 100%;
    height: 100%;
    border: none;
    background-color: #eff1f2;
    border-radius: 14px;
    outline: none;
    padding-left: 16px;
    transition: background 300ms;
    &:hover {
      background-color: #e6e8e9;
    }
    &:focus {
      box-shadow: 0 0 0 3px Highlight;
      background-color: #fff;
    }
  }
  .nav-collection {
    top: 50%;
    right: 2px;
    width: 32px;
    height: 24px;
    border-radius: 12px;
    position: absolute;
    transform: translateY(-50%);
  }
  iframe {
    width: 100%;
    min-height: calc(100vh - 90px);
    background-color: #f4f4f4;
  }
  @font-face {
    font-family: "iconfont";
    src: url("//at.alicdn.com/t/font_2498827_ixeycf9piwi.eot?t=1618909191934"); /* IE9 */
    src: url("//at.alicdn.com/t/font_2498827_ixeycf9piwi.eot?t=1618909191934#iefix")
        format("embedded-opentype"),
      /* IE6-IE8 */
        url("data:application/x-font-woff2;charset=utf-8;base64,d09GMgABAAAAAARMAAsAAAAACRgAAAP9AAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAHEIGVgCDbAqEWIN6ATYCJAMgCxIABCAFhG0HXBvgB8iemjwZo6X/pzEGCqv++RUMp3j4WkPf3907DqBLVFKWlYAeEITvVAHaqhpTX2Wqg0bcn2tvrwySMlMLqAhg4SW7/ZCdbArAEtACuU49Htpzd/pY8v+/v7rz78jJ2ATFUdxtI8Q5m90ohkRXqG9+Ojk88O0A2hZ4Con+dXWY+lW8rp29S6vF/rkVaoUf6IFqT62UiVgCkglF0POA3UQt4GICvXWJPC6U1bSAvwLmBeKdyRTwj6mVksTQCnXO1iJ+AUObPlMXwE/3+2NfRrBJqgyWXnpRqkPBFn4v4CyZFbMxwOnPCG4XGceAQnzKNX8E9MPHeOmVv6W7AcxqJfVW8zX3degr/DW/JYSAOK44GTrDLIqt5x9ekVSyhqhR8SN4RnobtkxEsCVC1GzZEIWtMCKx1YCo2PUgMrtetYbEt7PngQG/AWwRxBPmLBCbFNHBL97gqBlSLrd7e3q/cTt7s37X4g2z8WDZEji7CNRoLZfB/hJPkD4smYMWVpRegq+E0KPoJhOhrEnHZho20e4ZfPPD+R7sf4z/+YN+X+CLKwJP5Gc+pyjQ4hl21/PoJlO4f+eBl5rYZYsdGyY6lZZOdaC7TWtvaWltaW9ta2/rDtMbGW3A0NMJtr8J68Ev8tSsv+W04fC48OHI8pJwBiRSkSU5kSMpx4hHTp6n9CBFwyErNhddrtWp6ampBvejzyA9dUYiAC9fVpY/MJLekVl1J9dj1KOyFLB0WK7QmqITZl8Oa6k6OB0yzZDZ/r/yyoL/8s8Ejlrmv9bqGaEDspDQ+nN7ege9i9ayE9Bl946xMOnze44zfhcZNsNnxIbu8Mj9WUz3juH1XoJm0AyaQXdD/8XJaCrAO6RHXh5Mb3kcLQDTZ57ENICJYHuFMj3mizj1PN9nj3UalT/Kz/hT0JufDMv5b4MK+D97xHJd03mB+uy3LaFwfgtOzaKi8kDTFjk78ySB1gYvkNBjBb+DfdGnPqUrtYOENsbhIGmsiXNPHBJbiMegMjgOtdZp6B1VvnuwRPASpcMRSwSEebeQzPoH2bz32EL8CJVVP6A2Hwi9GxF15OBAFAYWC1Qk6kQdI6bG3dTQyUAvsQnZkFMRUZ5LakNhmz6SlpyajJWjG0UfK+xhli4lJVRwFykjLodOJyeW4IOoyWRDSisrJYXmvVOyxl0Q6DwBKSSkI1RjCJOGc6N2bykw9fVNEDPESSFK0ir0bZBgMwcn0iRLrYEtF7trpZ3LY2zDmHSSRBGUwLkQZYQROTHMEVb+pEFIIyUzWgQtWVKocrSuOHl8qetK90EPXqKNFDlKVFFHE210ynsu0NtyDtneTqTST1gM40y24W1wF3rbUhE+wmSGBAAAAAA=")
        format("woff2"),
      url("//at.alicdn.com/t/font_2498827_ixeycf9piwi.woff?t=1618909191934")
        format("woff"),
      url("//at.alicdn.com/t/font_2498827_ixeycf9piwi.ttf?t=1618909191934")
        format("truetype"),
      /* chrome, firefox, opera, Safari, Android, iOS 4.2+ */
        url("//at.alicdn.com/t/font_2498827_ixeycf9piwi.svg?t=1618909191934#iconfont")
        format("svg"); /* iOS 4.1- */
  }

  .iconfont {
    font-family: "iconfont" !important;
    font-size: 16px;
    font-style: normal;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }

  .icon-more:before {
    content: "\e607";
  }

  .icon-plus:before {
    content: "\e659";
  }

  .icon-left:before {
    content: "\e943";
  }

  .icon-refresh:before {
    content: "\e628";
  }

  .icon-home:before {
    content: "\e687";
  }

  .icon-star:before {
    content: "\e60f";
  }

  .icon-right:before {
    content: "\e944";
  }
}
</style>
