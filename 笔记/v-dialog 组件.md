#### 公用组件的封装

组件内只实现基本功能

组件的其他(定制)功能通过预留接口的方式实现

#### Dialog组件
```
<template>
  <div v-if="isShow" class="pop-dialog" @click.self="toggle">
    <div class="child">
      <div>this is Dialog</div>
      <slot></slot>
      <!-- <p>
        <button @click="this.father.sure">child sure</button>
        <button @click="this.father.cancel">child cancel</button>
      </p>-->
      <p>
        <button @click.stop="show">child sure</button>
        <button @click.stop="hide">child cancel</button>
      </p>
    </div>
  </div>
</template>
<script>
export default {
  props: ["father"],
  data() {
    return {
      isShow: true
    };
  },
  methods: {
    toggle() {
      this.isShow = !this.isShow;
    },
    show() {
      this.toggle();
      this.father.sure && this.father.sure();
    },
    hide() {
      this.father.cancel && this.father.cancel();
    }
  }
};
</script>
<style scoped>
</style>
```
#### 父组件

```
<template>
  <div class="home">
    <button @click="flag">click</button>
    <div>{{result}}</div>
    <div>{{ipt}}</div>
    <Dialog v-if="isShow" :father="this">
      <input type="text" v-model="ipt" />
      <!-- <div>
        <button @click="sure">确认</button>
      </div>
      <div>
        <button @click="cancel">取消</button>
      </div> -->
    </Dialog>
  </div>
</template>

<script>
import Dialog from "@/components/Dialog.vue";
export default {
  name: "home",
  data() {
    return {
      isShow: false,
      result: "",
      ipt: ""
    };
  },
  components: {
    Dialog
  },
  methods: {
    flag() {
      this.isShow = !this.isShow;
    },
    sure() {
      this.flag();
      this.result = "你点了确认";
    },
    cancel() {
      this.flag();
      this.ipt = "";
      this.result = "你点了取消";
    }
  }
};
</script>
<style>
.pop-dialog {
  position: fixed;
  top: 0;
  bottom: 0;
  width: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
}
.child {
  margin: auto;
  width: 300px;
  height: 300px;
  background: #fff;
}
input {
  width: 280px;
  height: 100px;
}
</style>
```