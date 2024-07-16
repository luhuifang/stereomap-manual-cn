# 快速开始

## StereoMap 是什么?

StereoMap 是一个 0 代码的桌面端软件，支持 Stereo-seq 数据分析及交互式的可视化。它通过解析时空芯片测序数据中的空间条形码，还原每个生物分子在组织中的空间定位和表达水平，分析单个或者多个基因在原始切片中的分子表达，实现亚细胞分辨率的可视化，并深入发掘潜在的生物学意义。

在 Stereo-seq 数据分析工作流程中，上游及下游的分析步骤中都会用到 StereoMap。

<figure><img src=".img/spaces_KPjxR1Lv74t5QCTxNb8d_uploads_ee2XDYSKoV428mkQkc5d_workflow.webp" alt=""><figcaption></figcaption></figure>

在上游分析中，StereoMap 可以提供：[图像 QC](tutorials/xiao-gong-ju-zhi-nan/) 小工具，评估图像是否可以运行 SAW 的全自动分析流程；[图像处理](tutorials/tu-xiang-chu-li-zhi-nan/)模块，用户可以对图像做手动处理后接入 SAW 。在下游分析中，StereoMap 可以提供交互式[可视化](tutorials/ke-shi-hua-zhi-nan.md)和数据探索的功能。

<figure><img src=".img/spaces_KPjxR1Lv74t5QCTxNb8d_uploads_yiN7kaofTH3Fqd38zrk4_3 modules.webp" alt=""><figcaption></figcaption></figure>

