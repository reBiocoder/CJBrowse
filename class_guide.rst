经典快速入门指南
================

参考序列
***********

在加载注释数据之前，请使用 ``bin/prepare-refseqs.pl`` 格式化JBrowse的参考序列。
对于FASTA格式序列文件

::

    bin/prepare-refseqs.pl --fasta docs/tutorial/data_files/volvox.fa

对于已经存储在Bio :: DB :: *数据库中的序列，可以将prepare-refseqs.pl与biodb-to-json.pl配置结合使用

::

    bin/prepare-refseqs.pl --conf docs/tutorial/conf_files/volvox.json

基因组注释和特征
****************

JBrowse可以从平面文件或具有Bio :: DB :: * Perl接口的数据库中导入数据。

bin/flatfile-to-json.pl
********************

如果您有平面文件，例如GFF3或BED，通常最好使用bin / flatfile-to-json.pl导入它们。
bin / flatfile-to-json.pl接受许多不同的命令行设置，
这些设置可用于自定义新track的外观。
运行bin / flatfile-to-json.pl --help查看可用设置的描述。

::

    bin/flatfile-to-json.pl --gff path/to/my.gff3 --trackType CanvasFeatures --trackLabel mygff

bin/biodb-to-json.pl
*************************

如果您拥有基因组注释数据库，例如Chado，Bio :: DB :: SeqFeature :: Store或Bio :: DB :: GFF，则可以使用JBrowse的biodb-to-json.pl。
您将bin / biodb-to-json.pl与配置文件一起使用（其格式在此处记录）。

::

    bin/biodb-to-json.pl --conf docs/tutorial/conf_files/volvox.json

Next-gen reads (BAM)
***********************

JBrowse可以直接从BAM文件显示对齐方式，而无需进行预处理。
要使用该文件，请将BAM文件和BAM索引下载到您的数据文件夹中，
然后您可以将文本片段手动编辑到tracks.conf中:

::

    [tracks.alignments]
    storeClass = JBrowse/Store/SeqFeature/BAM
    urlTemplate = myfile.bam
    category = NGS
    type = JBrowse/View/Track/Alignments2
    key = BAM alignments from SEQ1654

然后,你需要在data文件夹中存在以下文件:

::

    data/myfile.bam
    data/myfile.bam.bai
    data/tracks.conf

在tracks.conf包含上述配置的地方，然后myfile.bam将相对于tracks.conf的位置进行定位，
因此由于它们位于同一目录中，因此不需要任何其他路径限定。 请注意，urlTemplate可以是完整的URL路径.

另请注意：BAM文件需要排序和索引（即具有对应的.bai文件或.csi文件。如果为.csi，则可以使用csiUrlTemplate。
如果为.bai，则将自动定位它 作为<yourbamfile.bam>，最后加上.bai）。

Next-gen read track类型
****************

JBrowse有两种主要的轨道类型，这些轨道类型专门设计用于BAM数据：

Alignments2
显示BAM文件中的各个比对，以及BAM的MD或CIGAR字段中编码的插入，删除，跳过的区域和SNP。

SNPCoverage
显示覆盖度直方图，彩色条显示碱基水平不匹配和读数中可能的SNP的位置。


要使用它们，请在轨道配置中设置type = Alignments2或type = SNPCoverage。

Index Names
********************

加载要素数据后，要让用户通过在自动完成搜索框中键入要素名称或ID来查找要素，必须使用 ``bin/generate-names.pl`` 生成要素名称的特殊索引。

::

    bin/generate-names.pl -v

注意：每次使用任何* -to-json.pl脚本向JBrowse添加新注释时，都需要重新运行 ``bin/generate-names.pl`` 将新功能名称添加到索引。

还要注意：bin / generate-names.pl索引的轨道类型包括:

* GFF，GBK，BED通过flatfile-to-json.pl加载。 默认情况下，对“ ID”，“名称”和“别名”建立索引。 请注意，--nameAttributes可用于索引其他字段
* Features from biodb-to-json.pl
* VCF tabix文件和GFF3 tabix文件（对它们从GFF的“ ID”和“名称”字段建立索引，并且仅对来自VCF的ID进行索引）

BAM的reads和BigWigs不会由generate-names.pl编制索引


定量轨道（BigWig和Wiggle）
****************

JBrowse可以直接从BigWig文件显示对齐方式，而无需进行预处理。 只需将带有文件相对URL的节添加到您的data / tracks.conf文件中，格式为：

::

    [ tracks.my-bigwig-track ]
    storeClass = JBrowse/Store/SeqFeature/BigWig
    urlTemplate = myfile.bw
    type = JBrowse/View/Track/Wiggle/XYPlot
    key = Coverage plot of NGS alignments from XYZ

JBrowse有两种专门用于定量数据的track类型：

* JBrowse/View/Track/Wiggle/XYPlot

将定量数据显示为条形图。 有关配置选项，请参见JBrowse Wiki。

* JBrowse/View/Track/Wiggle/Density

将定量数据显示为“热图”图，默认情况下，其具有正分数的区域绘制为逐渐变深的蓝色，而具有负分数的区域绘制为逐渐变深的红色。 有关配置选项，请参见JBrowse Wiki，包括如何更改颜色改变点（bicolor_pivot）和颜色。


要使用它们，请在轨道配置中设置type = JBrowse / View / Track / Wiggle / XYPlot或type = JBrowse / View / Track / Wiggle / Density。


结论
**************
祝您好运，并享受JBrowse！