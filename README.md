# simulationTextarea
模拟输入框2种方式
## 1.div+css

### css
···
    /*placeholdeer*/
    textarea:empty:before{
        content: attr(placeholder);
        color:#bbb;
    }
    textarea:focus:before{
        content:none;
    }
    .textarea {

        width: 400px;

        min-height: 20px;

        max-height: 300px;

        _height: 120px;

        margin-left: auto;

        margin-right: auto;

        padding: 3px;

        outline: 0;

        border: 1px solid #a0b3d6;

        font-size: 12px;

        line-height: 24px;

        padding: 2px;

        word-wrap: break-word;

        overflow-x: hidden;

        overflow-y: auto;

        border-color: rgba(82,168,236,0.8);

        box-shadow: inset 0 1px 3px rgba(0,0,0,0.1),0 0 8px rgba(82,168,236,0.6);

    }
    ···
    ### html
    ···
    <div class="textarea" contenteditable="true"><br /></div>
    ···
    CSS代码中，因为IE6不支持min/max，所以做了hack，其他的也好理解。
    
## js+textarea

### css
···
    #textarea {

        display: block;

        margin: 0 auto;

        overflow: hidden;

        width: 550px;

        font-size: 14px;

        height: 18px;

        line-height: 24px;

        padding: 2px;

    }

    textarea {

        outline: 0 none;

        border-color: rgba(82,168,236,0.8);

        box-shadow: inset 0 1px 3px rgba(0,0,0,0.1),0 0 8px rgba(82,168,236,0.6);

    }
···
### js

/**
 * 文本框根据输入内容自适应高度
 * @param                {HTMLElement}        输入框元素
 * @param                {Number}                设置光标与输入框保持的距离(默认0)
 * @param                {Number}                设置最大高度(可选)
 */
 ···
var autoTextarea = function (elem, extra, maxHeight) {
        extra = extra || 0;
        var isFirefox = !!document.getBoxObjectFor || 'mozInnerScreenX' in window,
        isOpera = !!window.opera && !!window.opera.toString().indexOf('Opera'),
                addEvent = function (type, callback) {
                        elem.addEventListener ?
                                elem.addEventListener(type, callback, false) :
                                elem.attachEvent('on' + type, callback);
                },
                getStyle = elem.currentStyle ? function (name) {
                        var val = elem.currentStyle[name];
 
                        if (name === 'height' && val.search(/px/i) !== 1) {
                                var rect = elem.getBoundingClientRect();
                                return rect.bottom - rect.top -
                                        parseFloat(getStyle('paddingTop')) -
                                        parseFloat(getStyle('paddingBottom')) + 'px';        
                        };
 
                        return val;
                } : function (name) {
                                return getComputedStyle(elem, null)[name];
                },
                minHeight = parseFloat(getStyle('height'));
 
        elem.style.resize = 'none';
 
        var change = function () {
                var scrollTop, height,
                        padding = 0,
                        style = elem.style;
 
                if (elem._length === elem.value.length) return;
                elem._length = elem.value.length;
 
                if (!isFirefox && !isOpera) {
                        padding = parseInt(getStyle('paddingTop')) + parseInt(getStyle('paddingBottom'));
                };
                scrollTop = document.body.scrollTop || document.documentElement.scrollTop;
 
                elem.style.height = minHeight + 'px';
                if (elem.scrollHeight > minHeight) {
                        if (maxHeight && elem.scrollHeight > maxHeight) {
                                height = maxHeight - padding;
                                style.overflowY = 'auto';
                        } else {
                                height = elem.scrollHeight - padding;
                                style.overflowY = 'hidden';
                        };
                        style.height = height + extra + 'px';
                        scrollTop += parseInt(style.height) - elem.currHeight;
                        document.body.scrollTop = scrollTop;
                        document.documentElement.scrollTop = scrollTop;
                        elem.currHeight = parseInt(style.height);
                };
        };
 
        addEvent('propertychange', change);
        addEvent('input', change);
        addEvent('focus', change);
        change();
};
···
### html
···
<textarea
id="textarea"
placeholder="回复内容"></textarea>


    <script>
        var text=document.getElementById("textarea");

        autoTextarea(text);// 调用

    </script>
···
