# 修改自官方，
#npm install react-native-qiniu-sf --save  
# React Native Qiniu SDK
修改自官方，官方貌似好久没人维护了，自己修改了下。
增加了base64上传
上传npm多了个gz包，下次再删除。
估计好多人会对上传感到疑惑。下面是我的上传代码（smaple）


普通文件上传
```Conf.ACCESS_KEY = "xxx";
Conf.SECRET_KEY = "xxx";

var putPolicy = new Auth.PutPolicy2(
    {scope: "intlec"}
);
var uptoken = putPolicy.token();
let formInput = {
    key :moment().format('YYYY-MM-DD')+"/"+uuid.v4()+".jpg",//七牛没有文件夹，你可以按照这种形式来取文件名，uuid我个人比较喜欢的方式。
}

Rpc.uploadFile(yourLocalFile, uptoken, formInput).then((res)=>{
    if(res.status==200){
      this.setState({
          uri:"http://这里你绑定的域名/"+formInput.key
      });
    }
});
```
#=================================================
base64上传
```
Conf.ACCESS_KEY = "xxx";
Conf.SECRET_KEY = "xxx";

var putPolicy = new Auth.PutPolicy2(
    {scope: "intlec"}
);
var uptoken = putPolicy.token();
let formInput = {
    key :moment().format('YYYY-MM-DD')+"/"+uuid.v4()+".jpg",
}
Rpc.uploadBase64(yourBase64Ddata, uptoken, formInput.key).then((res)=>{
    console.log("数据反馈",res)
    if(res.status==200){
        this.setState({
            uri:"http://域名/"+formInput.key
        });
    }
});
```






####一下是官方的说明。
纯JavaScript实现的Qiniu SDK,

##安装
npm i react-native-qiniu-sf  --save

##使用方法

```javascript
import Qiniu,{Auth,ImgOps,Conf,Rs,Rpc} from 'react-native-qiniu';
Conf.ACCESS_KEY = <AK>
Conf.SECRET_KEY = <SK>
//强烈不建议在客户端保存 AK 和 SK ，反编译后 hacker 获取到可以对你的资源为所欲为，建议通过安全渠道从服务器端获取。

//upload file to Qiniu
var putPolicy = new Auth.PutPolicy2(
    {scope: "<Bucket>:<Key>"}
);
var uptoken = putPolicy.token();
let formInput = {
    key : "<Key>",
    // formInput对象如何配置请参考七牛官方文档“直传文件”一节
}
Rpc.uploadFile(<LOCAL_URL>, uptoken, formInput);

//download private file
var getPolicy = new Auth.GetPolicy();
let url = getPolicy.makeRequest('http://7xp19y.com2.z0.glb.qiniucdn.com/5.jpg');
//fetch from this url

//image sync operation
var imgInfo = new ImgOps.ImageView(1,100,200);
let url = imgInfo.makeRequest('http://7xoaqn.com2.z0.glb.qiniucdn.com/16704/6806d20a359f43c88f1cb3c59980e5ef');
//fetch from this url

//image info 
var self = this;
var imgInfo = new ImgOps.ImageInfo();
let url = imgInfo.makeRequest('http://7xoaqn.com2.z0.glb.qiniucdn.com/16704/6806d20a359f43c88f1cb3c59980e5ef');
fetch(url).then((response) => {
      return response.text();
    }).then((responseText) => {
      self.setState({info: responseText});
    }).catch((error) => {
      console.warn(error);
    });

//resource operation
//stat info
var self = this;
Rs.stat(<BUCKET>, <KEY)
        .then((response) => response.text())
        .then((responseText) => {
          self.setState({info: responseText});
        })
        .catch((error) => {
          console.warn(error);
        });
```

