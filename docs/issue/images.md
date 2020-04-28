


###### 1、加载网络图片问题：（可能会遇到跨域问题，阻止图片加载）

    new ImageLayer(id, images, options)

    new ImageLayer("images", [{
            url : 'http://example.com/foo.png',
            extent: [xmin, ymin, xmax, ymax],//地理范围
            opacity : 1
        }])
        
        
    //在第三个参数值中配置 crossOrigin 
    
    new ImageLayer("images", [{
                url : 'http://example.com/foo.png',
                extent: [xmin, ymin, xmax, ymax],
                opacity : 1
            }],{
                crossOrigin:'anonymous' 
            })
    

###### 2、extent参数取值问题


调用Geometry.getExtent()即可获取地理范围

杭州市栗子步骤：

  2.1  生成杭州市区域范围边界geojson，如何获取城市geojson，可打开网址（http://datav.aliyun.com/tools/atlas/#&lat=31.769817845138945&lng=104.29901249999999&zoom=4）选取所需城市；
  
  
  2.2 获取到的geojson可能是市级区域边界，需获取杭州市边界，通过网址（https://mapshaper.org/）导入步骤一生成的城市geojson, 合并边界可参考章节二--项目--[3D效果](project/3d) 底部描述方式；
  
  2.3 GeoJSON转化为Geometry(https://maptalks.org/examples/cn/json/geojson-to-geometry/#json_geojson-to-geometry), 再调用方法 `.getExtent()` 即可获取到杭州市地理范围；
    


