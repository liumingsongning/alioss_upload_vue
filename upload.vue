<template lang="html">
  <div id="video_container">
    <div>
        <slot name='button'></slot>
        <slot name='progressBar'></slot>
    </div>
</div>
</template>
<script>
import plupload from "plupload";
export default {
  data() {
    return {
      uploader:{},
      g_object_name:'',
      url:''
    };
  },
  props: {
    updata: {
       type: String,
       required: true
    },
    // filename: {
    //    type: String,
    //    required: true
    // },
    browse_button: {
      type: String,
      required: true
    },
    // bucket_name: {
    //   type: String,
    //   required: true,
    // },
    // showpause: {
    //   type: Boolean,
    //   default: true
    // },
    // showcontinue: {
    //   type: Boolean,
    //   default: true
    // },
    // filters: {
    //   type: Object,
    //   default: function() {
    //     return {};
    //   }
    // }
  },
  mounted() {
     this.upload();
  },
  watch: {  
    'updata':function(val){  
      if(val){
        this.set_upload_param();
      }
    }
  },
  methods: {
    set_upload_param() {
      // this.ajax
      //   .get("/aliosstoken")

      //   .then(function(res) {

          var data=JSON.parse(this.updata);

          // if (g_object_name_type == 'local_name')
          // {
          this.g_object_name = "${filename}"
          // }
          // else if (g_object_name_type == 'random_name')
          // {
          //     suffix = get_suffix(filename)
          //     g_object_name = key + random_string(10) + suffix
          // }
          // return ''
       
          var new_multipart_params = {
            key: data.dir+this.g_object_name,
            policy: data.policy,
            OSSAccessKeyId: data.accessid,
            success_action_status: "200", //让服务端返回200,不然，默认会返回204
            signature: data.signature
          };
         
          this.uploader.setOption({
            url: data.host,
            multipart_params: new_multipart_params
          });

          this.url=data.host+'/'+data.dir;
       
          this.uploader.start();
        // })
        // .catch(function(err) {
        //   if (err.status_code == 404) {
        //     alert(err.message);
        //   }
        // });
    },
    upload() {
      var self=this;
     
      this.uploader = new plupload.Uploader({
        runtimes: "html5,flash,silverlight,html4",
        browse_button:self.browse_button,
        multi_selection: false,
        url: "http://oss.aliyuncs.com",

        // filters: {
        //   mime_types: [
        //     //只允许上传图片和zip,rar文件
        //     { title: "Image files", extensions: "jpg,gif,png,bmp" },
        //     { title: "Zip files", extensions: "zip,rar" }
        //   ],
        //   max_file_size: "10mb", //最大只能上传10mb的文件
        //   prevent_duplicates: true //不允许选取重复文件
        // },

        init: {
          PostInit: function() {
            // console.log(6757567)
            // // document.getElementById("ossfile").innerHTML = "";
            // document.getElementById("postfiles").onclick = function() {
              // self.set_upload_param(uploader, "", false);
            //   return false;
            // };
          },

          FilesAdded: function(up, files) {
            self.$emit("on-add", 1);
            // plupload.each(files, function(file) {
            //   // 文件添加进队列后，处理相关的事情
            //   console.log("FilesAdded");
            //   // _this.fileSize = _this.toDecimal(file.size);
            //   console.log(file.type);
            //   return false;
            // });
            //  self.set_upload_param(uploader, "", false);
          },

          BeforeUpload: function(up, file) {
            console.log('BeforeUpload');
            // self.set_upload_param(up,'', true);
          },

          UploadProgress: function(up, file) {
            console.log('UploadProgress')
            self.$emit("on-progress", file.percent);
          },

          FileUploaded: function(up, file, info) {
            console.log('FileUploaded')
            // var domain = up.getOption("domain");
              var tmp_name = self.g_object_name
              console.log(tmp_name)
              tmp_name = tmp_name.replace("${filename}",file.name);
              self.$emit("on-uploaded", self.url+tmp_name)
          
            if (info.status == 200) {
              // document
              //   .getElementById(file.id)
              //   .getElementsByTagName("b")[0].innerHTML =
              //   "upload to oss success, object name:" +
              //   get_uploaded_object_name(file.name);
            } else {
              // document
              //   .getElementById(file.id)
              //   .getElementsByTagName("b")[0].innerHTML =
              //   info.response;
            }
          },

          Error: function(up, err) {
            
            if (err.code == -600) {
              // document
              //   .getElementById("console")
              //   .appendChild(
              //     document.createTextNode(
              //       "\n选择的文件太大了,可以根据应用情况，在upload.js 设置一下上传的最大大小"
              //     )
              //   );
            } else if (err.code == -601) {
              // document
              //   .getElementById("console")
              //   .appendChild(
              //     document.createTextNode(
              //       "\n选择的文件后缀不对,可以根据应用情况，在upload.js进行设置可允许的上传文件类型"
              //     )
              //   );
            } else if (err.code == -602) {
              // document
              //   .getElementById("console")
              //   .appendChild(
              //     document.createTextNode("\n这个文件已经上传过一遍了")
              //   );
            } else {
              // document
              //   .getElementById("console")
              //   .appendChild(
              //     document.createTextNode("\nError xml:" + err.response)
              //   );
            }
          }
        }
      });
      this.uploader.init();
    }
  }
};
</script>

  
