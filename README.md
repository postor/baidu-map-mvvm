# baidu-map-mvvm
a simple mvvm for baidu map | 以MVVM的方式使用百度地图

## usage | 使用

html

```
<script type="text/javascript" src="http://api.map.baidu.com/api?v=2.0&ak=YOUR_API_KEY"></script>
<script type="text/javascript" src="./dist/baidu-map-mvvm.umd.js"></script>
<script type="text/javascript" src="./index.js"></script>
```

index.js

```
// 百度地图API功能
var map = new BMap.Map("allmap");  // 创建Map实例
map.centerAndZoom(new BMap.Point(116.404, 39.915), 15);  // 初始化地图,设置中心点坐标和地图级别
//添加地图类型控件
map.addControl(new BMap.MapTypeControl({
  mapTypes:[
    BMAP_NORMAL_MAP,
    BMAP_HYBRID_MAP
  ]
}));	  
map.enableScrollWheelZoom(true);   //开启鼠标滚轮缩放

const items = [
  {
    Constructor: BMap.Polyline,
    data: [
      [
        new BMap.Point(116.399, 39.910),
        new BMap.Point(116.405, 39.920),
        new BMap.Point(116.423493, 39.907445)
      ], 
      {
        strokeColor:"blue", 
        strokeWeight:2, 
        strokeOpacity:0.5
      },
    ],
  }
]

const vm = new MapMvvm({
  items,
}, map)

```

## CMD && ES6

install

```
npm i baidu-map-mvvm
```

use

```
const MapMvvm = require('baidu-map-mvvm')

//or ES6
//import MapMvvm from 'baidu-map-mvvm'
```

## apis

### config

```
const config = {
    // items to render
    items: [{
        // constructor
        Constructor: baidu.maps.Polyline,

        // param for constructor
        data: {
            ...
        },

        // listen to events
        listeners:{
          // like click
          click: function(e){
            ...
          }
        },

        // init action
        init: function(){}
    }], 
    // methods
    methods: {
        log: function(){}
    },
    ready: function(){}
}

new MapMvvm(config,map) 
```

### context

- ready: this = MapMvvm instance
- methods: in each method, this = MapMvvm instance
- item.listeners: in each listener, this = item in items
- item.init: this = item in items
- item.$ref: the instance of item.Constructor
- item.$vm: MapMvvm instance

```
const config = {
    items: [{
        Constructor: BMap.Polyline,
        data: {
            ...
        },
        listeners:{
          click: function(e){
            // call from listener
            this.$vm.log(this)
          }
        },
        init: function(){
          this.$vm.log(this)
        }
    }], 
    // methods
    methods: {
        log: function(...args){
            console.log(args)
        }
    },
    ready: function(){
        // call from ready
        this.log('ready')
    }
}
const vm = new MapMvvm(config,map) 

// call from outside
vm.log('outside')
```

### setItems, addItem, removeItem

```
// update all
vm.$setItems(items)

// add
vm.$addItem(item)

// remove
vm.$removeItem(vm.$items[0])
```
use `this` instead of `vm` in `ready` or `method`, use `this.$vm` in `listener` 

## try it out | 体验一下

```
git clone https://github.com/postor/baidu-map-mvvm.git
cd baidu-map-mvvm
npm install && npm run build && npm run start
```

open http://localhost:8080