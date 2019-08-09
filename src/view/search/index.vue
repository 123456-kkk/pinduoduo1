<template>
  <div class="wrap">
    <header class="header">
      <div>
        <span>气质上衣女</span>
      </div>
    </header>
    <div class="main">
      <div class="list">
        <div class="list-left">
          <ul>
            <li
              v-for="(item,ind) in searchList"
              :key="ind"
              :class="{active:ind==selectIndex}"
              ref="lis"
              @click="leftscroll(ind)"
            >{{item.name}}</li>
          </ul>
        </div>
        <div class="list-right">
          <ul class="biglist" ref="list">
            <li class="list-item" v-for="(item,ind) in searchList" :key="ind">
              <div class="head">
                <h3>{{item.name}}</h3>
                <span>查看更多</span>
              </div>
              <ul class="smalllist">
                <li v-for="(v,k) in item.items" :key="k">
                  <img :src="v.icon" />
                  <p>{{v.title}}</p>
                </li>
              </ul>
            </li>
          </ul>
        </div>
      </div>
    </div>
    <Footer></Footer>
  </div>
</template>

<script>
import BScroll from "better-scroll";
import { mapState, mapActions } from "vuex";
export default {
  data() {
    return {
      scrollTop: [],
      scrollHeight: 0
    };
  },
  computed: {
    ...mapState(["searchList"]),
    selectIndex() {
      return this.scrollTop.findIndex((item, index) => {
        this.left_right(index)
        return (
          this.scrollHeight >= item &&
          this.scrollHeight < this.scrollTop[index + 1]
        );
      });
    }
  },
  methods: {
    ...mapActions(["_getsearchList"]),
    left() {
      this.leftScroll = new BScroll(".list-left");
    },
    right() {
      this.rightScroll = new BScroll(".list-right", {
        click: true,
        probeType: 2
      });
    },
    left_right(index) {
      let el = this.$refs.lis[index];
      this.leftScroll.scrollToElement(el, 300, 0, -500);
    },
    leftscroll(ind) {
      this.rightScroll.scrollTo(0, -this.scrollTop[ind]);
    }
  },

  watch: {
    searchList() {
      this.$nextTick(() => {
        let start = 0;
        let arr = [];
        arr.push(start);
        let lists = Array.from(
          this.$refs.list.getElementsByClassName("list-item")
        );

        lists.forEach((item, index) => {
          start += item.clientHeight;
          arr.push(start);
        });
        this.scrollTop = arr;
        this.rightScroll.on("scroll", pos => {
          this.scrollHeight = Math.abs(pos.y);
        });
      });
    }
  },
  mounted() {
    this._getsearchList();
    this.left();
    this.right();
    this.leftscroll()
  }
};
</script>

<style scoped>
.header {
  width: 100%;
  height: 1.3333rem;
  padding: 10px 0 0 0;
  border-bottom: 1px solid #f4f4f4;
}
.header div {
  width: 90%;
  height: 70%;
  background: #ededed;
  border: none;
  border-radius: 0.08rem;
  margin-left: 10px;
  text-align: center;
  line-height: 34.9px;
  color: #9c9c9c;
  font-size: 15px;
}
.list {
  width: 100%;
  height: 100%;
  /* background: red; */
  display: flex;
}
.list-left {
  width: 25%;
  height: 100%;
  overflow: hidden;
}

.list-left ul {
  width: 100%;
}
.list-left ul li {
  width: 100%;
  height: 53px;
  text-align: center;
  line-height: 53px;
  background: #fafafa;
  position: relative;
}
.list-left ul .active {
  background: #fff;
  border-left: 1px solid #e02e24;
}

.list-right {
  width: 75%;
  overflow: hidden;
}
.list-right::-webkit-scrollbar {
  display: none;
}
.list-right .biglist {
  width: 100%;
}
.list-right .biglist .head {
  width: 100%;
  height: 1rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.list-right .biglist .smalllist {
  width: 100%;
  display: flex;
  flex-wrap: wrap;
}
.list-right .biglist .smalllist li {
  width: 87px;
  height: 75px;
  text-align: center;
}
.list-right .biglist .smalllist img {
  width: 56px;
  height: 56px;
}
.list-item {
  padding: 10px;
}
</style>
