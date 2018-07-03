# wap-ceiling

## 移动端吸顶方案 ```position: fixed```

通过 **position: fixed** 属性，移动端吸顶，但兼容性差

`提示`：对兼容性考虑，采用css3属性 ```position: sticky```

------
在不支持position: sticky 属性的安卓系统和极少部分IOS系统移动端中，使用js去计算页面滑动的距离并复制给需要固定元素的top即可。
```javascript
/**
 * @file fixBar.js
 * @author liuchang33
 * @description: need Zepto and Hammer
 **/
var $ = window.Zepto;
var hammer = window.Hammer;
var $root = $('.fixbar-container');
var fixBar = function (option) {
    this.option = option || {};
    this.init();
};
fixBar.prototype = {
    init: function () {
        this.bindEvent();
    },
    bindEvent: function () {
        var ua = navigator.userAgent;
        var isANDROID = ua.match(/(Android);?[\s\/]+([\d.]+)?/);
        var isSupportSticky = this.isSupportSticky();
        // ANDROID
        if (isANDROID || !isSupportSticky) {
            var el = $root[0];
            if (!el) {
                return;
            }
            var rect = el.getBoundingClientRect();
            $root.data({
                top: $root.offset().top
            });
            var $holder = $('<div></div>').addClass('progress-holder')
                .hide().height(rect.height).width(rect.width);
            $holder.insertBefore($root);
            document.addEventListener('scroll', this.onScroll);
        }
    },
    isSupportSticky: function () {
        if (CSS.supports('position', 'sticky') 
            || CSS.supports('position', '-webkit-sticky')
            || CSS.supports('position', '-moz-sticky')
            || CSS.supports('position', '-o-sticky')) {
            return true;
        }
        return false;
    },
    onScroll: function (e) {
        var el = $root[0];
        var top = $root.data('top');
        if (window.scrollY < top) {
            el.style.position = 'relative';
            $('.progress-holder').hide();
        }
        else {
            el.style.position = 'fixed';
            $('.progress-holder').show();
        }
    }
}
module.exports = fixBar;
```

