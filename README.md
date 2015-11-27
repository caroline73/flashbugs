# flashbugs
在安全浏览器7.1版本，极速模式下，flash遮罩的问题

项目需求：
在项目内，一个页面内嵌入第三方flash，是以iframe嵌入形式实现的。新需求是在页面添加悬浮广告，在标题栏可以拖拽到任意位置，而且可以在mini模式和normal模式之间切换。
方案：
在页面，添加与引入iframe同级的dom，如下
<div class="video-content">
    <div class="xiu-play">
        <div  class="xiu-control">
             <a class="xiu-control-mini"></a>
       </div>
       <iframe src="http://xiu.youxi.com/activity/activity_xiu_video"></iframe>
       <a class="xiu-control-back"></a>
    <div>
</div>
遇到的问题：
浏览器兼容问题，
1、在ie浏览器下，除了iframe元素引用的页面之外，其他的dom节点被flash挡住，表现就是，拖拽的标题栏没有展示出来；
2、在安全浏览器极速模式下，嵌入的flash，将上一段dom完全遮挡住；
解决方案：
1、在ie浏览器下，在上一段dom的同级加入一个iframe，与广告内容大小相同，可以起到垫片的作用；
2、1中的解决方案并不能解决安全浏览器下的兼容问题，解决方案是在页面，通知第三方flash创建和广告内容大小位置相同的iframe用作垫片；
总结： 
解决方案2可以解决上面的两个问题，如果漂浮的广告不能拖拽，那么就完美解决了，但是，要实现拖拽，还是有一点不完美啊。
拖拽的时候，后面的垫片也要跟着移动，思路是这样的，当拖拽开始，后面的垫片就是一个和window一样大小的iframe，当拖拽结束，垫片就恢复和广告浮窗内容大小一致。
于是又发现了一个小问题，那就是iframe在安全浏览器7.1下，设置透明后，就真正的透明了，浏览器内容是什么颜色，iframe就是什么颜色，在本项目中，设置了body为黑色背景，所以当垫片变大的时候，就透过iframe，显示黑色的垫片了。
