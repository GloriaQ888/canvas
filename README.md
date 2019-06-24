<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <style type="text/css">
        body{
             background:#f9f9fb;
             overflow:hidden;
        }
            *{
                margin:0;
                padding:0px;
               
            }
            .AI_content{
                width:1120px;
                margin:0 auto;
            }
            .AI_content .AI_title{
                width:1120px;
                height:150px;
                line-height:150px;
                text-align:center;
                font-size:30px;
                font-weight:bold;
            }
            .ai_template{
                width:1120px;
                padding: 30px;
                color:#999999;
                font-size:12px;
                background:#fff;
                position: relative;
            }
            .item_tip{
                    background: #fff;
            }
            .img_tip1{
                width: 47px;
                height: 18px;
                background: url(all_icon.png) no-repeat -28px -5px;
                display: inline-block;
                vertical-align:bottom;
            }
           .emotion{
                margin-left: 50px;
           }
           .like_tip{
                color:#f7585d;
                margin-right:20px;
          }
            .AI_content .img_tip2{
                background:url(all_icon.png) no-repeat -8px -9px;
                width: 12px;
                height: 16px;
                display: inline-block;
                vertical-align: middle;
            }
            .AI_content .img_tip3{
                background:url(all_icon.png) no-repeat -92px -9px;
                width: 12px;
                height: 16px;
                display: inline-block;
                vertical-align: middle;
            }
            .unlike_tip{
                 margin-right:40px;
            }
            .canvasBox{
                 height:550px;
                 position: relative;
            }
            #canvas{
                width:1320px;
                height:700px;
                position: absolute;
                left:-120px;
                top:-90px;
            }
            #newCanvas{
                width:1320px;
                height:700px;
                position: absolute;
                left:-120px;
                top:-90px;
            }
            #newCanvas.cur{
                cursor:pointer;

            }
            .total_commit{
                width:100px;
                height:135px;
                background:#3d74ff;
                color:#fff;
                position: absolute;
                top: 0px;
                right: 40px;
                text-align: center;
            }
            .total_commit .total_score{
                font-size:30px;
                margin-top:50px;
            }
            .total_commit p{
                 font-size:16px;
            }
            .monte_tab{
                overflow:hidden;
                border-top:1px solid #f8f8f8;
                height: 40px;
            }
            .monte_tab li{
                list-style:none;
                float:left;
                width: 56px;
                margin-right: 40px;
                font-size:13px;
                height: 40px;
            }
            .monte_tab ul{
                width:1260px;
                position: absolute;
            }
            .monte_tab li a{
                height: 20px;
                display: inline-block;
                padding-top: 20px;
                cursor:default;
            }
            .monte_tab li a.cur{
                border-top: 4px solid #3d74ff;
            }
        </style>
    </head>
    <body>
        <div class="AI_content">
            <div class="AI_title">AI声音</div>
            <div class="ai_template">
                <div class="item_tip">
                    <span>声音指数：</span>
                    <span>低</span>
                    <span class="img_tip1"></span>
                    <span>高</span>
                    <span class="emotion">用户情感指数：</span>
                    <span class="img_tip2"></span>
                    <span class="like_tip">喜欢（0%）</span>
                    <span class="img_tip3"></span>
                    <span class="unlike_tip">不喜欢（0%）</span>
                    <span>AI分析样本数：</span>
                    <span class="AINum">15851255</span>
                </div>
                <div class="total_commit">
                    <div class="total_score">8.82</div>
                    <p>综合得分</p>
                </div>
                <div class="canvasBox">
                    <canvas id="canvas" width="1320" height="700"></canvas>
                    <canvas id="newCanvas" width="1320" height="700"></canvas>
                </div>
                
                <div class="monte_tab">
                    <ul>
                        
                    </ul>
                </div>
            </div>
            
        </div>
    </body>
    <script src="jquery.js"></script>
    <script src="canvas.js"></script>
    <script src="data.js"></script>
    <script type="text/javascript">
    console.log(window.data)
    var time = new Date();
    var y= time.getFullYear();
    var m=time.getMonth()+1;
    var monthHtml = "";
    var glodMonth = y +"-"+(m<10?"0"+m:m)+"-"+time.getDate();
    var nowY = y,nowM = m<10?"0"+m:m;
    for (var i=0;i<12;i++){
        y = m-1<0?y-1:y;
        m = m-1<0?12:m;
        if(m<10){m = "0"+m;}
        monthHtml += "<li><a>"+y+"-"+m+"</a></li>"
        $(".monte_tab ul").html(monthHtml);
        m--;
    }
    $(".monte_tab ul li").first().children().addClass("cur");
       var _index = {
         init:function (){
            this.circleArr = [];
            this.data = [];
            this.canvas = document.querySelector("#canvas");
            this.content = this.canvas.getContext("2d");
            this.newCanvas = document.querySelector("#newCanvas");
            this.newContent = this.newCanvas.getContext("2d");
            this.coordinateArr = [
                [[215,470,90],[637,208,90],[1107,449,90],[300,194,65],[849,373,65],[630,480,65],[431,365,50],[991,208,50]],
                [[305,436,90],[801,435,90],[1007,210,90],[244,194,65],[507,228,65],[749,173,65],[550,450,50],[1100,448,50]],
                [[244,264,90],[1087,410,90],[650,250,90],[550,515,65],[941,188,65],[801,480,65],[305,486,50],[449,173,50]],
                [[649,213,90],[224,264,90],[961,218,90],[450,400,65],[1107,460,65],[680,490,65],[250,526,50],[891,520,50]],
                [[640,213,90],[444,464,90],[1061,460,90],[320,200,65],[1027,190,65],[740,490,65],[200,446,50],[861,290,50]],
                [[680,203,90],[344,453,90],[1001,200,90],[430,190,65],[1097,440,65],[640,450,65],[200,286,50],[861,520,50]]
            ];
            this.txtArr= [];
            this.smallTxtArr=[];
            this.index = "";
            this.selectMonth = glodMonth;
            this.monthTab();
            this.getData();
            this.moveOn();
            this.smallcircleCoordinatArr = [];
         },

         //点击切换月份
         monthTab:function(){
            var that = this;
            $(".monte_tab li").click(function(event) {
                that.newContent.clearRect(0,0,that.newCanvas.width,that.newCanvas.height);
                if(that.circleArr.length){
                    if(that.circleArr[that.circleArr.length-1].stop){
                        if(that.smallcircleCoordinatArr.length){
                            for(var i=0;i<that.smallcircleCoordinatArr.length;i++){
                                that.smallcircleCoordinatArr[i].stop = true;
                                that.newContent.clearRect(0,0,that.newCanvas.width,that.newCanvas.height);
                            }
                        }
                       if($(this).text() == nowY+"-"+nowM){
                            that.selectMonth = $(this).text() +"-"+time.getDate();

                       }else{
                            that.selectMonth = getMonthLastDay($(this).text().slice(0, 4),$(this).text().slice(5, $(this).text().length));
                       }
                        that.content.clearRect(0, 0,that.canvas.width,that.canvas.height);
                        $(this).children("a").addClass('cur');
                        $(this).siblings().children("a").removeClass('cur');
                        that.circleArr = [];
                        that.txtArr = [];
                        that.smallcircleCoordinatArr = [];
                        // that.postData(function(res){
                        //     
                        // });
                        that.getData();

                    }
                }
               
            });
            // $(".monte_tab li").hover(function(){
            //     if(that.smallcircleCoordinatArr.length){
            //         for(var i=0;i<that.smallcircleCoordinatArr.length;i++){
            //             that.smallcircleCoordinatArr[i].stop = true;
            //             that.newContent.clearRect(0,0,that.newCanvas.width,that.newCanvas.height);
            //         }
            //         that.newContent.clearRect(0,0,that.newCanvas.width,that.newCanvas.height);
            //         that.smallcircleCoordinatArr = [];
            //         flag=true;
            //     }
            // })
         },
         
          //处理后台数据
         getData:function(){
            //将数据进行对比归类，得出每一项应该展示的圆环半径大小，把对应的项目名称和所占百分比存入数组
            var that = this;
            var labels = window.data.labels;
            that.data = window.data;
            $(".like_tip").text("喜欢（"+that.data.productGoodRate+")")
            $(".unlike_tip").text("不喜欢（"+that.data.productBadRate+")")
            $(".AINum").text(that.data.sampleNum);
            $(".total_score").text(that.data.productAllScore);
            for(var i=0;i<labels.length;i++){
                that.txtArr.push([labels[i].name,labels[i].labelSoundRate])
            }
            if(that.txtArr.length == labels.length){
                that.createCricle();
            }
            // that.selectMonth =  glodMonth;
            // var _url = "data.json";//that.selectMonth为用户当前点击选择的月份
            // $.ajax({
            //     url: _url,
            //     type: 'get',
            //     dataType: 'json',
            //     success:function(res){
            //         if(res.code ==200){
            //             var labels = res.data.labels;
            //             that.data = res.data;
            //             $(".like_tip").text("喜欢（"+that.data.productGoodRate+")")
            //             $(".unlike_tip").text("不喜欢（"+that.data.productBadRate+")")
            //             $(".AINum").text(that.data.sampleNum);
            //             $(".total_score").text(that.data.productAllScore);
            //             for(var i=0;i<labels.length;i++){
            //                 that.txtArr.push([labels[i].name,labels[i].labelSoundRate])
            //             }
            //             if(that.txtArr.length == labels.length){
            //                 that.createCricle();
            //             }
            //         }else{
            //             console.log(res.message)
            //         }
            //     },
            //     error:function(){
            //         console.log('请求失败');
            //     }
            // })
            
         },
         postData:function(callback){
            var that = this;
            var _url = "data.json";
            $.ajax({
                url: _url,
                type: 'post',
                dataType: 'json',
                data: {time: that.selectMonth},
                success:function(res){
                    callback && callback(res);
                },
                error: function(err){
                    callback && callback();
                }
            })
            
            
         },
         //生成圆环事件
         createCricle:function(){
            var that = this;
            that.index=parseInt(getRandom(0,5));
            console.log(that.index)
            for(var i = 0;i<that.data.labels.length;i++){
                var circle = new Circle({
                    dom:that.canvas,
                    x:that.coordinateArr[that.index][i][0],
                    y:that.coordinateArr[that.index][i][1],
                    data:that.txtArr[i][1],
                    radius:that.coordinateArr[that.index][i][2],
                    color:"",
                    bgColor:"#eaeaee",
                    txt:that.txtArr[i][0],
                    font:"15px 微软雅黑",
                    perFont:"11px 微软雅黑",
                    fontColor:"black",
                    isBigCircle:true
                })
                circle.ani();
                that.circleArr.push(circle);

          }
         },
         //canvas鼠标事件
        moveOn:function(){
            var that = this;
            var x=0,y=0;
           
            that.newCanvas.addEventListener('mousemove', function(e){
                 x = e.offsetX;
                 y = e.offsetY;
                 on()
            });
            //大圆小圆跳转事件
            that.newCanvas.addEventListener('click', function(e){
                var clickX = e.offsetX;
                var clickY = e.offsetY;
                if(that.circleArr.length){
                    for(var i = 0;i<that.circleArr.length;i++){
                        var distance = Math.sqrt(Math.pow((that.circleArr[i].x-clickX),2)+Math.pow((that.circleArr[i].y-clickY),2));
                        if(distance<=that.circleArr[i].radius){
                            if(!that.data.labels[i].hasOwnProperty('groupLabelList')){
                                   location.href = that.data.labels[i].url;
                            }
                        }
                    }
                }
                if(that.smallcircleCoordinatArr.length){
                    for(var i = 0;i<that.smallcircleCoordinatArr.length;i++){
                        var distance = Math.sqrt(Math.pow((that.smallcircleCoordinatArr[i].x-clickX),2)+Math.pow((that.smallcircleCoordinatArr[i].y-clickY),2));
                        if(distance<=that.smallcircleCoordinatArr[i].radius){
                               location.href = that.smallTxtArr[i].url;
                        }
                    }
                }

            });
            //鼠标移动伸出小圆事件
            var cIndex=null,flag=false;
            function on(){
                var glodIndex=undefined ;
                var r = 35,counter =0,isMove = false;
                var aa=0 ,oldDistance = 0,oldIndex=cIndex;
                if(that.circleArr.length){
                   for(var i = 0;i<that.circleArr.length;i++){
                        var distance = Math.sqrt(Math.pow((that.circleArr[i].x-x),2)+Math.pow((that.circleArr[i].y-y),2));
                        if(distance<=that.circleArr[i].radius){
                            glodIndex = i;
                            if(that.data.labels[i].groupLabelList){
                                if(that.circleArr[i].radius == 90&&that.data.labels[i].groupLabelList.length>=8){
                                    that.smallTxtArr = that.data.labels[i].groupLabelList;
                                    that.smallTxtArr.splice(8,that.smallTxtArr.length-8);
                                }else{
                                    that.smallTxtArr = that.data.labels[i].groupLabelList;
                                }
                                if(that.circleArr[i].radius < 90&&that.data.labels[i].groupLabelList.length>6){
                                    that.smallTxtArr = that.data.labels[i].groupLabelList;
                                    that.smallTxtArr.splice(6,that.smallTxtArr.length-6);
                                }else{
                                    that.smallTxtArr = that.data.labels[i].groupLabelList;

                                }
                                counter =that.smallTxtArr.length
                            }
                            oldDistance = Math.sqrt(Math.pow((that.circleArr[glodIndex].x-x),2)+Math.pow((that.circleArr[glodIndex].y-y),2))
                            aa++;
                            $("#newCanvas").addClass('cur');
                            if(cIndex!=i){
                                // that.newContent.clearRect(0,0,that.newCanvas.width,that.newCanvas.height);
                                flag=true;
                            }else{
                                flag=false;
                            }
                            cIndex=i;
                        }
                   }
                   if(cIndex!=oldIndex ){
                      for(var i=0;i<that.smallcircleCoordinatArr.length;i++){
                            that.smallcircleCoordinatArr[i].stop = true;
                        }
                      setTimeout(function(){
                            that.newContent.clearRect(0,0,that.newCanvas.width,that.newCanvas.height);

                      },10)
                         
                   }
                   if(cIndex!=null){
                        if(oldDistance-120<=(that.circleArr[cIndex].radius+r)*2){
                            aa++;
                            $("#newCanvas").addClass('cur');
                        }

                        if(Math.sqrt(Math.pow((that.circleArr[cIndex].x-x),2)+Math.pow((that.circleArr[cIndex].y-y),2))>(that.circleArr[cIndex].radius+r)*2){
                            aa=0;

                        }
                         glodIndex=cIndex;
                    }
                   if(aa==0){ 
                        cIndex=null;
                        counter = 0;
                        $("#newCanvas").removeClass('cur');
                        for(var i=0;i<that.smallcircleCoordinatArr.length;i++){
                            that.smallcircleCoordinatArr[i].stop = true;
                        }
                         that.newContent.clearRect(0,0,that.newCanvas.width,that.newCanvas.height)
                   }else{
                        
                       }
                    var newX = 0;
                    var newY =0;
                    if(glodIndex!=undefined&& flag){
                        oldIndex = glodIndex;
                        for(var i=0;i<counter;i++){
                            newX = that.circleArr[glodIndex].x+Math.cos((2*Math.PI/counter)*(i+1))*(that.circleArr[glodIndex].radius+r+35);
                            newY = that.circleArr[glodIndex].y+Math.sin((2*Math.PI/counter)*(i+1))*(that.circleArr[glodIndex].radius+r+35);
                            var smallcircle = new Circle({
                                dom:this.newCanvas,
                                x:newX,
                                y:newY,
                                data:that.smallTxtArr[i].labelSoundRate,
                                radius:r,
                                color:"#f03f60",
                                bgColor:"#fff",
                                txt:that.smallTxtArr[i].name,
                                font:"11px 微软雅黑",
                                perFont:" 7px 微软雅黑",
                                fontColor:"rgb(240,63,94)",
                                isBigCircle:false

                            })
                            smallcircle.ani()
                            that.smallcircleCoordinatArr.push(smallcircle)
                        
                        }
                    }
                    if(that.smallcircleCoordinatArr.length){//判断是否点击中小圆
                        for(var k = 0;k<that.smallcircleCoordinatArr.length;k++){
                            var distance = Math.sqrt(Math.pow((that.smallcircleCoordinatArr[k].x-x),2)+Math.pow((that.smallcircleCoordinatArr[k].y-y),2));
                            if(distance<=that.smallcircleCoordinatArr[k].radius){

                            }
                        }
                    }
                    //得到鼠标所在的圆环的坐标，去确定周边生成的小圆的坐标,大圆与小圆之间相距为大圆半径+小圆半径+100
                    //假设有伸出4个小圆
                    

                }
            }
            
   }
}
       function getRandom(min,max){
            return Math.random()*(max-min)+min;
       }
       function  getMonthLastDay(year,month) {
            var result;
            var lastDay= new Date(year,month,0);
            var year ;
            var month ;
            var day ;
            if(lastDay.getMonth() == new Date().getMonth()){
                lastDay= new Date();
                year = lastDay.getFullYear();
                month = lastDay.getDay();
                day = lastDay.getMonth()+1;
                // currentDate.getFullYear()+"-"+(currentDate.getMonth()+1)+"-"+currentDate.getDay();
            }else{
                year = lastDay.getFullYear();
                month = lastDay.getMonth() + 1;
                day = lastDay.getDate();
            }
            day = day < 10 ? '0'+day : day;
            month = month < 10 ? '0'+ month : month;
            result = year+'-'+month+'-'+day;
            return result
        }
       $(function(){
            _index.init();
       })
        
    </script>
</html>
