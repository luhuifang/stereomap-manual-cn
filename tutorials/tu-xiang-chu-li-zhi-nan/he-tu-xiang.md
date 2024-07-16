# H&E图像

## 为什么要做H&E染色图像?

苏木素-伊红（H&E染色）染色是一种常用的组织染色，可为医学诊断提供组织学信息，被认为是金标准。苏木素主要将细胞核染成蓝紫色，而伊红主要将细胞质和细胞外基质染成不同深浅的粉红色，通过 Stereo-seq 生成的空间表达矩阵与 H&E 染色图像相结合，可以将细胞的形态与空间局部特征表达关联起来，挖掘组织切片中更多的信息。

## H&E染色图像的注意事项 <a href="#considerations-for-nuclei-staining-images" id="considerations-for-nuclei-staining-images"></a>

因 Stereo-seq 芯片是不透明的硅片，H&E 染色的图像需要用落射明场的显微镜成像，拍摄的背景颜色和轨迹线的颜色都接近为白色。与荧光灰度图像不同，H&E 染色图像通过 R-G-B 颜色的组合可以提供复杂的组织病理学信息，图像的文件大小比相同尺寸的灰度图像要大得多，故所需的计算资源也会相对较高。目前，StereoMap 在 16GB 的内存下，仅能够处理 24 位彩色、10 倍镜下1cm x 1cm 芯片的图像数据。

