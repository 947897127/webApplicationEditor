<template>
  <div class="home">
    <h1>{{cList.length}}组件列表</h1>

    <div>
      <button @click="addComponent()" style="margin-right: 20px">add</button>
      <button @click="removeComponent()">remove</button>
      <button @click="addPage()">增加子页面</button>
    </div>

    <component v-for="item in cList" :uuid="item.base.uuid" :key="item.base.uuid" :id="item.name" :is="item.name"/>
    
  </div>
</template>

<script>
import Ctrl from '../components/Ctrl';

export default {
  name: 'BaseView',
  components: { Ctrl },
  data() {
    return {
        components: ["Input"],
        cList: [],
    }
  },
  methods: {
      addComponent() {
            let name = 'Input';
            /**
             * 实例了一个组件之后, 需要增加其配置信息到映射里面
             */
            let config = this.$C.addComponentsUuidMap(name);
            /**
             * 挂载并渲染
             */
            this.$C.install([name]) && this.cList.push(config);
            /**
             * 增加到页面映射关系
             */
            this.$P.setPathUuidMap(this.$router.currentRoute.name, config.base.uuid);
      },
      removeComponent() {
          console.log(this.$C.getFocus)
      },
      addPage() {
        console.log(this.$router.currentRoute.name)
        // this.$P.addPage();
      }
  },
  created() {

  },
  mounted() {
    let path = this.$router.currentRoute.name;

    if(this.$P.pathUuidMap[path]){
        this.cList = this.$P.pathUuidMap[path].map(uuid => this.$C.componentsUuidMap[uuid]);
        this.$C.install(this.cList.map(c => c.name));
        console.log(this.cList)
    }
    
    /**
     * 定时删除组件
     * 模拟拖拽增加组件
     */
    // setTimeout(()=> {
    //   this.$C.install(["Input"]) && this.components.push(this.$C.pack("Input"));
    //   /**
    //    * 组件删除
    //    */
    //   // this.components.shift();
    // }, 2000)
  }
}
</script>
