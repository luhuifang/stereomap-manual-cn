# 图像质控-Image QC

图像质控 (Image QC) 用于评估 Stereo-seq 实验拍摄的显微镜图像是否可以在 Stereo-seq 分析流程软件包(SAW) 中准确的自动分析。

## QC 入口

从 StereoMap 启动页的 **Tools** 进入 **Image QC**。

<figure><img src="../../.img/image QC entry (1).png" alt=""><figcaption></figcaption></figure>

## **图像类型** <a href="#input-image-recommendation" id="input-image-recommendation"></a>

支持的图像类型包括核染色图像（ssDNA 或 DAPI ）、一张核染色图像（DAPI）和最多六张免疫荧光图像（ IF 图像），以及彩色图像（ H&E 图像），推荐的图像文件格式是TIFF，但是我们针对部分显微镜输出的小图文件也做了适配，比如蔡司的 CZI 文件或 Motic 的小图文件。如果您不确定您的显微镜成像系统是否适用于 Stereo-seq 实验，请参考 **STOmics 显微镜评估参考手册**以评估您的成像系统，可以从 [STOmics](https://www.stomics.tech/) > [文档库](https://www.stomics.tech/resources/Documents)中访问该指南。

### 数据类型 <a href="#image-data-types" id="image-data-types"></a>

<table><thead><tr><th width="148">图像类型</th><th>数据类型</th><th>文件格式</th></tr></thead><tbody><tr><td>核染色</td><td><p>8 bit /16 bit 的单通道的灰度图像</p><p><em>推荐该数据格式</em></p></td><td><p>TIFF 文件 (<code>.tif</code> or <code>.tiff</code>) </p><p>CZI  (<code>.czi</code>)小图文件</p></td></tr><tr><td>核染色+免疫荧光</td><td>8 bit /16 bit 多张单通道的灰度图像</td><td>TIFF 文件(<code>.tif</code> or <code>.tiff</code>) </td></tr><tr><td>核染色+免疫荧光</td><td><p>8 bit /16 bit 单张多通道的灰度图像</p><p><em>只适配了蔡司的多通道显微镜图像</em></p></td><td>CZI  (<code>.czi</code>)小图文件</td></tr><tr><td>H&#x26;E</td><td>24 bit /48 bit 的单张彩色图像</td><td><p>TIFF  文件(<code>.tif</code> or <code>.tiff</code>) </p><p>CZI  (<code>.czi</code>)小图文件</p></td></tr></tbody></table>

### 文件格式 <a href="#image-file-format" id="image-file-format"></a>

<table><thead><tr><th width="216">文件格式</th><th width="490">显微镜型号</th></tr></thead><tbody><tr><td><code>.tif</code> or <code>.tiff</code> file</td><td><ul><li>不区分</li></ul></td></tr><tr><td>QC后的输出文件<code>.tar.gz</code> </td><td><ul><li><strong>图像质控</strong>的输出文件TAR.GZ</li></ul></td></tr><tr><td><code>.tif</code>格式的小图文件</td><td><ul><li>Customized Motic</li><li> STOmics Go Optical</li><li>Leica DM6 B</li></ul></td></tr><tr><td>其它格式</td><td><ul><li><code>.czi</code> file from ZEISS Axio Scan.Z1</li><li><code>.czi</code> file from ZEISS Axioscan 7</li></ul></td></tr></tbody></table>

请查阅 [QC 操作指南](tu-xiang-zhi-kong-image-qc.md#qc-cao-zuo-zhi-nan)获取每种染色的输入数据示例。

## QC 指标及标准

图像的质量直接反映了显微镜的稳定性和成像系统的优劣。图像的基本的评估包括 track 线评估及拼接评估，当然可能会根据染色类型的不同，增加图像清晰度评估和校准评估。每种图像的评估组合及评估标准会依据图像类型而略有不同。

评估指标包括：

* **Track线评估**：Track 是刻在Stereo-seq芯片表面的标记线。 显微镜拍摄的图像和空间特征表达矩阵都可以被检测到 track线，可以将这两种数据模态结合起来，用于将图像跟表达矩阵的对齐。Track 线得分综合考虑了 track 线的清晰度（是否可见）及相邻 track 线的数量等因素，算法根据检测的 track 线，可以计算图像的角度、尺度和位置等并推导出图像的全局模板，以评估图像是否可以自动成功配准。
* &#x20;**图像清晰度评估**： 图像清晰度分数可以作为评估细胞分割结果可行性的参考。计算的方式为：运用训练好的深度学习模型对输入的图像进行切分，将切分后的图像分为好、中度模糊、重度模糊、轻微过曝和重度过曝五个类型，对每种分类赋予不同的权重，最后计算整个图像的清晰度得分。
* **显微镜拼接稳定性：**将显微镜拍摄的每个 FOV 小图拼接在一起，可以查看整个组织形态的全景图像。在图像拍摄过程中可能存在：显微镜震动或者移动、实验室台面外部干扰或者拼接算法不佳等原因，可能会导致拼接的全景图像错位，对图像的准确性和可靠性造成影响。在**图像 QC** 的过程中，将每个拼接好的全景图像裁成不同的 FOV，评估相邻 FOV 重叠区域祥形态特征的相似性，确定拼接效果。
* **图像校准**：在一个芯片的多组图像数据中，每组图像的拼接、配准和组织区域范围必须保持严格的一致性才可以保证跟对应的表达矩阵对齐。由于染色条件和成像系统的限制，跟矩阵配准的图像仅在 DAPI 核染色图像中可见，因此必须保证其它 IF 图像首先跟 DAPI 图像对齐，然后使用相同的配准参数将各个图像与空间特征表达矩阵做配准，算法使用图像的相似性和特征偏移评估两张图像是否对齐。该评估指标仅适用于DAPI+mIF的单通道图像场景。

<table><thead><tr><th width="148">评估指标</th><th>分数</th><th>示例</th></tr></thead><tbody><tr><td><strong>Track线评估</strong></td><td><ul><li>≥ 60，算法检测到足够数量的合个的 track 线，图像可以与空间表达矩阵做自动配准。</li><li>&#x3C; 60，算法检测到较少的 track 线或者未检测出 track线，无法推导出全局模板，您可能需要重新拍照。</li></ul><p><em>QC 是否通过的必要条件。</em></p></td><td><img src="../../.img/trackline detection.png" alt="" data-size="original"></td></tr><tr><td><strong>图像清晰度</strong></td><td><p></p><ul><li> ≥ 80，图像的质量较好，算法有很大的概率可以准确识别或者分割细胞。</li><li>&#x3C; 80，SAW 内置的细胞分割算法自动分割的成功率可能较低，但并不一定意味着无法识别细胞。</li></ul><p><em>QC 中倾向于通过的指标。</em></p></td><td><img src="../../.img/image clarity.png" alt="" data-size="original"></td></tr><tr><td><strong>显微镜稳定性评估（拼接评估）</strong></td><td><ul><li>≥ 60，超过 30% 的 FOV 与相邻 FOV 的重叠区域具有清晰相似的特征，并且这些 FOV 的平均特征偏移分数大于 0.7。</li><li>&#x3C; 60，超过 30% 的 FOV 与相邻 FOV 的重叠区域具有模糊的特征，并且这些 FOV 的平均特征偏移分数小于 0.7。</li></ul><p><em>有条件的评估指标，只适用于 mIF 的小图。</em></p></td><td><img src="../../.img/stitching evaluation.png" alt="" data-size="original"></td></tr><tr><td><strong>校准评估</strong></td><td><ul><li>通过，核染色图像（DAPI）与另一图像(IF)之间的最大偏移量 ≤ 20 像素，且图像的形态相似度 ≥ 1%。</li><li>未通过，核染色图像（DAPI）与另一图像(IF)之间的最大偏移量 > 20 像素，或者图像的形态相似度 &#x3C; 1%。</li></ul><p> <em>有条件的评估指标，只适用于mIF。</em></p></td><td><img src="../../.img/image calibration.png" alt="" data-size="original"></td></tr></tbody></table>

备注：可以参考 **STOmics 显微镜评估参考手册**的 **4.3 章节图片示例**获取更多图像相关的信息**。**

## QC 操作指南

图像 QC 的操作界面如下：

<figure><img src="../../.img/image QC interface.png" alt=""><figcaption></figcaption></figure>

打开图像 QC 小工具，拖入待QC的文件，填写 **Chip SN**、**Operator**、**Image Path** 和 **Staining Types** 等信息**，**点击 **Run** 按钮，可开始进行图像质控的评估。

<table><thead><tr><th width="185">Image information</th><th>Description and example</th></tr></thead><tbody><tr><td><strong>Chip SN</strong></td><td>Stereo-seq 芯片序列号，可以在 Stereo-seq 芯片底部找到。<br><br><em>E.g. S1 (1x1) 芯片: SS200000135TL_D1, C02533C</em><br><em>S0.5 (0.5x0.5) 芯片: FP200009107_E414, B03210C211</em><br>大芯片<em>: S200000108BR_A3A4, D02070C3D3</em><br><em>请确保填写的 SN 跟实际拍摄图像的 SN 是对应的。</em></td></tr><tr><td><strong>Operator</strong></td><td>用户信息，建议输入拍摄人员的邮箱地址。</td></tr><tr><td><strong>Image Path</strong></td><td>待 QC 的文件目录，拖入后自动填充。<br><br><em>建议拖入的文件路径中不能包含字母、数字和下划线以外的任何字符。</em></td></tr><tr><td><strong>Staining Types</strong></td><td>图像的染色类型，包括 ssDNA，DAPI，DAPI+mIF和H&#x26;E。</td></tr><tr><td><strong>Upload</strong></td><td><p>将图像数据上传到 <a href="https://cloud.stomics.tech/">STOmcis Cloud</a> 或者用户自定义的路径，选项包括：</p><ul><li>No：不上传。</li><li>QC Input Files ：仅上传待 QC 的原始文件。</li><li>QC Output Files (TAR.GZ): 仅上传 QC 后的文件。</li><li>Select all: 上传待 QC 的原始文件及 QC 后的文件.</li></ul><p>可以查阅<a href="tu-xiang-zhi-kong-image-qc.md#shang-chuan-she-zhi">上传设置</a>获取更多信息。</p></td></tr><tr><td><strong>Remark</strong></td><td>输入图像或者 QC 的额外描述信息，用于记录各类不同的情况。</td></tr></tbody></table>

{% hint style="info" %}
若拍照时已经输入芯片号和拍图人员邮箱，这两栏会自动读取芯片号和拍图人员并显示，如果内容重新填写，则将覆盖拍照时输入的内容，此功能可用于修改拍照时的填写错误。
{% endhint %}

如果您输入的图像为 TIFF 格式的拼接大图，系统将提示您提供额外的显微镜详细信息，这些信息对于准确推导模板至关重要。如果无法获取如下显微镜配置的信息，您也可以直接使用系统推荐的默认值，但还是建议尽可能跟显微镜工程师沟通获取准确的信息。

<figure><img src="../../.img/image QC interface microscope info.png" alt=""><figcaption></figcaption></figure>

图像质控全流程可以分为两大步骤：**图像质控评估和图像压缩**。如果您选择了**上传**，会增加图像上传的进度及状态，图像成功上传也是您质控全流程中一个重要的部分。QC 全流程运行完成，界面会展示图像质控指标的状态、分数、耗时、评估结果及进一步分析的建议等信息。

<figure><img src="../../.img/image QC pass.png" alt=""><figcaption></figcaption></figure>

QC 运行完成后，可以在 **StereoMapWorkspace** -> **QC** 保存路径中查看 QC 后的的结果文件，[**保存路径**](../../xia-zai.md#bao-cun)可以在[**设置**](../../xia-zai.md#she-zhi)中更改。

<figure><img src="../../.img/image QC output (1).png" alt=""><figcaption></figcaption></figure>

如果您的图像数据是 QC 成功的，可以通过`--image-tar`参数，将 QC 输出的`.tar.gz`给到`SAW count`模块将图像数据跟矩阵数据一起分析。相反，如果您的图像数据 QC 未通过，又想做图像跟矩阵的可视化展示，需要将图像文件输入[Image Processing](../tu-xiang-chu-li-zhi-nan/) 做手动处理后再接回 SAW。

## QC 输入文件说明

### **核染色图像：ssDNA 或者 DAPI**

输入文件示例:

<table><thead><tr><th width="243">文件格式</th><th>示例</th></tr></thead><tbody><tr><td><code>.tif</code> /<code>.tiff</code> 拼接大图</td><td><img src="../../.img/nuclei-input tif.png" alt=""></td></tr><tr><td>QC 的输出文件<code>.tar.gz</code> </td><td><img src="../../.img/nuclei-input tar.png" alt=""></td></tr><tr><td><code>.tif</code>小图</td><td><p></p><ul><li>Motic:</li></ul><p><img src="../../.img/nuclei-input tiles-motic.png" alt=""></p><ul><li>STOmics Go Optical:</li></ul><p><img src="../../.img/spaces_KPjxR1Lv74t5QCTxNb8d_uploads_zfXtDODJw7v1l8YgOcbp_nuclei-input tiles-STOmicsGoOptical.webp" alt=""></p><ul><li>Leica:</li></ul><p><img src="../../.img/nuclei-input tiles-leica.png" alt=""></p></td></tr><tr><td>其它格式</td><td><img src="../../.img/nuclei-input czi.png" alt=""></td></tr></tbody></table>

核染色图像的评估主要基于 [Track 线评估](tu-xiang-zhi-kong-image-qc.md#qc-biao-zhun)和[图像清晰度](tu-xiang-zhi-kong-image-qc.md#qc-biao-zhun)，[track 线评估](tu-xiang-zhi-kong-image-qc.md#qc-biao-zhun) QC 是否通过的主要指标。

{% hint style="info" %}
通常情况下，一张 S1 的核染色图像（10 倍物镜，8 位深的`.tif`，约20,000 像素 x 20,000 像素）QC 的时长大约为 2.5 分钟。

如果图像较大或位深度为 16 位，则 QC 的处理时间会相对更长。
{% endhint %}

### **核染色+免疫荧光图像：DAPI+mIF**

输入文件示例：

{% hint style="info" %}
请将您的图像或图像文件夹按照如下规则重新命名，以便软件可以解析染色类型及 IF 的从名称。注意："<>"内是需要替换的信息。&#x20;

* DAPI: <芯片号>
* IF: <芯片号>\_\<IF名称>\_IF
{% endhint %}

<table><thead><tr><th width="258">文件格式</th><th width="486">示例</th></tr></thead><tbody><tr><td><code>.tif</code> /<code>.tiff</code> 拼接大图</td><td><img src="../../.img/mif-input tif.png" alt=""></td></tr><tr><td>QC 的输出文件<code>.tar.gz</code> </td><td><img src="../../.img/mif-input tar.png" alt=""></td></tr><tr><td><code>.tif</code>小图文件</td><td><p></p><ul><li>Motic:</li></ul><p><img src="../../.img/mif-input tiles-motic.png" alt=""></p><ul><li>STOmics Go Optical:</li></ul><p><img src="../../.img/mif-input tiles-STOmicsGoOptical.png" alt=""></p></td></tr><tr><td>其它格式</td><td><p></p><ul><li>Zeiss multi-page <code>.czi</code></li></ul><p><img src="../../.img/mif-input czi.png" alt=""></p><p></p></td></tr></tbody></table>

核染色+免疫荧光图像的评估主要基于：[track线评估](tu-xiang-zhi-kong-image-qc.md#qc-zhi-biao-ji-biao-zhun)、[图像清晰度评估](tu-xiang-zhi-kong-image-qc.md#qc-zhi-biao-ji-biao-zhun)、[拼接评估](tu-xiang-zhi-kong-image-qc.md#qc-zhi-biao-ji-biao-zhun)和[校准评估](tu-xiang-zhi-kong-image-qc.md#qc-zhi-biao-ji-biao-zhun)四个指标，除[图像清晰度](tu-xiang-zhi-kong-image-qc.md#qc-zhi-biao-ji-biao-zhun)的评估结果不做为 QC 是否通过的条件外，其它三个指标都是 QC 是否通过的主要指标。

{% hint style="info" %}
通常情况下，一张 S1 的核染色图像加二张IF图像（10 倍物镜，8 位深的`.tif`，约 20,000像素 x 20,000像素）QC 的时长大约为 3 分钟。

如果您输入的是小图文件或者 16 位深的图像，预期的时间将会更长，因为会额外增加解析的时长及[拼接评估](tu-xiang-zhi-kong-image-qc.md#qc-zhi-biao-ji-biao-zhun)，每张图像的[拼接评估](tu-xiang-zhi-kong-image-qc.md#qc-zhi-biao-ji-biao-zhun)大约为 2 分钟。
{% endhint %}

### **H&E图像：H&E**

输入文件示例：

<table><thead><tr><th width="258">文件格式</th><th width="489">示例</th></tr></thead><tbody><tr><td><code>.tif</code> /<code>.tiff</code> 拼接大图</td><td><img src="../../.img/HE-input tif.png" alt=""></td></tr><tr><td>QC 的输出文件<code>.tar.gz</code></td><td><img src="../../.img/HE-input tar.png" alt=""></td></tr><tr><td><code>.tif</code>小图文件</td><td><p></p><ul><li>Motic:</li></ul><p><img src="../../.img/HE-input tiles-motic.png" alt=""></p><ul><li>STOmics Go Spatial:</li></ul><p><img src="../../.img/mif-input tiles-STOmicsGoOptical (1).png" alt=""></p></td></tr><tr><td>其它格式</td><td><img src="../../.img/HE-input czi.png" alt=""></td></tr></tbody></table>

H&E 图像的评估主要是基于[ track 线评估](tu-xiang-zhi-kong-image-qc.md#qc-zhi-biao-ji-biao-zhun)，确保在显微镜下可以看到清晰的 track 线是至关重要的，对于算法来说，识别 H&E 彩色图像的 track 线比灰度图像具有更大的挑战。

{% hint style="info" %}
通常情况下，一张 S1 的核染色图像（10 倍物镜，48 位深的`.tif`，约 20,000 像素 x 20,000像素）QC 的时长大约为 4 分钟。

因 H&E 图像是一种彩色的图像，其大小约为灰度图像的三倍，因此图像解析时间将会更长。此外，如果您输入的数据为小图文件或者 QC 后的`.tar.gz`由于图像拼接和解析时间较长，预期的时间成本也会更高。
{% endhint %}

## 上传设置

主要配置 QC 后图像的上传方式，包括 HPC 或者云上传，如果对于上传方式有任何疑问，请随时与 FAS 沟通。

### 内置的上传路径

<div>

<figure><img src="../../.img/spaces_KPjxR1Lv74t5QCTxNb8d_uploads_RtufvbpnTA9gWe1PTuLV_qc upload settings (1).webp" alt=""><figcaption></figcaption></figure>

 

<figure><img src="../../.img/spaces_KPjxR1Lv74t5QCTxNb8d_uploads_1dZSOMVnefW5gaZpxuNf_qc upload type (1).webp" alt=""><figcaption></figcaption></figure>

 

<figure><img src="../../.img/qc upload region (1).png" alt=""><figcaption></figcaption></figure>

</div>

<table><thead><tr><th width="183">上传类型</th><th>描述</th><th>区域</th></tr></thead><tbody><tr><td>ALICLOUD</td><td><a href="https://cn.aliyun.com/">ALICLOUD</a> 为阿里云上传方式，适合将分析流程部署在阿里云的片区用户。如有其他新增片区，请联系总部 FAS，帮助更新后台配置表。</td><td><ul><li>SINGAPORE, AP</li></ul></td></tr><tr><td>AWS</td><td><a href="https://aws.amazon.com/cn/">AWS</a> 为亚马逊云上传方式，适合将分析流程部署在 AWS 云的片区用户。如有其他新增片区，请联系总部 FAS，帮助更新后台配置表。</td><td><ul><li>CALIFORNIA, US</li><li>RIGA, EU</li><li>SINGAPORE, AP</li></ul></td></tr><tr><td>HPC</td><td>HPC 为实验室内部网络的配置，直接上传到片区的集群，需要联系 FAS 提前做好网络配置，适合华大内网使用。</td><td><ul><li>CHONGQING, CN</li><li>SHENZHEN, CN</li></ul></td></tr><tr><td>RAYSYNC</td><td><a href="https://www.raysync.io/">RAYSYNC</a> 为镭速上传，连接互联网即可通过镭速上传到深圳集群无需做其他配置。</td><td><ul><li>CHONGQING, CN</li><li>QINGDAO, CN</li><li>SHENZHEN, CN</li></ul></td></tr></tbody></table>

### 自定义的上传路径

如果您自己购买了阿里云或 AWS 云服务，您可以通过设置自定义上传路径将图像传输到您的个人云存储中。

<figure><img src="../../.img/spaces_KPjxR1Lv74t5QCTxNb8d_uploads_1iiQclHP578OX2BVDlU3_qc upload custom (1).webp" alt=""><figcaption></figcaption></figure>

选择您购买的云服务类型 **ALICLOUD.CUSTOM** 或 **AWS.CUSTOM**，区域选择 **OWNER，**配置您的远程路径、keyID、密码和 S3 存储桶的名称。如果您的云服务类型是阿里云，需要填写域名信息。

<div>

<figure><img src="../../.img/spaces_KPjxR1Lv74t5QCTxNb8d_uploads_T1XREByuy6fhjwkMJBi1_qc upload custom info alicloud.webp" alt=""><figcaption></figcaption></figure>

 

<figure><img src="../../.img/spaces_KPjxR1Lv74t5QCTxNb8d_uploads_ihymmV5y4n4yYVS3kwEg_qc upload custom info aws.webp" alt=""><figcaption></figcaption></figure>

</div>

最后，点击 **Confirm** 配置完成，即可在在 **Upload information configuration** 窗口中找到自定义配置类型。

<div>

<figure><img src="../../.img/spaces_KPjxR1Lv74t5QCTxNb8d_uploads_8RTfnk66WyKoA4wPlkMW_qc upload custom a new cloud.webp" alt=""><figcaption></figcaption></figure>

 

<figure><img src="../../.img/qc upload custom a new cloud and save.png" alt=""><figcaption></figcaption></figure>

</div>
