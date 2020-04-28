maptalks提供了简单的信息提示框，也可以自定义提示框样式、内容，可参考官网

###### 一、[图形信息框](https://maptalks.org/examples/cn/ui-control/ui-geo-infownd/#ui-control_ui-geo-infownd) 

    <!DOCTYPE html>
    <html>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1">
      <title>空间与UI组件 - 图形信息框</title>
      <style type="text/css">
        html,body{margin:0px;height:100%;width:100%}
        .container{width:100%;height:100%}
      </style>
      <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/maptalks/dist/maptalks.css">
      <script type="text/javascript" src="https://cdn.jsdelivr.net/npm/maptalks/dist/maptalks.min.js"></script>
      <body>
    
        <div id="map" class="container"></div>
        <script>
    
          var map = new maptalks.Map('map', {
            center: [-0.113049,51.49856],
            zoom: 14,
            baseLayer: new maptalks.TileLayer('base', {
              urlTemplate: 'https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}.png',
              subdomains: ['a','b','c','d'],
              attribution: '&copy; <a href="http://osm.org">OpenStreetMap</a> contributors, &copy; <a href="https://carto.com/">CARTO</a>'
            })
          });
    
          var layer = new maptalks.VectorLayer('vector').addTo(map);
          var marker = new maptalks.Marker([-0.113049,51.49856]).addTo(layer);
    
          marker.setInfoWindow({
            'title'     : 'Marker\'s InfoWindow',
            'content'   : 'Click on marker to open.'
    
            // 'autoPan': true,
            // 'width': 300,
            // 'minHeight': 120,
            // 'custom': false,
            //'autoOpenOn' : 'click',  //set to null if not to open when clicking on marker
            //'autoCloseOn' : 'click'
          });
    
          marker.openInfoWindow();
    
        </script>
      </body>
    </html>
    

###### 二、[自定义信息框](https://maptalks.org/examples/cn/ui-control/ui-custom-infownd/#ui-control_ui-custom-infownd)

    <!DOCTYPE html>
    <html>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1">
      <title>空间与UI组件 - 自定义信息框</title>
      <style type="text/css">
        html,body{margin:0px;height:100%;width:100%}
        .container{width:100%;height:100%}
        .content{color:#fff;width:190px;height:128px;background-color:#051127;border:1px solid #0c2c45}
        .pop_title{float:left;padding-left:10px;width:180px;height:36px;line-height:36px;background:url(title.png);font-weight:bold;font-size:16px}
        .pop_time{float:left;width:183px;height:30px;margin:0 10px;line-height:36px}
        .pop_dept{float:left;padding:5px;max-width:65px;line-height:15px;text-align:center;border:1px solid #192b41;margin:0 10px}
        .pop_arrow{float:left;width:15px;height:24px;line-height:24px;background:url(arrow.png) no-repeat center center}
        .arrow{display:block;width:17px;height:10px;background:url(em.png) no-repeat;position:absolute;left:50%;margin-left:-5px;bottom:-10px}
      </style>
      <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/maptalks/dist/maptalks.css">
      <script type="text/javascript" src="https://cdn.jsdelivr.net/npm/maptalks/dist/maptalks.min.js"></script>
      <body>
    
        <div id="map" class="container"></div>
        <script>
          var map = new maptalks.Map('map', {
            center: [-0.113049,51.49856],
            zoom: 14,
            baseLayer: new maptalks.TileLayer('base', {
              urlTemplate: 'https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}.png',
              subdomains: ['a','b','c','d'],
              attribution: '&copy; <a href="http://osm.org">OpenStreetMap</a> contributors, &copy; <a href="https://carto.com/">CARTO</a>'
            })
          });
    
          var coordinate = map.getCenter().toFixed(3);
    
          var options = {
            //'autoOpenOn' : 'click',  //set to null if not to open window when clicking on map
            'single' : false,
            'width'  : 183,
            'height' : 105,
            'custom' : true,
            'dx' : -3,
            'dy' : -12,
            'content'   : '<div class="content">' +
              '<div class="pop_title">Custom InfoWindow</div>' +
              '<div class="pop_time">' + new Date().toLocaleTimeString() + '</div><br>' +
              '<div class="pop_dept">' + coordinate.x + '</div>' +
              '<div class="pop_dept">' + coordinate.y + '</div>' +
              '<div class="arrow"></div>' +
              '</div>'
          };
          var infoWindow = new maptalks.ui.InfoWindow(options);
          infoWindow.addTo(map).show(coordinate);
    
        </script>
      </body>
    </html>
    
    
上述是官网提供的两种信息框形式，接下来则是加入到标点点击事件中，结合上一章[点聚合效果](project/markercluster)，在第二部分标点，绘制标点时增加点击事件；

###### 1、点击事件

    // mark点击弹框
    function onClick(e) {
        e.target.setInfoWindow({
            'title'     : 'Marker\'s InfoWindow',
            'content'   : 'Click on marker to open.',
            'dy': 5,
            'autoPan': true,
            'custom': false,
            'autoOpenOn': 'click',  //set to null if not to open when clicking on marker
            'autoCloseOn': 'click'
        })
    }

###### 2、绘制标点时将事件加入

    let markers = []
    for (let i = 0; i < addressPoints.length; i++) {
        let a = addressPoints[i]
        markers.push(new maptalks.Marker([a[0], a[1]], {
            'symbol': [
                {
                    'markerType': 'ellipse',
                    'markerWidth': 20,
                    'markerHeight': 20,
                    'markerFill': 'rgb(64, 158, 255)',
                    'markerFillOpacity': 0.7,
                    'markerLineColor': '#73b8ff',
                    'markerLineWidth': 3
                },
                {
                    'markerType': 'ellipse',
                    'markerWidth': 10,
                    'markerHeight': 10,
                    'markerFill': '#006ddd',
                    'markerFillOpacity': 0.7,
                    'markerLineWidth': 0
                }
            ]
        }).on('mousedown', onClick));
        //此处则是加入点击事件
    }

整体效果图如下：
![androidImage](./images/tip.jpg) 
