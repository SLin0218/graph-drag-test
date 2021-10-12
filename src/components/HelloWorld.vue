<template>
  <div>
    <div id="topo">
    </div>
    <br/>
    <br/>
    <br/>
    <br/>
    <div id="out" class="out" draggable="true">外部节点</div>
  </div>
</template>

<script>
import G6 from '@antv/g6';

export default {
  name: 'HelloWorld',
  data() {
    return {
      data: {
        // 点集
        nodes: [
          {
            id: 'node1', // String，该节点存在则必须，节点的唯一标识
            x: 100, // Number，可选，节点位置的 x 值
            y: 200, // Number，可选，节点位置的 y 值
          },
          {
            id: 'node2', // String，该节点存在则必须，节点的唯一标识
            x: 300, // Number，可选，节点位置的 x 值
            y: 200, // Number，可选，节点位置的 y 值
          },
        ],
        // 边集
        edges: [
          {
            source: 'node1', // String，必须，起始点 id
            target: 'node2', // String，必须，目标点 id
          },
        ],
      },
      ids: 4,
    }
  },
  mounted() {
    this.graph = new G6.Graph({
      container: 'topo', // String | HTMLElement，必须，在 Step 1 中创建的容器 id 或容器本身
      width: 800, // Number，必须，图的宽度
      height: 500, // Number，必须，图的高度
    });
    this.graph.read(this.data);
    this.graph.on("mouseup", evt => {
      // 一些操作
      console.log(evt);
    })
    document.getElementById("topo");
    this.drag();
  },
  methods: {
    drag() {
      function dragstart_handler(ev) {
        // Add the target element's id to the data transfer object
        ev.dataTransfer.setData("text/plain", ev.target.id);
      }

      // Get the element by id
      const element = document.getElementById("out");
      const element2 = document.getElementById("topo");
      // Add the ondragstart event listener
      element.addEventListener("dragstart", dragstart_handler);
      const that = this;

      element2.addEventListener("drop", (evt) => {
        const point = that.graph.getPointByClient(evt.clientX, evt.clientY);
        console.log(point)
        const model = {
          id: 'node' + that.ids,
          label: 'lin',
          address: 'cq',
          x: point.x,
          y: point.y,
          style: {
            fill: 'blue',
          },
        };
        that.graph.addItem("node", model);
        that.ids += 1;
      })
    }
  }
}
</script>

<style>
#topo {
  border: 5px solid red;
}

.out {

}
</style>