StereoMap 和 SAW 中处理的H&E染色图像为单张 24 位深的彩色图像。（有关更多信息，请参见 [图像类型和格式](./#tu-xiang-lei-xing-he-ge-shi) 获取更多信息。）

| 荧光图像 | 数据类型和格式    |
| ---- | ---------- |
| 彩色图像 | 24位的单张彩色图像 |

## 第一步：上传图像 <a href="#step-1-upload-image" id="step-1-upload-image"></a>

<figure><img src="../../.gitbook/assets/color-select image file.png" alt=""><figcaption></figcaption></figure>

点击选择框中的 **Choose file**，选择一个兼容的图像文件或者也可以将所选文件直接拖入到选择框中，核染色图像类型仅允许上传一个图像文件。

请参考[图像处理输入文件](../../kuai-su-kai-shi.md#tu-xiang-chu-li-shu-ru-wen-jian)获取更详细的图像文件信息。

<figure><img src="../../.gitbook/assets/color-image parsing info.png" alt=""><figcaption></figcaption></figure>

选择文件后会触发一个文件解析的过程，该过程不仅会读取图像，还会从输入中获取必要的信息，解析的时间会因文件类型和图像大小有所差异。在图像处理步骤，Stereo-seq 芯片序列号（SN）和显微镜配置会提供重要的参考信息，如果输入的文件是`.tar.gz` 或者 `.stereo`，则该输入文件已包含这些信息；如果输入的文件格式为`.tif` 或者`.tiff`，则需要用户输入指定的必要信息才可以做图像的解析和质控分析,一般情况下，您可以选择显微镜配置来输入有关显微镜的信息。请参阅[显微镜信息配置](../../xia-zai.md#xian-wei-jing-xin-xi-pei-zhi)获取更多信息。

如果图像质控完成，您可以查看每个 QC 指标前指示器的状态。这些指示器与图像处理结果相关，您可能会在步骤标签上看到一些提示图标，指示某个步骤的潜在风险。

| 图标                                                                                                                                                                                                   | 说明                                                                                                   |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| <img src="../../.gitbook/assets/image (65).png" alt="" data-size="original">                                                                                                                         | **完成。**您已经完成了此步骤。                                                                                    |
| ​![](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FKPjxR1Lv74t5QCTxNb8d%2Fuploads%2FSQnwX4aIvqws6GaF2rpo%2Fwarning.png?alt=media&token=62387cad-f1e5-47d2-afcd-14ae2ed1c540) | **警告**。您可能需要特别注意这一步骤。图像质量对于自动操作来说并不理想。例如，如果您在第 2 步图像配准时看到此警告，这可能表明 SAW 中的自动配准可能出现了错误。您需要检查结果并手动进行调整。 |
| <img src="../../.gitbook/assets/image (64).png" alt="" data-size="original">                                                                                                                         | **错误。**您的图像有问题，无法在此步骤中处理。                                                                            |

## 第二步：图像配准

在该步骤中，您需要调整图像的方向、角度和尺度，以使其与空间特征表达矩阵对齐。

<figure><img src="../../.gitbook/assets/color-regiatration.png" alt=""><figcaption></figcaption></figure>

如果您在第 一步中：上传了 `.stereo`文件，进入第二步时，您就可以同时看到图像和空间特征表达矩阵；上传了 `.tar.gz` 或 `.tif/.tiff` 图像文件，在第二步您需要选择一个`.stereo` 文件来指定空间特征表达矩阵，可以点击 <img src="../../.gitbook/assets/image (67).png" alt="" data-size="line">选择 `.stereo` 文件。此外，如果您在第一步上传了`.stereo` 文件（包含图像文件），想更改第二步展示的空间特征表达矩阵文件，您可以点击<img src="../../.gitbook/assets/image (68).png" alt="" data-size="line">重新加载一个新的矩阵文件，但还是使用第 1 步上传的`.stereo`的图像。

<figure><img src="../../.gitbook/assets/color-add or replace matrix.png" alt=""><figcaption></figcaption></figure>

手动配准过程包括两个阶段：根据组织形态粗略的匹配图像的方向，然后精细调整图像的位置和尺度，确保与空间特征表达矩阵完全重叠。您还可以通过 **Chip trackline** ，辅助进行精细对齐。

为了粗略对齐图像，您需要将显微镜图像与特征矩阵方向变换为一致。

* 使用**翻转工具**<img src="../../.gitbook/assets/Flip.png" alt="" data-size="line">可以将图像绕Y轴镜像翻转。
* 点击**旋转按钮**<img src="../../.gitbook/assets/Rotation.png" alt="" data-size="line">使用<img src="../../.gitbook/assets/control_knob.png" alt="" data-size="line">可以将图像以相同的方向旋转。

当图像方向跟表达矩阵方向一致时，您就可以继续进行精细对齐了。

做精细配准时，您需要将图像移动到有组织覆盖的区域。

* 通过**平移面板** <img src="../../.gitbook/assets/Step move.png" alt="" data-size="line">设置移动的步长和移动的四个方向（上、下、左、右）。
* 因显微镜拍摄的图像数据的尺寸跟特征矩阵不同，您可以使用**缩放工具**<img src="../../.gitbook/assets/scale.png" alt="" data-size="line">来调整缩放的比例。

您可以勾选 **Chip trackline** 显示参考轨迹线模板，辅助对齐图像。此时，参考轨迹线模板可以作为矩阵的一种表示形式，如果轨迹线较暗，可以手动调整图像的**归一化**<img src="../../.gitbook/assets/normalization.png" alt="" data-size="line">、**对比度**<img src="../../.gitbook/assets/Contrast.png" alt="" data-size="line">、**亮度**<img src="../../.gitbook/assets/brightness.png" alt="" data-size="line">、**不透明度**<img src="../../.gitbook/assets/opacity.png" alt="" data-size="line">和饱和度<img src="https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FKPjxR1Lv74t5QCTxNb8d%2Fuploads%2FwTCPv8qijlzGWobcLkAR%2Fsaturation.png?alt=media&#x26;token=1d8181b1-329a-4015-9ec9-75b269b8b2bd" alt="" data-size="line">等工具按钮。

<figure><img src="../../.gitbook/assets/color-chip tracklines.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
调整图像以便可以清楚的展示轨迹线。
{% endhint %}

<figure><img src="../../.gitbook/assets/color-trackline overlap example.png" alt=""><figcaption></figcaption></figure>

完成配准的图像将标记为已完成![](<../../.gitbook/assets/image (48).png>)。

{% hint style="info" %}
如果做了手动配准，步骤 3 和 4 是可以跳过，您可以导出手动后的`TAR.GZ`图像文件，输入到 SAW 运行自动的组织分割和细胞分割。

另外，我们会输出配准后的`*regist.tif` 图像文件，**如果您考虑使用第三方分割工具，强烈建议使用此图像文件作为输入**，避免图像与 Mask 之间存在位移、角度和尺度的偏差。
{% endhint %}

## 第三步：组织分割

{% hint style="info" %}
&#x20;**组织分割**是个可跳过的步骤。
{% endhint %}

该步骤中，您需要识别组织区域，准确的识别组织边界可以减少背景干扰对于聚类结果的影响。基于图像的组织分割结果将映射到空间特征表达矩阵上，生成组织区域的表达热图。

<figure><img src="../../.gitbook/assets/color-tissue seg.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
如果您在第一步中上传了`.stereo`文件，您可以在配准后的图像图层上看到一个半透明的组织掩膜。

如果是`.tar.gz`或`.tif/.tiff`图像文件，您需要使用手动工具来绘制组织区域。
{% endhint %}

在该步骤中，您可以编辑之前记录的组织掩膜或创建一个新的掩膜。如果是`.tar.gz`或`.stereo`文件中记录的组织掩膜将会在 **Segmentation mask** 下拉菜单中标记为 **RECORD**，而通过手动绘制或导入创建的掩膜将标记为 **CUSTOM** 。如需更在画布显示的掩膜图像，只需从 **Segmentation mask** 下拉菜单中进行选择切换即可。

<figure><img src="../../.gitbook/assets/color-tissue mask tag.png" alt=""><figcaption></figcaption></figure>

如果需要编辑组织区域的掩膜图像，可以使用**套索**<img src="../../.gitbook/assets/image (57).png" alt="" data-size="line">、**画笔**<img src="../../.gitbook/assets/image (58).png" alt="" data-size="line">和**橡皮擦**<img src="../../.gitbook/assets/image (60).png" alt="" data-size="line">等工具的组合。**套索**一般用于选择或剔除大面积区域，而**画笔**和**橡皮擦**工具更适合于编辑组织周围或组织中的小孔等较小的区域。

<div>

<figure><img src="../../.gitbook/assets/color-tissue mask lasso.png" alt=""><figcaption><p>套索</p></figcaption></figure>

 

<figure><img src="../../.gitbook/assets/color-tissue mask brush.png" alt=""><figcaption><p>画笔</p></figcaption></figure>

 

<figure><img src="../../.gitbook/assets/color-tissue mask eraser.png" alt=""><figcaption><p>橡皮擦</p></figcaption></figure>

</div>

您可以通过单击右侧面板上的 **Segmentation mask** 下拉菜单，单击<img src="../../.gitbook/assets/image (61).png" alt="" data-size="line">来导入第三方分割工具创建的`.tif`格式的二进制掩膜文件，如果对于导入的结果不满意，您可以单击<img src="../../.gitbook/assets/image (62).png" alt="" data-size="line">来替换新的掩膜文件。

<div>

<figure><img src="../../.gitbook/assets/color-tissue mask import.png" alt=""><figcaption><p>导入组织 Mask</p></figcaption></figure>

 

<figure><img src="../../.gitbook/assets/color-tissue mask import result.png" alt=""><figcaption><p>展示导入的组织 Mask 的名称</p></figcaption></figure>

 

<figure><img src="../../.gitbook/assets/color-tissue mask replace.png" alt=""><figcaption><p>替换组织 Mask</p></figcaption></figure>

</div>

## 第四步：细胞分割

{% hint style="info" %}
**细胞分割**是个可跳过的步骤。
{% endhint %}

细胞分割是生成单细胞空间分辨率数据的核心步骤，H&E 染色的图像，可以对核区域或者细胞区域做分割。

<figure><img src="../../.gitbook/assets/color-cell seg.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
如果您在第一步中上传了`.stereo`文件，您可以在配准后的图像图层上看到一个红色的细胞/细胞核掩膜。

对于`.tar.gz`或`.tif/.tiff`图像文件，您需要使用手动工具来绘制细胞。

因只在组织区域的范围内展示对应的细胞分割结果，组织外的区域将会展示为白色的背景。

![](<../../.gitbook/assets/color-cell seg zoom.png>)
{% endhint %}

类似于组织分割，您可以选择编辑之前已经记录的掩膜（标记为 **RECORD** ）或创建一个新的掩膜（标记为 **CUSTOM** ）。通过从 **Segmentation mask** 下拉菜单中切换画布中展示的掩膜。

<figure><img src="../../.gitbook/assets/color-cell mask tag.png" alt=""><figcaption></figcaption></figure>

使用**套索**<img src="../../.gitbook/assets/image (54).png" alt="" data-size="line">、**画笔**<img src="../../.gitbook/assets/image (55).png" alt="" data-size="line">和**橡皮擦**<img src="../../.gitbook/assets/image (56).png" alt="" data-size="line">工具来编辑细胞。**套索**最适合用于取消大面积的背景，而**画笔**和**橡皮擦**工具更适合于较小的区域，例如标记某个细胞。

<div>

<figure><img src="../../.gitbook/assets/color-cell mask lasso.png" alt=""><figcaption><p>使用套索工具套索一个细胞</p></figcaption></figure>

 

<figure><img src="../../.gitbook/assets/color-cell mask lasso deselect.png" alt=""><figcaption><p>使用套索工具的反选删除部分细胞</p></figcaption></figure>

</div>

<div>

<figure><img src="../../.gitbook/assets/color-cell mask brush.png" alt=""><figcaption><p>画笔</p></figcaption></figure>

 

<figure><img src="../../.gitbook/assets/color-cell mask eraser.png" alt=""><figcaption><p>橡皮擦</p></figcaption></figure>

</div>

建议您使用配准后的图像导入到第三方分割工具创建`.tif`格式的二进制细胞掩膜文件。您可以通过单击右侧面板上的 **Segmentation mask** 下拉菜单，单击<img src="../../.gitbook/assets/image (61).png" alt="" data-size="line">来导入第三方分割工具创建的`.tif`格式的二进制掩膜文件，如果对于导入的结果不满意，您可以单击<img src="../../.gitbook/assets/image (62).png" alt="" data-size="line">来替换新的掩膜文件。

<div>

<figure><img src="../../.gitbook/assets/color-cell mask import.png" alt=""><figcaption><p>导入细胞 Mask</p></figcaption></figure>

 

<figure><img src="../../.gitbook/assets/color-cell mask import result.png" alt=""><figcaption><p>展示导入的细胞 Mask 的名称</p></figcaption></figure>

 

<figure><img src="../../.gitbook/assets/color-cell mask replace.png" alt=""><figcaption><p>替换细胞 Mask</p></figcaption></figure>

</div>

## 第五步：导出

最后是导出图像配准、组织分割和细胞分割的结果。单击 **Export image processing record** 将生成一个`.tar.gz`文件和一个`.tif`文件。

<figure><img src="../../.gitbook/assets/color-export.png" alt=""><figcaption></figcaption></figure>

手动后的文件可以在 _**StereoMapWorkspace -> Processing**_ 路径下查看，该路径位于您指定的保存路径中，默认为电脑的 _**D**_ 盘，您可以在启动页的[设置](../../xia-zai.md#she-zhi)中修改[保存路径](../../xia-zai.md#bao-cun)。如果在第二步做了手动配准，此文件夹下会输出配准后的图像 `*regist.tif` ，如果您未做手动配准，该 TIFF 文件可以在 SAW 的输出目录 `/outs/`文件夹中找到。

<figure><img src="../../.gitbook/assets/color-export directory.png" alt=""><figcaption></figcaption></figure>

`.tar.gz`文件包含原始的图像数据和手动处理的记录。此文件用于在SAW将图像数据和测序数据一起分析。`.tar.gz`文件的内部结构是固定的，不建议对其结构或者文件做任何修改。

`.tif`文件是经过裁剪和调整的配准后的图像，跟特征表达矩阵尺寸相同。它可以在任何第三方分割工具中使用，并且生成的数据可以重新导入到 StereoMap。

## 手动后的图像结果 TAR.GZ 接回 SAW

手动后的图像数据有两种选择可以接回 SAW 跑后续分析。

其中一种选择为：运行`SAW count`流程，将手动后的图像文件`.tar.gz`作为`--image-tar`参数的输入。`SAW count`流程将 Stereo-seq 测序的 FASTQ 文件与手动后的图像文件一起分析，，并生成 `HTML`报告。

```bash
cd /saw/runs

saw count \
    --id=<ID> \
    --sn=<SN> \
    --omics=<OMICS> \
    --kit-version=<TEXT> \
    --sequencing-type=<TEXT> \
    --chip-mask=/path/to/chip/mask \
    --organism=<organism> \
    --tissue=<tissue> \
    --fastqs=/path/to/fastq/folders \
    --reference=/path/to/reference/folder \
    --image-tar=/path/to/image/tar
```

另一种选择为：运行`SAW realign`流程，将手动后的图像文件`.tar.gz`作为`--realigned-image-tar`参数的输入。`SAW realign`流程跳过比对步骤，基于手动后的结果文件重新生成配准后的图像数据，输出对应的矩阵文件，并生成`HTML`报告。

```bash
cd /saw/runs

saw realign \
    --id=<ID> \
    --sn=<SN> \
    --count-data=/path/to/previous/SAW/count/task/folder/id \
    --realigned-image-tar=/path/to/realigned/image/tar
```
