# Stereo-seq N FFPE

## 基于 H&E 图像的区域标注

空间转录组可以在原始组织结构的背景下可视化基因表达热图，为从分子水平上挖掘组织的功能提供一定的指导意义。另一方面，H&E 染色可以提供组织结构和组织学信息的详细视图，整体着色可以显示细胞的总体布局和分布，使得病理学家能够轻松的区分细胞核和细胞质。

您可以基于病理组织学特征在 H&E 图像上标记感兴趣的区域，提取对应的表达矩阵数据，以全面分析组织的结构特征，可参考如下步骤：&#x20;

首先，调整基因表达热图的透明度，确保 H&E 图像可以清楚的看到。

<div>

<figure><img src="../../.img/adjust opacity.png" alt=""><figcaption></figcaption></figure>

 

<figure><img src="../../.img/adjust opacity HE.png" alt=""><figcaption></figcaption></figure>

</div>

接下来，您可以使用 Lasso 套索工具在 H&E 图像上进行标注（类似于[套索](stereo-seq-t-ff.md#tao-suo)），按键盘的 Enter 键完成本次标注，点击 **Save** 保存标注区域的名称及归属的组即可。

<div>

<figure><img src="../../.img/lasso region1.png" alt=""><figcaption></figcaption></figure>

 

<figure><img src="../../.img/lasso region2.png" alt=""><figcaption></figcaption></figure>

</div>

如果您需要将多个区域标注在一个组中分析，只需保存的时候将他们归属到相同的组中。

<div>

<figure><img src="../../.img/lasso region3.png" alt=""><figcaption></figcaption></figure>

 

<figure><img src="../../.img/lasso region4.png" alt=""><figcaption></figcaption></figure>

</div>

退出 Lasso 并调整基因表达热图的透明度，可以查看在不同图层上展示的标注结果。

<figure><img src="../../.img/lasso region5.png" alt=""><figcaption></figcaption></figure>

区域标注完成后，如果您需要获取所选区域的空间特征表达矩阵，可以单击右侧的按钮<img src="../../.img/image (125).png" alt="" data-size="line">，选择 **GeoJSON to lasso targeted area** 导出标注的区域文件。当然，如果您想要了解标注区域之间的差异，也可以选择 **GeoJSON for differential expressoin** 。

<figure><img src="../../.img/export.png" alt=""><figcaption></figcaption></figure>

您可以在 **StereoMapWorkspace -> Lasso** 目录下找到 Lasso后输出的 `YYYYMMDDHHMMSS.lasso.geojson` 文件，或者在 **StereoMapWorkspace -> Diffexp** 目录下找到差异分析输出的`YYYYMMDDHHMMSS.diffexp.geojson`文件。

请参考[套索](stereo-seq-t-ff.md#gou-hua-gan-xing-qu-qu-yu-bing-sheng-cheng-xin-de-re-tu)和[差异分析](stereo-seq-t-ff.md#cha-yi-fen-xi)了解如何将 GeoJSON 输入到 SAW 提取空间表达矩阵文件或者运行差异分析。

## 微生物宿主分析 <a href="#microorganism-and-host-genes" id="microorganism-and-host-genes"></a>

Stereo-seq N FFPE 产品可以通过随机探针捕获转录组+微生物的信息，使用`SAW count`的流程中的 `--micro-detect` 参数，可以输出宿主和微生物的空间基因表达矩阵。

在 StereoMap 的可视化模块，可以在 **Microorganism** 图层菜单查看微生物的矩阵数据，同时支持点击图层前![](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FKPjxR1Lv74t5QCTxNb8d%2Fuploads%2FZdCJmwFEa4Dh3dP7ulSV%2F%E9%A1%B5%E9%9D%A2\_1.png?alt=media&token=6bf2de76-fbde-4781-b9ea-bc61f5ce53a2)按钮打开新的子窗口。

<div>

<figure><img src="../../.img/open micro layer in new window.png" alt=""><figcaption></figcaption></figure>

 

<figure><img src="../../.img/micro-host side-by-side.png" alt=""><figcaption></figcaption></figure>

</div>

主窗口跟子窗口可以基于坐标点相互联动。在主窗口中，您可以选择某个聚类或使用 Lasso 高亮显示特定的区域，则对应子窗口会联动展示相应的内容。

&#x20;如下图所示，主窗口显示了 bin 200 的聚类图，而子窗口展示了相同 binsize 的微生物表达热图。当您在主窗口中选择 Cluster 1 时，子窗口将联动展示相同坐标点的信息。在子窗口中，您可以再次使用 Lasso 功能展示该区域的的统计信息。

<div>

<figure><img src="../../.img/select cluster.png" alt=""><figcaption></figcaption></figure>

 

<figure><img src="../../.img/lasso cluster region that have bacteria.png" alt=""><figcaption></figcaption></figure>

</div>

微生物按照分类的级别+双下划线+物种名称组成，比如 phylum Actinobacteria 在 Panel 中可以写成 _**p_Actinobacteria**_，其中分类的级别为p（门），c（纲），o（目），f（科），g（属）和s（种）等首字母的缩写。
