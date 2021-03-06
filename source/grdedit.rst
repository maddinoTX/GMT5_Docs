.. index:: ! grdedit

grdedit
=======

- 官方文档： :ref:`gmt:grdraster`
- 简介：修改网格文件的头段或内容

语法
----

``<grid>``
    要修改的网格文件

``-A``
    如有必要，则对网格间隔做微调使得其与数据的范围相兼容。

``-D<xname>/<yname>/<zname>/<scale>/<offset>/<invalid>/<title>/<remark>``
    修改网格文件的基本信息：

    - ``<xname>`` X变量名，格式为 ``varname [unit]`` ，比如 ``"distance [km]"``
    - ``<scale>`` 读入网格数据后要乘以的因子
    - ``<offset>`` 读入数据后并乘以因子后要加入的常数
    - ``<invalid>`` 无有效值的节点处的替代值，默认为NaN
    - ``<title>`` 网格文件的标题
    - ``<remark>`` 网格文件的注释信息

    若想要某些值不变，可直接将该字段留空，但 ``/`` 不可省略。若 ``/`` 是某个值的一部分，则可以用其他符号作为分隔符，此时需要在开头和结尾多加一个该符号，比如 ``-D:xname:yname:zname:scale:offset:invalid:title:remark:`` 。

    假设数据的范围是 ``300/310/10/30`` ，现修改数据的范围以及标题::

        gmt grdedit data.nc -R-60/-50/10/30 -D//////"Gravity Anomalies"

``-E[a|h|l|r|t|v]``
    对网格进行处理

    - ``-Ea`` 将网格在180度附近旋转（？）
    - ``-Eh`` 水平旋转网格（从左到右）
    - ``-El`` 逆时针将网格旋转90度
    - ``-Er`` 顺时针将网格旋转90度
    - ``-Et`` 对网格进行转置（想象成一个二维矩阵）
    - ``-Ev`` 垂直旋转网格（从上到下）

``-G<outgrid>``
    默认情况下，该命令会直接修改并覆盖原始网格文件，使用该选项则将修改后的网格写到新的文件中。

``-N<table>``
    从文件 ``<table>`` 中读入数据，并用其替换网格中对应节点的值。

``-R<w>/<e>/<s>/<n>``
    修改网格文件的范围。同时，网格间隔会做相应修改。

``-S``
    仅用于全球地理网格数据。将网格沿着经度范围整体偏移，使得其满足 ``-R`` 定义的新的范围。

    原数据范围是 ``0/360/-72/72`` ，现将数据整体偏移180度使得数据范围是 ``-180/180/-72/72`` ::

        gmt grdedit world.nc -R-180/180/-72/72 -S

``-T``
    修改网格文件的头段，将一个网格线配置的文件变成像素配准的文件，或反之。

    使用该选项后，网格线配准的数据的范围将在四个方向上扩大半个网格间隔，像素点配置的数据的范围将在四个方向上缩小半个网格间隔。

示例
----

假定数据范围是300/310/10/30，下面的命令将范围修改为-60/50/10/30，并为其加上标题::

    gmt grdedit data.nc -R-60/-50/10/30 -D//////"Gravity Anomalies"

网格范围为0/360/-72/72，移动数据，使得其范围为-180/180/-72/72::

    gmt grdedit world.nc -R-180/180/-72/72 -S

将网格数据逆时针旋转90度，并将旋转后的网格写到新网格文件中::

    gmt grdedit oblique.nc -El -Goblique_rot.nc
