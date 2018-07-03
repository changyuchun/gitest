
 ## 吉亚呼叫中心软电话条嵌入指南

+ #### 环境说明
> 线上环境callcenter.58.com.cn
>
> 沙箱环境 10.9.194.12	callcenter.58.com.cn



+ #### 	确保业务系统用户都已经在呼叫中心分配分机号，如果没有，请联系：张士超，孙楠，康伟，凌禹

+ #### 将下面电话条html嵌入页面，cc_bspid的value填写用户在bsp系统的ID
 ```html
    <script src="http://callcenter.58.com.cn/phonebar/jquery-1.8.2.min.js"></script>
    <script>
    	
    var gcallid = null;
    var handlers = {
        //电话条呼出按钮激发事件
        onbeforecall: function(phone) {
            handlers.callphone(phone, {});
        },
        //挂断电话,请勿重写，直接调用  callid:呼入时为onring里的callid 呼出时onconnected里的callid的前10位
        hangcall:function(callid){},
        ///自动转接,客户接听成功为转接成功，否则回到原通话,请勿重写，直接调用
        autotransfer:function(callid,destphone){},
        //盲转接 直接转接出去 当前通话挂断,请勿重写，直接调用 此callid是onconnected里的callid的前10位
        blindtransfer: function(callid, destphone) {},
        //转接 需要在电话条上手动点击转接按钮完成转接操作,此方法会显示转接界面如无需界面可调用auto,请勿重写，直接调用 此callid是onconnected里的callid的前10位
        transfercall: function(callid, destphone) {},
        //摘机,请勿重写，直接调用 此callid只能是onring里的callid
        pickupcall: function(callid) {},
        //一键外呼,请勿重写，直接调用
        callphone: function(phone, businessdata) {},
        //外呼进来 开始响铃时，如需，重写业务逻辑
        'onring': function(phone, callid) {
            gcallid = callid;
            console && console.log("onring");
            console && console.log(phone);
            console && console.log(callid);
        },
        //通话成功挂断时，如需，重写业务逻辑
        'on_success_hangup': function(startdatetime, enddatetime, phone, timelong, callid, direction) {
            console && console.log("on_success_hangup");
            console && console.log(startdatetime + " " + enddatetime + " " + phone + " " + timelong + " " + callid + " " + direction);
        },
        //电话条非成功挂断时，如需，重写业务逻辑
        'on_fail_hangup': function(startdatetime, enddatetime, phone, timelong, callid, direction) {
            console && console.log("on_success_hangup");
            console && console.log(startdatetime + " " + enddatetime + " " + phone + " " + timelong + " " + callid + " " + direction);
            talkingEvent && talkingEvent(phone);
        },
        //外呼开始时，如需，重写业务逻辑
        'oncalldata': function(startdatetime, extnum, callid) {
            gcallid = callid ? callid.substr(0, 10) : "";
            console && console.log("oncalldata");
            console && console.log(startdatetime + " " + extnum + " " + callid);
        },
        //坐席接通时，或外呼接通时，如需，重写业务逻辑
        'onconnected': function(startdatetime, extnum, callid) {
            console && console.log("onconnected");
            console && console.log(startdatetime + " " + extnum + " " + callid);
        },
        //登录完成，如需，重写业务逻辑
        'onlogined': function(extnum) {
            console && console.log("onlogined");
            console && console.log(extnum);
        },
        //电话条html渲染完成，如需，重写业务逻辑
        'on_phonebar_inited': function() {},
        //是否可以使用以下 get或set status方法
        can_get_set_status: false,
        //设置电话条状态,切勿实现此方法，直接调用，can_get_set_status 为 true时可以使用此方法
        'setStatus': function(state) {},
        //获取电话条状态,切勿实现此方法，直接调用，can_get_set_status 为 true时可以使用此方法
        'getStatus': function() {},
        'retryCount': 10,
        'ensureCallcenterLoaded': function() {
            if (handlers.can_get_set_status) {
                console.log("callcenter加载完成");
            } else {
                if (--handlers.retryCount > 0) {
                    window.setTimeout(handlers.ensureCallcenterLoaded, 1000);
                }
            }
        }
    };
    handlers.ensureCallcenterLoaded();
    </script>
    <div id="callcenterdiv" style="position: relative;z-index: 30;height: 40px;text-align: center;">
        <input type="button" value="自动转接" onclick="handlers.autotransfer(gcallid,'15810249367')">
        <input type="button" value="盲转接" onclick="handlers.blindtransfer(gcallid,'15810249367')"> 
        <input type="button" value="接听" onclick="handlers.pickupcall(gcallid)">
        <input type="button" value="挂断" onclick="handlers.hangcall(gcallid)">
        <input type="hidden" id="cc_bspid" value="2015041417260621cfbe37">
        <!-- 当前接入电话条的系统名称 找呼叫中心人员沟通协定 可以为空-->
        <input type="hidden" id="systemtype" value="">
        <!--技能组，多个技能组中间使用“;”隔开 可以为空-->
        <input type="hidden" id="groupname" value="">
        <!-- “1” 代表话后评价 可以为空-->
        <input type="hidden" id="configstr" value="">
        <!--代表技能，如：english|100;chinese|20 ，每个技能用”;”隔开，“|“前面是技能”面是技能值，其值大，优先转接 可以为空-->
        <input type="hidden" id="skills" value="">
        <script src="http://callcenter.58.com.cn/phonebar/customer_service_phone.js"></script>
        <div id="cc_phone" style="margin-left: 0px;"></div>
    </div>

```

+ #### 测试电话条页面
> [点击打开电话条测试页面](http://callcenter.58.com.cn/pb.html)
