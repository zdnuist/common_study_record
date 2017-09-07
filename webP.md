##### WebP
WebP（发音 weppy），是一种支持有损压缩和无损压缩的图片文件格式，派生自图像编码格式 VP8。根据 Google 的测试，无损压缩后的 WebP 比 PNG 文件少了 45％ 的文件大小，即使这些 PNG 文件经过其他压缩工具压缩之后，WebP 还是可以减少 28％ 的文件大小。
PNG 转 WebP 的压缩率要高于 PNG 原图压缩率，同样支持有损与无损压缩
转换后的 WebP 体积大幅减少，图片质量也得到保障（同时肉眼几乎无法看出差异）
转换后的 WebP 支持 Alpha 透明和 24-bit 颜色数，不存在 PNG8 色彩不够丰富和在浏览器中可能会出现毛边的问题

WebP 的优势体现在它具有更优的图像数据压缩算法，能带来更小的图片体积，而且拥有肉眼识别无差异的图像质量；同时具备了无损和有损的压缩模式、Alpha 透明以及动画的特性，在 JPEG 和 PNG 上的转化效果都非常优秀、稳定和统一

##### Android平台测试
解码耗时：WebP 的解码时间是 PNG 格式的 4.4 倍（24.8ms）
流畅程度：两种格式下，AIO 滑动流畅度无明显差异
CPU使用：两种格式下，连续发送 15 个表情，CPU 使用均在 10%—26% 之间波动，两者无明显差异
内存占用：两者格式下，连续发送 15 个表情，PSS 内存占用跨度均为 11M，无明显差异

WebP 使用的是 Fancy 采样算法，既然是采样算法必然有采样区块，而 JPEG 的采样区块是 8*8，对于原始图片的长宽不是 8 的倍数，都需要先补成 8 的倍数，使其能一块块的处理，所以对于 8 的整数倍的图片，压缩会更高效

在色彩数相对较少的前提下，无损压缩的效果要优于有损压缩；而色彩数很多时，有损压缩效果要优于无损压缩，这个分界点在 256±100 之间

##### Animated WebP 的优势
支持有损和无损压缩，并且可以合并有损和无损图片帧
体积更小，GIF 转成有损 Animated WebP 后可以减小 64% 的体积，转成无损可以节省 19% 的体积
颜色更丰富，支持 24-bit 的 RGB 颜色以及 8-bit 的 Alpha 透明通道（而 GIF 只支持 8-bit RGB 颜色以及 1-bit 的透明）
添加了关键帧、metadata 等数据

##### Animated WebP 存在的问题
消耗较多的 CPU 和解码时间（多 1.5~2.2 倍）
和 GIF 相比起来支持度还不够，目前仍无法通用
为了支持 Animated WebP，Chrome 的新内核 Blink 添加了近 1500 行的代码


##### 参考
[1] https://isux.tencent.com/introduction-of-webp.html