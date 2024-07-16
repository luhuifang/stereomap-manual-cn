# Getting Started

## What is StereoMap?

StereoMap is a programming-free desktop application that allows you to interactively visualize and explore your Stereo-seq data from STOmics Products. You can use StereoMap to pinpoint the precise locations of genes within a tissue with subcellular resolution, discover molecular expression patterns for single or multiple genes in the original tissue section, and gain insights into the underlying biology.

A typical Stereo-seq data analysis workflow involves **StereoMap** at both upstream and downstream steps.

<figure><img src=".img/spaces_KPjxR1Lv74t5QCTxNb8d_uploads_qM4qgG1evqMMYJT4cGAw_stereo-seq_data_analysis_workflow.webp" alt=""><figcaption></figcaption></figure>

In the upstream, you may use **Image QC** tool and **Image Processing** tools to check whether the image can be automatically processed by SAW or needs to be manually processed before analyzing with spatial gene expression matrix.

## System Requirements

### Windows

<table><thead><tr><th></th><th width="210.33333333333331">Windows</th><th>Windows (Tools - QC only)</th></tr></thead><tbody><tr><td>Minimum Requirements</td><td><ul><li>Windows 10 (64-bit)</li><li>16 GB RAM</li><li>SSD storage highly recommended</li><li>100% scaling display setting recommended</li></ul></td><td><ul><li>Windows 10 (64-bit)</li><li>8 GB RAM</li><li>SSD storage highly recommended</li><li>100% scaling display setting recommended</li></ul></td></tr></tbody></table>

### Performance Notes

* When analyzing large datasets (100 k+ cells/bins), 32 GB RAM and a quad-core processor are highly recommended for optimal performance.
* When working with data on an external storage device, such as an external hard disk drive, the performance of the application may be risked by data transfer from the device, including slower I/O, heightened latency, and potential crash due to unexpected disconnection.&#x20;

## Installation

StereoMap for Windows is distributed as a self-installing executable (`.exe`) file. Double-click on the installation file to start installing.

<mark style="background-color:orange;">【安装包图标】</mark>

<mark style="background-color:orange;">【安装界面】</mark>

<mark style="background-color:orange;">【安装步骤注意事项1】</mark>

### Compatibility with SAW

* Highly recommend using SAW v8.0.
* Re-run **image QC** to convert the compressed TAR (`.tar.gz`) image file to the latest version.
