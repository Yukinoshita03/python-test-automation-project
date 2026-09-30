# Python Test Automation Project

基于 Python、Requests 和 pytest 的接口自动化测试项目。用 YAML 定义请求、参数、响应提取和断言，再由 pytest 组织执行；提供 CSV/XML/Excel 数据工具、数据库适配器和 Allure 报告入口。

公开版保留完整的框架和测试源码，配置与用例数据使用模板或合成样例。仓库不包含被测业务后端，接入前需要填写自己的测试环境配置，并按接口契约调整用例。

## 提交历史说明

完整源码最初以本地文件保存，开发月份为作者回忆的 2026 年 9 月，没有阶段性旧版本或原始逐日 Git 记录。

`main` 展示按项目依赖顺序组织的 **30 天事后重建快照**，日期范围为 **2026-09-01 至 2026-09-30**。作者日期和提交者日期都使用该重建日程；实际重建和发布发生在 **2026-10-14**。每条提交以 `reconstruct:` 标识，并在正文注明实际重建日期。

原来的 7 条真实导入记录保存在 [codex/original-import-20261014](https://github.com/Yukinoshita03/python-test-automation-project/commits/codex/original-import-20261014/)，包含早期组件仓库的 2026-09-20 提交 `b078b922` 和 2026-10-14 的模块导入。

| 重建日程 | 阶段 |
| --- | --- |
| 09-01—09-06 | 工程结构、依赖、配置与日志 |
| 09-07—09-11 | 数据读取、合成数据与数据库适配 |
| 09-12—09-16 | 请求封装、断言、通知与基础工具 |
| 09-17—09-19 | 单接口执行器、业务执行器与可选 GUI |
| 09-20—09-25 | 业务、产品和用户接口 YAML 场景 |
| 09-26—09-29 | pytest 夹具、测试模块与运行入口 |
| 09-30 | 使用文档与完整项目说明 |

原代码中的非敏感作者和来源注释保留；仓库暂未指定开源许可证。

## 目录

- `pythonproject/base/`：请求用例执行、变量替换、响应抽取和用例生成工具。
- `pythonproject/common/`：请求、YAML/CSV/XML/Excel、日志、断言和可选外部系统适配器。
- `pythonproject/conf/`：运行设置和 `config.example.ini` 模板。真实 `config.ini` 不进入仓库。
- `pythonproject/data/`：合成 CSV/XML/YAML 数据；原始业务 XLS 未导入。
- `pythonproject/testcase/`：YAML 用例、测试模块和夹具。
- `pythonproject/run.py`、`pythonproject/pytest.ini`：本地入口和 pytest 收集配置。

请求执行流程：

```text
YAML 用例 → pytest 参数化测试 → RequestBase → 请求发送 → 响应提取与断言
                                     ↓
                              日志 / 报告附件
```

## 配置和外部连接

从仓库根目录创建环境并准备本地配置：

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r pythonproject/requirements.txt
cd pythonproject
cp conf/config.example.ini conf/config.ini
cp environment.example.xml environment.xml
export API_TEST_CONFIG_FILE="$PWD/conf/config.ini"
```

填写本地 `conf/config.ini`，并根据目标 API 调整合成用例后，可执行 `python -m pytest testcase`。运行时生成的 `extract.yaml`、日志、报告与本地配置已加入忽略规则。

`run.py` 的 Allure 路径还需要 `allure-pytest` 插件与 Allure 命令行工具；`tm` 路径需要对应的报告插件。原始依赖声明保留，安装和这些报告路径尚未在此次导入中验证。

Jenkins、邮件和钉钉集成默认关闭：分别需要显式设置 `API_TEST_JENKINS_ENABLED=1`、`API_TEST_EMAIL_ENABLED=1`、`API_TEST_DD_MSG=1`，并自行提供对应配置。SSH、数据库和 HTTP 请求只有在用户显式调用测试或适配器时才会发生。

## 验证边界

测试需要用户自己的 API、数据库和其他外部服务；没有这些服务，测试会失败或无法完成初始化。本次整理只做静态结构检查：所有保留 Python 文件使用 `ast.parse`，并对 YAML、XML、INI 做静态解析。没有在本次导入中运行 pytest、发起接口请求、连接数据库/SSH/Jenkins、发送邮件/钉钉通知，也没有声称业务用例通过。
