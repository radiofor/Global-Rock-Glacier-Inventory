[English](README.md) | **中文**

# 全球岩石冰川清单数据集

本指南介绍国家青藏高原／第三极环境数据中心（TPDC）所发布清单的下载与使用方法。

- **数据 DOI：** https://doi.org/10.11888/Cryos.tpdc.303562
- **规模：** 301,524 个多边形，覆盖 19 个区域，总面积约 38,849 km²。
- **格式与坐标系：** GeoPackage（`.gpkg`），WGS 84 地理坐标系（EPSG:4326）。
- **数据包大小：** 490.98 MB。
- **指南更新日期：** 2026 年 10 月 2 日。

## 1 直接下载

1. 打开[数据页面](https://doi.org/10.11888/Cryos.tpdc.303562)。
2. 点击 **Download（下载）**，打开 **FTP account（FTP 账号）** 窗口。
3. 点击 **Download without login（不登录下载）**，启动直接下载。
4. 加载图标转圈期间保持网页打开。**页面不显示下载进度。**
5. 弹出保存窗口后保存压缩包；部分浏览器会自动保存到默认下载目录。
6. 解压压缩包，进入 `global_rgs_epsg4326` 文件夹。

## 2 通过 FTP 下载

1. 在数据页面点击下载。
2. 从 **FTP account** 窗口复制主机地址、端口、用户名和密码。
3. 在 FTP 客户端中选择 **FTP** 协议，填写页面提供的账号信息，将传输模式设为**被动模式（PASV）**，**不启用 TLS 加密**，然后连接服务器。
4. 按需选择全部文件或指定文件：`global_rgs.gpkg` 为全球清单，`global_rgs_XX.gpkg` 为区域子集，`global_rg_regions.gpkg` 为分区边界。区域编号及名称见[分区文件索引](#4-分区文件索引)。
5. 在 GIS 软件中直接打开下载的 GeoPackage。

直接下载可获取完整数据包；FTP 支持按需选择单个文件。

## 3 数据包结构

解压后的文件结构：

```text
全球石冰川编目数据集/
└── global_rgs_epsg4326/
    ├── global_rgs.gpkg
    ├── global_rg_regions.gpkg
    ├── global_rgs_01.gpkg
    ├── ...
    └── global_rgs_19.gpkg
```

| 文件 | 内容 |
| --- | --- |
| `global_rgs.gpkg` | 全球完整清单，301,524 个多边形 |
| `global_rgs_01.gpkg`–`global_rgs_19.gpkg` | 19 个区域子集，包含与全球文件一致的清单字段 |
| `global_rg_regions.gpkg` | 19 个分区边界要素，字段为 `fid`、`geom` 和 `name` |

每个文件包含一个要素图层，图层名为去掉 `.gpkg` 扩展名后的文件名。区域子集保留全球原始 `fid`、完整几何和属性。跨区域要素按最大相交面积归入一个区域，不在分区边界处裁切。

19 个分区完整覆盖全球清单，要素 ID 互不重复。分析时不要将全球文件与分区文件再次合并。

## 4 分区文件索引

区域名与边界文件一致，其中区域 15 的名称被截短。大小为解压后文件大小（1 MB = 1,000,000 字节）。

| 区域 ID | 数据中保存的区域名称 | 文件 | 多边形数量 | 解压后大小 MB |
| --- | --- | --- | ---: | ---: |
| 01 | Alaska | global_rgs_01.gpkg | 30312 | 23.76 |
| 02 | Western Canada and USA | global_rgs_02.gpkg | 41019 | 29.06 |
| 03 | Iceland | global_rgs_03.gpkg | 462 | 0.52 |
| 04 | Svalbard Archipelago | global_rgs_04.gpkg | 109 | 0.2 |
| 05 | Scandinavia | global_rgs_05.gpkg | 1693 | 1.5 |
| 06 | Severny Island | global_rgs_06.gpkg | 147 | 0.22 |
| 07 | Central Europe | global_rgs_07.gpkg | 8903 | 6.65 |
| 08 | Caucasus and Middle East | global_rgs_08.gpkg | 4292 | 3.56 |
| 09 | Central Asia | global_rgs_09.gpkg | 83541 | 69.02 |
| 10 | South Asia West | global_rgs_10.gpkg | 30208 | 27.23 |
| 11 | South Asia East | global_rgs_11.gpkg | 38917 | 35.09 |
| 12 | Southern Andes | global_rgs_12.gpkg | 7136 | 6.59 |
| 13 | New Zealand | global_rgs_13.gpkg | 413 | 0.38 |
| 14 | Western Mediterranean | global_rgs_14.gpkg | 37 | 0.13 |
| 15 | South Georgia and the South Sand | global_rgs_15.gpkg | 74 | 0.16 |
| 16 | Northern Andes | global_rgs_16.gpkg | 2297 | 1.82 |
| 17 | South Greenland | global_rgs_17.gpkg | 865 | 0.81 |
| 18 | North Asia | global_rgs_18.gpkg | 49055 | 37.13 |
| 19 | Arctic Canada South | global_rgs_19.gpkg | 2044 | 1.75 |

## 5 清单属性字段

| 字段 | 存储类型 | 单位 | 含义 |
| --- | --- | --- | --- |
| `fid` | 整数 | 无 | 要素标识，区域子集保留原始全球 ID |
| `geom` | MultiPolygon | WGS 84 坐标 | 岩石冰川多边形几何 |
| `area` | 实数 | km² | 在等面积坐标系 EPSG:8857 中计算的多边形面积 |
| `longitude` | 实数 | 十进制度 | WGS 84 质心经度 |
| `latitude` | 实数 | 十进制度 | WGS 84 质心纬度 |
| `elevation` | 实数 | 海拔 m | 多边形平均高程 |
| `slope` | 实数 | 度 | 多边形平均坡度 |
| `aspect` | 实数 | 方向类别 | 八方位坡向的众数类别；不是角度值 |
| `maat` | 实数 | °C | 多边形平均年平均气温 |
| `magt` | 实数 | °C | 多边形平均年平均地温 |
| `precipitation` | 实数 | mm/年 | 多边形平均年降水量 |
| `pzi` | 实数 | 无量纲 | 多边形平均多年冻土分区指数 |

缺失值为 **NULL**，不是零。`aspect` 是类别编码，不应直接求平均。

坡向编码从东方开始，按顺时针排列：

| 编码 | 方向 |
| --- | --- |
| 1 | 东（E） |
| 2 | 东南（SE） |
| 3 | 南（S） |
| 4 | 西南（SW） |
| 5 | 西（W） |
| 6 | 西北（NW） |
| 7 | 北（N） |
| 8 | 东北（NE） |

计算面积时使用 `area` 字段，或投影到等面积坐标系（如 EPSG:8857）。EPSG:4326 几何坐标的单位是度。

## 6 打开和使用数据

**QGIS 或 ArcGIS Pro：** 将所需 `.gpkg` 添加为矢量图层；加载 `global_rg_regions.gpkg` 查看分区边界。

**Python GeoPandas 示例：**

```python
from pathlib import Path
import geopandas as gpd

folder = Path("path/to/global_rgs_epsg4326")
rgs = gpd.read_file(folder / "global_rgs_01.gpkg", layer="global_rgs_01")
print(rgs.crs)                       # EPSG:4326
print(len(rgs))                      # 多边形数量
print(rgs["area"].sum())              # 面积总和，单位 km²
print(rgs[["maat", "magt"]].isna().sum())
```

部分读取器将 `fid` 作为索引而非属性列。导出或连接数据时应保留该标识。

## 7 数据制作与环境属性来源

清单主要利用高分辨率 Esri World Imagery，辅以 Google Satellite 影像。通过 UPerNet 和 Mask2Former 语义分割模型集成生成候选多边形，随后开展专家人工检查和修正。

环境属性为多边形范围内的栅格统计值，来源如下：

| 属性 | 来源 | 参考期或空间分辨率 |
| --- | --- | --- |
| 高程、坡度、坡向 | FABDEM；大高加索地区辅以 NASADEM | 30 m DEM |
| MAAT、降水 | CHELSA v2.1 气候平均值 | 1981–2010；30 角秒 |
| MAGT | UiO PEX–MAGT v5.0 | 2000–2016；南极洲为 2000–2017；1 km |
| PZI | 全球多年冻土分区指数 | 基于 1961–1990 年 MAAT；30 角秒 |

各属性的参考期和空间分辨率不同。该清单不是时间序列。

## 8 质量与解释限制

对 3,015 个随机抽样多边形的验证得到 **90.9% 的精确率**（Wilson 95% 置信区间：**89.8%–91.9%**）。该指标衡量已绘制要素的可靠性，不代表清单完整性或边界准确性。

清单适用于分布、面积和环境分析。活动状态、速度、含冰量和冰体积未包含在发布字段中，需要额外观测。

## 9 引用与数据使用

请按照 TPDC 记录引用数据：

> Xu, J., Feng, M., Su, Y., Yan, D., Wu, Q., Zhao, P., Zhang, X., and Li, X. (2026). A global rock glacier inventory dataset. National Tibetan Plateau / Third Pole Environment Data Center. https://doi.org/10.11888/Cryos.tpdc.303562

数据使用条款及联系方式见 TPDC 数据页面。
