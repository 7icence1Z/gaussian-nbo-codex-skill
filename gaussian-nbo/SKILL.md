---
name: gaussian-nbo
description: 配合 gaussian-dft-ultimate 调用 Gaussian 的 NBO/NPA 分析，识别内嵌或外部 NBO，解读给体与受体作用。
---

# Gaussian NBO 补充

先发现并读取已安装的 `gaussian-dft-ultimate`（Gauss-DFT-Ultimate），复用其准备、资源、运行及检查流程；缺失时说明依赖。这里只补充 NBO。

1. **能力**：只读检查版本、安装和已有输出。`l607` 可内嵌 NBO 3.1，二进制 `Gaussian NBO Version` 标记是线索，输出横幅才确认运行版本。搜不到 `nbo.exe` 不等于没有；测试脚本也不能证明安装。外部 NBO 6/7 需授权程序及兼容接口。仅查询时不运行或改配置。
2. **输入**：基础用 `Pop=NBO`；自定义用 `Pop=NBORead` 加 `$NBO ... $END`。按实际版本核对选项，3.1 不支持所有现代功能。复用 checkpoint 时保留原文件，使用绝对路径及独立输出，保持原方法、基组/ECP、环境：

```text
%oldchk=<absolute source.chk>
%chk=<absolute analysis.chk>
#p <original method/basis> Geom=AllCheck Guess=Read Pop=NBORead

$NBO NBO NPA $END

```

`Geom=AllCheck` 后不写标题、电荷、自旋或坐标；`Guess=Read` 仍执行 SCF。

3. **Windows**：按自带启动脚本核对调用；在作业目录用 `& $g16 $input $output`，路径运行时发现/传入，输入输出及 checkpoint/scratch 用绝对路径。为进程设置 `GAUSS_EXEDIR`、`GAUSS_SCRDIR`；不向 `g16.exe` 传 GUI 的 `-run -inp=...`。
4. **结果**：检查最终正常终止、SCF 和 NBO 输出；报告版本、NPA、占据数、给体→受体编号/类型及 E(2)/单位。金属体系检查 Lewis 结构和警告；E(2) 不是键能或 EDA 分量。保存 NBO/NLMO 可能覆盖规范 MO，应使用独立 checkpoint。

仅需核对版本或选项时查阅：[NBO 官方 FAQ](https://nbo6.chem.wisc.edu/faq_css.htm)、[Gaussian Pop](https://gaussian.com/population/)。ORCA 的 NBO 接口另需外部 NBO 6/7，不能借用 Gaussian 的 `l607`。
