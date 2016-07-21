# ShapeLocker
PATTERN LOCK, unlock with gestures

(手势解锁,九宫格解锁,图形解锁)

UNSOLVED:<br>
尚未解决的问题:
 (不影响真机运行的应用, 有在模拟器上运行的需求见下文解决办法)<br>
 有同学反映<strong><em>模拟器上运行</strong></em>会出现"密码圈绘制不全"的bug, 如图<br>
 
 ![image](https://github.com/panespanes/ShapeLocker/blob/master/mdp.png)
 <br>
 做了些实验, 真机不会出现这个bug, AVD自带模拟器没有重现, BlueStacks模拟器没有类似情况,<br> 而"靠谱助手","海马玩","逍遥模拟器"等VirtualBox底层的模拟器均重现bug.<br>
 猜测是由于VirtualBox的驱动对于安卓UI图形绘制的支持并不全面导致的:<br>
 具体原因ShapeLocker对界面刷新做了优化, 响应触摸事件后<strong><em>只重绘被影响区域</em></strong>,<br> 
 相关代码:   
 ```java  

 Rect invalidateRect = new Rect();
 ...
 invalidateRect.set((int) (left - radius), (int) (top - radius), (int) (right + radius), (int) (bottom + radius));
 ...
 invalidateRect.union((int) (left - radius), (int) (top - radius), (int) (right + radius), (int) (bottom + radius));
 ...
 invalidateRect.set((int) (left - widthOffset), (int) (top - heightOffset), (int) (right + widthOffset), (int) (bottom + heightOffset));
 ...
 invalidate(invalidateRect);

```
 而VirtualBox并不会按照严格按照rect大小重绘区域.<br>
 解决办法(真机运行的项目中请不要这样修改, 因为会造成平均34%左右性能损失):
 将这部分代码改为
 ```java
invalidate();
 ```
 即可.