* [**Visual Explore**](tutorials/ke-shi-hua-zhi-nan.md)：探索 Stereo-seq 数据的关键模块，您可以交互式地研究组织内不同 bin 或者 cell 下的特征表达分布情况。此外，它还支持将图像或多组学的数据通过多窗口的形式共可视化，为下游分析提供更多的参考信息。
* &#x20;[**Image Processing**](tutorials/tu-xiang-chu-li-zhi-nan/)：图像处理的核心模块，您可以手动将图像与表达矩阵对齐，也可以使用图画工具编辑图像的 Mask 或者导入第三方工具生成的 Mask，手动操作完成可以将手动后的文件输入到 SAW 做后续分析。
* [**Tools**](tutorials/xiao-gong-ju-zhi-nan/)： 图像处理和 Stereo-seq 数据集所需的分析小工具。&#x20;
  * [Stereo-seq Image QC](tutorials/xiao-gong-ju-zhi-nan/#tu-xiang-zhi-kong)：用于评估显微镜图像的质量，确认在 SAW 中是否可以进行自动分析。

***

## StereoMap 的输入文件

如下展示了图像处理和可视化模块需要用到的显微镜图像文件和 SAW 输出的文件。

### 可视化输入文件

下载并解压缩 SAW 输出的压缩包文件(`visualization.tar.gz`)，将会找到一个`.stereo`的清单文件，用户可以直接使用该清单文件打开可视化的数据，它包含了 SAW 输出的`visualization.tar.gz` 文件中用于可视化展示的文件路径映射信息。在使用`.stereo`的清单文件打开可视化的数据时，请您务必遵循如下规则：

* 默认情况下，可视化展示的所有文件与清单文件都在相同的目录路径中进行管理，强烈建议保持 SAW 输出的原始目录的文件结构。如果某个文件被移动到另一个位置或文件名已更改，请切记一定要修改清单文件中对应的文件路径。
* [可视化](tutorials/ke-shi-hua-zhi-nan.md)模块的目录中必须至少有一个基因表达（矩阵）文件（.gef）或者一个图像金字塔文件（.rpi）可用。

<table><thead><tr><th width="234">文件扩展名</th><th>描述</th></tr></thead><tbody><tr><td><code>.stereo</code></td><td><p>是 JSON 格式的清单文件，包含实验信息、流程信息、基本的分析统计信息及 SAW 输出的图像和空间表达矩阵的映射信息的可视化文件。</p><p></p><p><em>解压 SAW 输出的压缩包文件(<code>visualization.tar.gz</code>)，将会找到一个<code>.stereo</code>的清单文件，StereoMap 支持打开 SAW 流程输出的可视化文件。</em></p></td></tr><tr><td><code>.gef</code></td><td>用于可视化的基因表达（矩阵）文件，是 HDF5 格式。包含每个 spot 点的每个基因的 MID 的个数。Spot 点是固定大小的正方形形状，基因在每个 spot 点的表达量在该正方形中的累计加和。默认情况下，可视化的<code>.gef</code>文件包含 bin1、bin5、bin10、bin20、bin50、bin100和 bin200的点。<br><img src=".img/spaces_KPjxR1Lv74t5QCTxNb8d_uploads_my7dfb1Nh0ZH7xVs7z70_bin_size_example.webp" alt=""></td></tr><tr><td><code>.cellbin.gef</code></td><td><p>用于可视化的细胞特征的基因表达（矩阵）文件，是 HDF5 格式。它包含每个细胞的面积和空间位置，每个细胞中每个基因的 MID 的个数以及每个细胞所属的类群信息。在 <code>.cellbin.gef</code> 中，最小的单元为细胞。</p><p><img src=".img/spaces_KPjxR1Lv74t5QCTxNb8d_uploads_UOj2rqtoHLn86qXl8ElX_cell_bin_example.webp" alt=""></p><p><em>仅适用于SAW运行有图的流程</em></p></td></tr><tr><td><code>.rpi</code></td><td><p><code>.rpi</code>文件以金字塔形式保存多种分辨率的图像，主要为更好的做可视化展示。每一个图像被降采样多个分辨率的图层，按照 256*256 的像素进行切割并将图像碎片进行保存。若在当前分辨率下的图片尺寸小于 256ⅹ256，则无需切割。<img src=".img/image.png" alt=""></p><p><em>可以在 StereoMap 的</em><a href="tutorials/ke-shi-hua-zhi-nan.md"><em>可视化</em></a><em>模块打开图像数据。</em></p></td></tr><tr><td><code>&#x3C;SN>.bin&#x3C;N>_&#x3C;leiden_res>.h5ad</code></td><td><code>.h5ad</code>文件以 AnnData 文件格式存储空间转录组的聚类信息，每一个<code>&#x3C;SN>.bin&#x3C;N>_&#x3C;leiden_res>.h5ad</code>的文件只允许存储一个 bin 大小的分析结果。在生成的文件中，<code>&#x3C;SN></code> 代表 Stereo-seq 芯片序列号，<code>&#x3C;N></code> 代表 bin 大小，<code>&#x3C;leiden_res></code> 代表 Leiden 的分群颗粒度。在 SAW 标准分析中，默认的空间聚类分析使用的 binsize 为 200，Leiden 分群颗粒度为 1.0 。</td></tr><tr><td><code>&#x3C;SN>.cellbin_&#x3C;leiden_res>*.h5ad</code></td><td><p><code>.h5ad</code>文件以 AnnData 文件格式存储细胞的聚类信息。在</p><p>在生成的文件中， <code>&#x3C;SN></code> 代表 Stereo-seq 芯片序列号，<code>&#x3C;leiden_res></code> 代表 Leiden 的分群颗粒度，<code>*</code> 表示可选内容，指是否做了细胞修正。如果<code>*</code> 缺失，<code>.h5ad</code> 文件将对应的是基于细胞核覆盖区域的特征表达矩阵的聚类结果；如果 <code>*</code> 被 <code>.adjusted</code> 替换，<code>.h5ad</code> 文件将记录基于细胞核修正后的特征表达矩阵的聚类结果，修正的逻辑为：在保证两个细胞不重叠的情况下，默认在原来细胞核的基础上外扩10 个像素。在 SAW 标准分析中，默认细胞聚类分析使用 1.0 的 Leiden 分群颗粒度进行处理。</p><p></p><p><em>SAW的标准分析流程中只有输入图像数据才会生成该文件，因细胞的位置信息是从显微镜的图像中获取的。</em>     </p></td></tr></tbody></table>

### 图像处理输入文件

SAW 嵌入了自动的图像处理算法，用于图像拼接（小图 FOV 拼接成大图）、组织分割、细胞分割及识别 Stereo-seq 芯片上的轨迹线（tracklines），可将图像与具有相同轨迹线的特征表达矩阵对齐。对于组织/细胞边界模糊或者算法无法自动检测到轨迹线的数据，可能需要您手动圈选组织/细胞轮廓或者对齐。

[图像处理](tutorials/tu-xiang-chu-li-zhi-nan/)模块提供手动处理图像的一系列操作，主要包含手动圈选组织/细胞区域、导入第三方分割工具的Mask 结果以及手动将图像与特征表达矩阵对齐等功能。手动图像处理输入的数据包括：基于显微镜拍摄的原始 TIFF 大图图像或者运行 SAW 自动图像算法后的数据。

<table><thead><tr><th width="140">文件扩展名</th><th>描述</th></tr></thead><tbody><tr><td><code>.tar.gz</code></td><td><p>存储原始的显微镜图像及图像质控的信息。</p><p><br><a href="tutorials/xiao-gong-ju-zhi-nan/">小工具</a>-><a href="tutorials/xiao-gong-ju-zhi-nan/">图像质控 Image QC</a> 生成的文件。</p></td></tr><tr><td><code>.tif</code> 或者<code>.tiff</code></td><td>TIFF 格式的图像数据。<br><br><em>显微镜输出的大图。</em></td></tr><tr><td><code>.stereo</code></td><td><p>JSON 格式的可视化清单文件，包含 SAW 自动分析的图像结果文件及表达矩阵文件。<br></p><p><em><code>.stereo</code>的清单文件可以通过解压 SAW 输出的压缩包（<code>visualization.tar.gz</code>）找到，如果<code>.stereo</code>作为图像处理模块的输入文件，该清单文件中至少包含图像（<code>.tar.gz</code>）文件和表达矩阵 (<code>.gef</code>) 文件的路径。SAW 输出的图像压缩文件<code>.tar.gz</code>记录了自动算法分割和配准的结果。</em></p></td></tr></tbody></table>

**图像处理**的预期结果可能依据输入的数据会存在差异，更多详情可参考：[图像处理指南](tutorials/tu-xiang-chu-li-zhi-nan/)。
