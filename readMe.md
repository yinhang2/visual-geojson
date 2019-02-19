# visual_GeoJSON

该js用于展示对栅格数据和矢量数据的展示，依赖于leaflet和Jquery，内部封装了Schedular调度器对象，color，layer和db等函数用于对数据展示，其中部分函数并未暴露出用户接口。用户也可以根据暴露出的方法自行实现schedular对象。
## 数据格式要求
1. 使用该js库的数据要求为GeoJson格式数据
2. 使用该js库的动态展示要求初始化图层使用GeoJSon数据，后续数据格式为

    `{
		time: 121312,
		data: [
			2451.23, 1902.4,31,....	
		]
	}`
以time作为rowkey，进行列式存储
4. 如果调用Schedular的saveByServer从后台读取数据，对于后台无要求，要求数据格式以上述列子中对象存储的数组即可

## Map
在该js库中封装了依赖leaflet扩展以适应中国国内的天地图和高德地图，提供了init方法
### function init
#### param: lat,lng
参数为经纬度，用于向外提供高德地图的底图，缩放级别为8，考虑到用户会使用不同地图API构造地图，因此不同底图并不会影响js库的使用

##Layer
封装了矢量图层和栅格数据的构造。
### titleLayer
#### param： name, data
data为GeoJson的数据，name为需要展示不同类型的图层类型，目前已封装有点线面的图层，name分别为point, line和surface，返回值layer

### gridLayer
#### param： geojson ，startRGB， endRGB
用于根据geojson中的数据构造栅格数据图层，rgb分别代表最小值的颜色和最大值的颜色，返回值为layer

## schedular
### function Schedular
#### param:database,url,startTime,endTime,startRGB,endRGB, map, layer

该函数是调度器的构造函数，接受八个参数，database为前端IndexDB的存储名称，url为后台数据接口，startTime和endTime为期待动态展示时的开始数据key和结束数据key（在库中以time表示独一无二的key），设定颜色边界（startColor和 endColor），map可以是用户使用其他WebGis公有js库创建的底图图层，也可以使用该js库中自行封装的map方法构造。layer为需要动态展示的图层。
### function saveByServer
#### param time number
用于通过后台保存数据，time为需要取的第一个数据的key，number为一次读取数据的数目，数据会保存在indexDB中和内部维护的数据中
### function saveByOffline
#### param list time
用于保存在js中的数据，time为需要取的第一个数据的key，list为数据的数组，其形式为[a,b,c,...] (a,b,c为数据）支持批量录入

### function start
用于播放已缓存在本地的栅格数据，在例子中实现了一个简单的离线读取四个数据进行播放的例子




# visual_GeoJSON - English

The js document is used for the display of raster data and vector data. It depends on leaflet and Jquery. Internally encapsulates the Schedular scheduler object, functions such as color, layer, and db to display the data. Some functions do not expose the user interface. Users can also implement schedular objects themselves based on the exposed methods.
## Data format requirements
1. The data required to use the js library is GeoJson format data.
2. Dynamic display using the js library requires the initialization layer to use GeoJSon data, the subsequent data format is

    `{
		time: 121312,
		data: [
			2451.23, 1902.4,31,....	
		]
	}`
time rowkey，Columnar storage
4. If you call Schedule's saveByServer to read data from the background, there is no requirement for the background, you can request the data format to store the array in the above column.

## Map
In the js library, the dependency leaflet extension is encapsulated to adapt to the Chinese domestic sky map and the high German map, and the init method is provided.
### function init
#### param: lat,lng
The parameter is latitude and longitude, which is used to provide a base map of the high-definition map. The zoom level is 8. Considering that the user will construct the map using different map APIs, different base maps will not affect the use of the js library.

## Layer
Constructs a vector layer and raster data.
### titleLayer
#### param： name, data
Data is the data of GeoJson. Name is the type of layer that needs to display different types. Currently, the layer with a little line surface is encapsulated. The names are point, line and surface, and the return value is layer.

### gridLayer
#### param： geojson ，startRGB， endRGB
Used to construct a raster data layer based on the data in geojson. rgb represents the color of the minimum value and the maximum value respectively. The return value is layer.

## schedular
### function Schedular
#### param:database,url,startTime,endTime,startRGB,endRGB, map, layer

This function is the constructor of the scheduler, accepts eight parameters, database is the storage name of the front-end IndexDB, url is the background data interface, startTime and endTime are the start data key and the end data key when expecting dynamic display.（Representing a unique key in time in the library），Set color borders（startColor and endColor），The map can be a basemap layer created by the user using other WebGis public js libraries, or it can be constructed using the map method encapsulated in the js library. The layer is the layer that needs to be dynamically displayed.
### function saveByServer
#### param time number
Used to save data through the background, time is the key of the first data to be fetched, number is the number of data read at a time, the data will be stored in the indexDB and internally maintained data.
### function saveByOffline
#### param list time
For storing data in js, time is the key of the first data to be fetched, and list is an array of data in the form [a, b, c,...] (a, b, c are data) Support batch entry

### function start
Used to play raster data that has been cached locally. In the example, a simple example of offline reading four data for playback is implemented.

