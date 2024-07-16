# 下载

## StereoMap 4.0.0 (2024年6月)

[时空官网下载链接](https://www.stomics.tech/products/BioinfoTools/OfflineSoftware)

{% hint style="info" %}
下载 “StereoMap 4.0.0 exe” 软件安装包，下载过程中如果被操作系统拦截，点击“保留”，将安装包添加到信任中。

![](img/automatically blocks.png)
{% endhint %}

### 更新亮点

* ImageStudio 软件跟 StereoMap 合并，可以在一个软件中完成图像质控、图像手动处理和可视化的展示。
* 图像手动处理更新为 step by step 的推荐步骤进行，降低用户的学习成本。
* 新增`.stereo`文件即可展示可视化的分析数据。
* 支持用户手动圈选的感兴趣区域归集到自定义的某个组中。
* 支持将差异分析的参数导出给 SAW 运行差异分析，并将结果导入可视化展示。

[版本更新说明->](fa-ban-shuo-ming/stereomap-fa-ban-shuo-ming.md)

***

## 系统要求

### Windows

<table><thead><tr><th width="160"></th><th width="258">Windows</th><th width="236">Windows（仅小工具-QC）</th></tr></thead><tbody><tr><td>最低配置</td><td><p></p><ul><li>Windows 10或更高的版本（64 位） </li><li>内存：16 GB</li><li>强烈建议使用 SSD 固态硬盘 </li><li>建议缩放设置为 100%</li></ul></td><td><p></p><ul><li>Windows 10或更高的版本（64 位） </li><li>内存：8 GB</li><li>强烈建议使用 SSD 固态硬盘 </li><li>建议缩放设置为 100%</li></ul></td></tr></tbody></table>

### 性能说明

* 如果待分析的数据集（比如 100 k+ 的 cells/bins）较大时，强烈建议使用四核处理器，32GB 及以上的内存，性能方面应该会有较好的体验。
* 如果输入的数据存储在外部（例如外部硬盘）设备时，软件的性能可能会因设备的传输受到影响，包括 I/O 读写变慢或者延迟，连接意外断开等导致软件报错。

***

## 软件安装

下载`.exe`文件，双击可执行文件开始安装。

<div align="left">

<figure><img src="img/image (110).png" alt="" width="76"><figcaption></figcaption></figure>

</div>

选择安装路径，默认为"D:\StereoMap"，可根据需要修改目标文件夹。点击 **Install** 开始安装。

<figure><img src="img/installing.png" alt=""><figcaption></figcaption></figure>

安装完成后，点击 **Finish** 即可。

<figure><img src="img/spaces_q9nLCBaEQA96hC5FzYjG_uploads_pJO9D88tpz6oGPObCuCn_installation done.webp" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
首次安装或者在线更新，请确保您的安装目录必须满足至少 20GB 的可用磁盘空间。
{% endhint %}

{% hint style="info" %}
请确保安装路径中未包含其它特殊字符或者符号。当前支持的字符包括：空格、下划线、字母A\~Z和数字0\~9。
{% endhint %}

### 相关软件（SAW）兼容

* SAW >= v8.0。
* 请使用最新的 [Image QC](tutorials/xiao-gong-ju-zhi-nan/#tu-xiang-zhi-kong) 重新QC，生成 TAR`(.tar.gz)`图像文件。

***

## 设置

### 在线更新

点击右上角的设置<img src="img/image (152).png" alt="" data-size="line">，选择  **About** ，联网状态下可以手动检查版本信息，如果有新版本发布，您可以选择下载并安装新的软件包。

<figure><img src="img/setting-about (1).png" alt=""><figcaption></figcaption></figure>

您也可以勾选软件开机时自动检查最新版本的选项，程序会自动检测是否有新版本的更新。

<figure><img src="img/setting-general-autoupdate.png" alt=""><figcaption></figcaption></figure>

### 保存

设置每个模块输出文件的路径，默认是您的安装目录，您也可以根据需求更改。

<figure><img src="img/image (111).png" alt=""><figcaption></figcaption></figure>

### 显微镜信息配置

{% hint style="info" %}
**提示：**建议在做图像 QC 时，保存您的显微镜配置信息方便后续使用。
{% endhint %}

设置显微镜图像的详细信息，该信息会在 QC 评估中使用到，您也可以点击 **Edit/Add** 按钮新增或者编辑配置信息。

<div>

<figure><img src="img/microscope settings.png" alt=""><figcaption></figcaption></figure>

 

<figure><img src="img/microscope settings-add_edit.png" alt=""><figcaption></figcaption></figure>

</div>

您可以新增<img src="img/image (156).png" alt="" data-size="line">、修改<img src="img/image (157).png" alt="" data-size="line"> 和删除<img src="img/image (158).png" alt="" data-size="line">已有的显微镜配置信息，参数说明如下：

* **Config Name**：显微镜配准的名称。
* **Manufacture**：显微镜品牌的名称。
* **Model**：显微镜的型号。
* **Objective**：拍摄时显微镜的放大倍数。
* **Overlap**：相邻两个FOV拍照时的重叠比例。
* **Scale**：拍摄图像的比例，单位为μm/pixel（微米每像素），请至少输入三位小数。
* **FOV Width**：拍摄时的一个FOV的宽度。
* **FOV Height**：拍摄时的一个FOV的高度。
* **Set as Default**: 是否设置为默认的显微镜配置。

<figure><img src="img/add microscope configuration.png" alt=""><figcaption></figcaption></figure>

点击 **Confirm** 按钮保存编辑的配置。

<figure><img src="img/2 microscope configurations.png" alt=""><figcaption></figcaption></figure>

另外，在[图像处理](tutorials/tu-xiang-chu-li-zhi-nan/)的第一步中，如果输入的是TIFF大图，您也可以增加、编辑或者删除该配置信息。

<div>

<figure><img src="img/tif uploading IP.png" alt=""><figcaption></figcaption></figure>

 

<figure><img src="img/microscope config IP.png" alt=""><figcaption></figcaption></figure>

</div>
