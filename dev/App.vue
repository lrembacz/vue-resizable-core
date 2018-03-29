<template>
  <div :style="styles">
	Test
    <vue-resizable-core
      :onResizeStart="onResizeStart"
      :onResize="onResize"
      :onResizeStop="onResizeStop"
	  :axis="'both'"
      :grid="[5,5]"
	  :w="width"
	  :h="height"
	  :lockAspectRatio="false"
	  :minConstraints="[20,20]"
	  :maxConstraint="[Infinity, Infinity]"
      :className="'resizable'"
    >
      <div class="resizable-handle"></div>
    </vue-resizable-core>
  </div>
</template>
 
<script type="text/javascript">
import VueResizableCore from '../src/index.js'
 
export default{
  data() {
    return {
	  width: 200,
	  height: 200
	}
  },
  computed:{
    styles(){
	  return {
		'background-color':'blue',
		'width': this.width + 'px',
		'height': this.height + 'px',
		'display':'block',
		'position':'relative'
	  };
	}
  },
  components:{
    VueResizableCore
  },
  methods:{
    onResizeStart(e, { node, size }){
      console.log('onResizeStart');
      console.log('e', e);
      console.log('size', size);
	  this.width = size.width;
    },
    onResize(e, { node, size }){
      console.log('onResize');
      console.log('e', e);
      console.log('size', size);
	  this.height = size.height;
	  this.width = size.width;
    },
    onResizeStop(e, { node, size }){
      console.log('onResizeStop');
      console.log('e', e);
      console.log('size', size);
	  this.height = size.height;
	  this.width = size.width
    },
  }
  
}
</script>

<style>
    .resizable-handle {
        position:absolute;
        width:20px;
        height:20px;
        bottom:0;
        right:0px;
        text-align:right;
    }
    .resizable-handle::after {
        content: "";
        position: absolute;
        right: 3px;
        bottom: 3px;
        width: 5px;
        height: 5px;
        border-right: 2px solid #FFFFFF;
        border-bottom: 2px solid #FFFFFF;
    }
</style>
