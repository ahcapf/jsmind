<template>
  <div>
    <div id="jsmind_container" style="width: 100%;height: 500px;" />
    <!--自定义右键菜单html代码-->
    <div id="menu">
      <div class="menu">功能1</div>
      <div class="menu">功能2</div>
      <div class="menu">功能3</div>
      <div class="menu">功能4</div>
      <div class="menu">功能5</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TestCase',
  data() {
    return {
      jm: undefined,
      options: { // 配置
        container: 'jsmind_container', // [必选] 容器的ID，或者为容器的对象
        editable: true, // [可选] 是否启用编辑
        support_html: true, // 是否支持节点里的HTML元素
        theme: 'greensea', // [可选] 主题
        mode: 'full', // 显示模式
        view: {
          hmargin: 100, // 思维导图距容器外框的最小水平距离
          vmargin: 50, // 思维导图距容器外框的最小垂直距离
          line_width: 2, // 思维导图线条的粗细
          line_color: '#555' // 思维导图线条的颜色
        },
        layout: {
          hspace: 30, // 节点之间的水平间距
          vspace: 20, // 节点之间的垂直间距
          pspace: 13 // 节点与连接线之间的水平间距（用于容纳节点收缩/展开控制器）
        }
      },
      mind: {
      /* 元数据，定义思维导图的名称、作者、版本等信息 */
        'meta': {
          'name': 'jsMind-demo-tree',
          'author': 'hizzgdev@163.com',
          'version': '0.2'
        },
        /* 数据格式声明 */
        'format': 'node_tree',
        /* 数据内容 */
        'data': { 'id': 'root', 'topic': 'js mind', 'children': [
          { 'id': 'easy', 'topic': 'Easy', 'direction': 'left', 'expanded': false, 'children': [
            { 'id': 'easy1', 'topic': 'Easy to show' },
            { 'id': 'easy2', 'topic': 'Easy to edit' },
            { 'id': 'easy3', 'topic': 'Easy to store' },
            { 'id': 'easy4', 'topic': 'Easy to embed' }
          ] },
          { 'id': 'open', 'topic': 'Open Source', 'direction': 'right', 'expanded': true, 'children': [
            { 'id': 'open1', 'topic': 'on GitHub' },
            { 'id': 'open2', 'topic': 'BSD License' }
          ] },
          { 'id': 'powerful', 'topic': 'Powerful', 'direction': 'right', 'children': [
            { 'id': 'powerful1', 'topic': 'Base on Javascript' },
            { 'id': 'powerful2', 'topic': 'Base on HTML5' },
            { 'id': 'powerful3', 'topic': 'Depends on you' }
          ] },
          { 'id': 'other', 'topic': 'test node', 'direction': 'left', 'children': [
            { 'id': 'other1', 'topic': "I'm from local variable" },
            { 'id': 'other2', 'topic': 'I can do everything' }
          ] }
        ] }
      }
    }
  },
  mounted() {
    this.initMind()
  },
  methods: {
    initMind() {
      this.jm = new window.jsMind(this.options)
      this.jm.show(this.mind)
      const data = this.jm.get_data('node_tree').data
      this.addPopMenu(data ? [data] : undefined)
    },
    rightClick(event) {
      if (event.button === 2) { // 如果button=1（鼠标左键），button=2（鼠标右键），button=0（鼠标中间键）
        event.stopPropagation()
        this.popMenu(event)
        return false
      }
    },
    addPopMenu(data) {
      if (!data || data.length === 0) return false
      data.forEach(item => {
        const doc = document.querySelector(`jmnode[nodeid=${item.id}]`)
        doc.onmousedown = this.rightClick
        doc.tag = item
        doc.oncontextmenu = ev => {
          return false // 屏蔽右键菜单
        }
        this.addPopMenu(item.children)
      })
    },
    popMenu(e) {
      // 获取我们自定义的右键菜单
      const menu = document.querySelector('#menu')

      console.log(e)
      // 根据事件对象中鼠标点击的位置，进行定位
      menu.style.left = e.clientX - 140 + 'px'
      menu.style.top = e.clientY - 80 + 'px'

      // 改变自定义菜单的宽，让它显示出来
      menu.style.width = '125px'
      // 关闭右键菜单，很简单
      window.onclick = function(e) {
        // 用户触发click事件就可以关闭了，因为绑定在window上，按事件冒泡处理，不会影响菜单的功能
        menu.style.width = 0
      }
      return true
    }
  }
}
</script>

<style scoped>
#jsmind_container{
  background: white;
}
/*css代码*/
#menu{
  width: 0; /*设置为0 隐藏自定义菜单*/
  height: 125px;
  overflow: hidden; /*隐藏溢出的元素*/
  box-shadow: 0 1px 1px #888,1px 0 1px #ccc;
  position: absolute; /*自定义菜单相对与body元素进行定位*/
  background: white;
  z-index: 999;
}
.menu{
  width: 130px;
  height: 25px;
  line-height: 25px;
  padding: 0 10px;
}
</style>
