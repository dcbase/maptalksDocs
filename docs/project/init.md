
###### 一、地图初始化绘制

可参考[官方文档--Example--初始化](https://maptalks.org/examples/en/map/load/)测试用例；

maptalks.css、maptalks.min.js必须加载

地图初始化主要问题为 底图、地图中心点位置、地图缩放大小 

[demo01](https://github.com/dcbase/maptalksDemo/blob/master/init.html)

    <!DOCTYPE html>
    <html>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1">
      <title>Map - Display a map</title>
      <style type="text/css">
        html,body{margin:0px;height:100%;width:100%}
        .container{width:100%;height:100%}
      </style>
      <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/maptalks/dist/maptalks.css">
      <script type="text/javascript" src="https://cdn.jsdelivr.net/npm/maptalks/dist/maptalks.min.js"></script>
      <body>
    
        <div id="map" class="container"></div>
    
        <script>
        
        //地图底图地址
        let mapUrl = 'https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}.png'
        
        //地图中心位置
        let mapCenter = [-0.113049,51.498568]
        
        const map = new maptalks.Map('map', {
            center: mapCenter,
            zoom: 14,  //地图放大倍数
            baseLayer: new maptalks.TileLayer('base', {
                urlTemplate: mapUrl,
                subdomains: ['a','b','c','d'],
                attribution: '&copy; <a href="http://osm.org">OpenStreetMap</a> contributors, &copy; <a href="https://carto.com/">CARTO</a>'
            })
        });
    
        </script>
      </body>
    </html>



###### 二、绘制区域块

可参考[官方文档--Example--绘制区域（ Polygon ）](https://maptalks.org/examples/en/geometry/polygon/#geometry_polygon)测试用例；

需要引入相关区域json，绘制区域块是通过点位连接绘制线、面，所以需引入相关地级市GeoJson格式的数据(示例json:[meishan.json](https://github.com/dcbase/maptalksDemo/blob/master/json/meishan.json))，相关代码 [github](https://github.com/dcbase/maptalksDemo/blob/master/3d.html)

相关代码如下：

    <script>
    
    const edgeColor	= '#4682B4'
    const polygonColors = ["#C0C0C0", "#87CEFA"]
    
    drawPolygons(idx, coordinates, properties) {
        const polygon = new maptalks.MultiPolygon(coordinates, {
            symbol: {
                lineWidth: 1,
                lineColor: edgeColor,  //绘制边界线颜色属性
                polygonFill: polygonColors[0],  //绘制区域面积颜色属性
                polygonOpacity: 1,  //透明度
                text:properties.name
            },
            properties: {
                altitude: altitude,
                id: properties.id,
                name: properties.name,
                index: idx,
                properties: properties
            }
        })
        .on("mouseenter", function(e) {
            //鼠标划入区域块颜色值
            e.target.updateSymbol({
                polygonFill: polygonColors[1]
            });
        })
        .on("mouseout", function(e) {
            //鼠标离开区域块颜色值
            e.target.updateSymbol({
                polygonFill: polygonColors[0]
            });
        })
        this.polygons.push(polygon);
    },
    drawRegion() {
        const self = this
        $.getJSON("./json/meishan.json", "", function(mapData) {
            const features = mapData.features;
            features.forEach((g, i) => {
                const properties = g.properties;
                const coordinates = g.geometry.coordinates
                self.drawPolygons(i, coordinates, properties)
            });
            const polygonsLayer = new maptalks.VectorLayer(
                "vector-polygon",
                self.polygons,
                {
                    enableAltitude: true
                }
            ).addTo(self.mapDom);
        })
    }
    
    </script>

###### 三、绘制区域名称

可参考[官方文档--Example--绘制文字图层（ Label ）](https://maptalks.org/examples/cn/geometry/label/)测试用例；

在文档中暂未找到可以直接绘制面是添加文字属性，此处则另起一个文字图层进行叠加，在原有 `drawPolygons()` 方法中增加 `maptalks.Label()`，相关代码 [github](https://github.com/dcbase/maptalksDemo/blob/master/3d01.html)

相关代码如下：

    <script>
    
    drawPolygons(idx, coordinates, properties) {
            const polygon = new maptalks.MultiPolygon(coordinates, {
              symbol: {
                lineWidth: 1,
                lineColor: edgeColor,
                polygonFill: polygonColors[0],
                polygonOpacity: 1,
                text:properties.name
              },
              properties: {
                altitude: altitude,
                id: properties.id,
                name: properties.name,
                index: idx,
                properties: properties
              }
            })
              .on("mouseenter", function(e) {
                e.target.updateSymbol({
                  polygonFill: polygonColors[1]
                });
              })
              .on("mouseout", function(e) {
                e.target.updateSymbol({
                  polygonFill: polygonColors[0]
                });
              })
              
            //此处为地区名称图层增加
            let areaName = properties.name //区域名称
            const label = new maptalks.Label(areaName,
                properties.center,
                {
                    'draggable' : true,
                    'textSymbol': {
                        'textFaceName' : 'monospace',
                        'textFill' : '#34495e',
                        'textSize' : 13,
                        'textWeight' : 'bold',
                        'textVerticalAlignment' : 'center'
                    },
                    zIndex:11  //设置图层层级，值越大则层级越高
                });
                
               
            this.polygons.push(polygon);
            this.polygons.push(label);
        },
   
    </script>
    
    
    
###### 总结  

2D基本地图区域块绘制完成,效果图如下：

![androidImage](./images/demo01.jpg)
