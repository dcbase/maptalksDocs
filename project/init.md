
###### 一、地图初始化绘制

可参考[官方文档 Examples](https://maptalks.org/examples/en/map/load/)第一个测试用例

[demo01]

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









