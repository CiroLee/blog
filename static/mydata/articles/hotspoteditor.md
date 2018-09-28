## 热点简易编辑器      

krpano是一款强大的全景引擎，可以帮助全景爱好者和开发者快速的部署全景项目。全景交互中非常重要的一个工具——热点。krpano中的热点非常灵活，可以自定义样式，可以跳转到其他场景，甚至可以自定义事件。但是，官网提供的编辑器的编辑能力不足，很多时候都要不停的修改参数，然后反复刷新查看效果，尤其是在编辑热点的**3D属性**时，反复的调试更是令人抓狂。因此，就萌生了写一款可以快速编辑热点并且实时预览的工具。        

经过无数个日日夜夜的努力（其实是偷懒啦~~），终于完成了热点编辑器的简易版，特点是使用方便，所有属性操作都可以实时预览。       

![https://mmbiz.qpic.cn/mmbiz_png/aKmia1xkgAZYFbcJAWZ5c53cABIwr9FxjQawh5ibBzica9e561X0ELukI9LcLqJnb9wpVTbNIUMLSwttpSYA3TaPA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1](https://mmbiz.qpic.cn/mmbiz_png/aKmia1xkgAZYFbcJAWZ5c53cABIwr9FxjQawh5ibBzica9e561X0ELukI9LcLqJnb9wpVTbNIUMLSwttpSYA3TaPA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)         

![https://mmbiz.qpic.cn/mmbiz_png/aKmia1xkgAZYFbcJAWZ5c53cABIwr9FxjLiaRDyeVDEH90wx7Hcky2OWHanBdQfFHqwWzBkTH917NBnjwTO5AyFQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1](https://mmbiz.qpic.cn/mmbiz_png/aKmia1xkgAZYFbcJAWZ5c53cABIwr9FxjLiaRDyeVDEH90wx7Hcky2OWHanBdQfFHqwWzBkTH917NBnjwTO5AyFQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)           

热点简易编辑器包含了热点hotspot的常用属性的编辑，如：拖拽、尺寸（scale）、透明度（alpha）、深度（depth）、旋转（rotate）、偏移（offset）和3D旋转（3drotate）。    

使用起来也非常方便：    

1.选择全景文件夹：

![https://mmbiz.qpic.cn/mmbiz_png/aKmia1xkgAZYFbcJAWZ5c53cABIwr9Fxj4VFUQaib6OMUVicAEPORVTMpspMSr3O8vzuGyQ7q6pCjn63LvPw3DvhA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1](https://mmbiz.qpic.cn/mmbiz_png/aKmia1xkgAZYFbcJAWZ5c53cABIwr9Fxj4VFUQaib6OMUVicAEPORVTMpspMSr3O8vzuGyQ7q6pCjn63LvPw3DvhA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)        

注意：全景主文件必须是tour.xml和tour.swf，且位于根目录；     

2.点击右上角齿轮图标，打开工具栏：

![https://mmbiz.qpic.cn/mmbiz_png/aKmia1xkgAZYFbcJAWZ5c53cABIwr9FxjjY5ibhPBVCJtkZ1bNQv4b28zNGiaa9oMdTKJ5ZJAVZwEdew9scCv9AAg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1](https://mmbiz.qpic.cn/mmbiz_png/aKmia1xkgAZYFbcJAWZ5c53cABIwr9FxjjY5ibhPBVCJtkZ1bNQv4b28zNGiaa9oMdTKJ5ZJAVZwEdew9scCv9AAg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)            

     

3.右侧彩色图标为各个工具，左侧下拉列表会罗列出可编辑的热点。首先选择要编辑的热点，然后点击右侧工具进行编辑。      

![https://mmbiz.qpic.cn/mmbiz_png/aKmia1xkgAZYFbcJAWZ5c53cABIwr9FxjKbWRd66llo9TiaMIfrzwzK2mkvAoDNO1JiagOe8HDH7vzIvTCLmjRhEA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1](https://mmbiz.qpic.cn/mmbiz_png/aKmia1xkgAZYFbcJAWZ5c53cABIwr9FxjKbWRd66llo9TiaMIfrzwzK2mkvAoDNO1JiagOe8HDH7vzIvTCLmjRhEA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)   

    

4.编辑结束后会弹出结果复制提示，然后把结果粘贴到对应代码处就可以啦！    

![https://mmbiz.qpic.cn/mmbiz_png/aKmia1xkgAZYFbcJAWZ5c53cABIwr9FxjLiaRDyeVDEH90wx7Hcky2OWHanBdQfFHqwWzBkTH917NBnjwTO5AyFQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1](https://mmbiz.qpic.cn/mmbiz_png/aKmia1xkgAZYFbcJAWZ5c53cABIwr9FxjLiaRDyeVDEH90wx7Hcky2OWHanBdQfFHqwWzBkTH917NBnjwTO5AyFQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)    



以上就是基本操作流程，是不是很简单！软件完全开源，[github传送门](https://github.com/CiroLee/hotspotEditor)

