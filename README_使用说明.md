# load_power_cf - final

正式模型：v0.5 population + NLCD。

- 人口：2018 LandScan，县内归一化。
- NLCD：2018 Annual NLCD，提取 developed impervious area，并使用正向 NLCD 超额项。
- 县级权重：98% population + 2% positive NLCD excess。
- 空间分辨率：ERA5 坐标中心对齐的 0.25° 网格。
- 负荷数据：OEDI county hourly HDF5，目标年份 2016–2023。

本目录只放正式代码、配置说明和空的数据/缓存/输出目录；不复制历史 NetCDF、validation 或实验版本。

## 目录

```text
code/       正式运行代码
data/       放置 population、land_cover、boundaries、historic load 数据
cache/      运行时生成的边界、NLCD 和州级权重缓存
output/     运行后生成 NetCDF；初始为空
```

`data/land_cover/` 下每个年份必须放三个 NLCD 图层，三者并列：

```text
data/land_cover/
├─ Annual_NLCD_FctImp_2018_CU_C1V2/
│  └─ Annual_NLCD_FctImp_2018_CU_C1V2.tif
├─ Annual_NLCD_ImpDsc_2018_CU_C1V2/
│  └─ Annual_NLCD_ImpDsc_2018_CU_C1V2.tif
└─ Annual_NLCD_LndCov_2018_CU_C1V2/
   └─ Annual_NLCD_LndCov_2018_CU_C1V2.tif
```

2015–2022 每一年都按同样结构放置 `FctImp`、`ImpDsc` 和 `LndCov`；例如 2019 负荷使用 2018 年的这三个文件。

运行入口为 `code/run_final_2016_2023.py`。它会循环处理 2016–2023 年：负荷年份 Y 自动使用 Y-1 年的人口和 NLCD（2016 使用 2015，……，2023 使用 2022），并固定使用 2023 harmonized county-equivalent boundaries。运行前请把各年份数据按代码中的文件名放入 `data`。

示例：

```powershell
Set-Location -LiteralPath "D:\desktop\load_power_cf - final"
python code\run_final_2016_2023.py
```
