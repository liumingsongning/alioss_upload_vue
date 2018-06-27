# alioss_upload_vue
一个阿里云上传用于vue的包
========================
1，在项目中执行以下命令
```
npm install alioss_upload_vue

```
2,示例
```
 <upload 
      :updata='updata'   
      browse_button='pickfiles' //指定上传按钮id，随便定义
      @on-add="UploadAdd"
      @on-uploaded='Uploaded' //这是子组件传回的上传完成后完整文件路径，Uploaded是你自己需要定义的，必须在methods中定义此方法
      @on-progress='Progress' //返回上传进度
      > 
      <Button type="ghost" id="pickfiles" slot='button'>选择文件</Button> //随便定义一个插槽，slot='button'必须写上，id与上面的browse_button一致
      <Progress slot='progressBar'></Progress>//随便定义一个插槽，slot='progressBar'必须写上，显示进度用，子组件传回的进度参数在 @on-progress="Progress"中获得
  </upload>
  //js部分
  //导入
  import Upload from "alioss_upload_vue";
  
  data () {
      return {
          updata:'',
      }
  },
   methods: {
      UploadAdd() {
        var self = this;
        this.ajax                        //在此方法中调用后台数据
          .get("/aliosstoken")
          .then(function(res) {
            self.updata = res.data.data;
          })
          .catch(function(err) {

          });
      },
      Uploaded(val){
        console.log(val);
      },
      Progress(val){
         console.log(val);
      }
    }
  },
  ```
  
  

