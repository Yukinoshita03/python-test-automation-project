# Python API 自动化测试框架基础组件

这个仓库保留原项目的通用框架代码，供后续自行编写和组织测试用例。当前公开版本不含业务测试用例、pytest 运行入口、真实环境配置、运行报告或日志。

## 保留内容

- `pythonproject/base/`：HTTP/API 调用封装、ID 生成及测试数据辅助工具。
- `pythonproject/common/`：断言、数据库连接、数据文件读写、日志、请求发送和 LLM 错误分析等通用能力。
- `pythonproject/conf/`：框架配置读取逻辑；`config.ini` 需由使用者本地创建。
- `pythonproject/requirements.txt`：项目依赖清单。

含有原环境 webhook 凭据或内部 Jenkins 地址的专用通知/集成模块未发布。测试用例、测试初始化与运行脚本由项目维护者后续补充。

## 本地准备

```bash
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r pythonproject/requirements.txt
cp pythonproject/conf/config.example.ini pythonproject/conf/config.ini
```

然后按本机环境编辑 `pythonproject/conf/config.ini`。此文件已加入 `.gitignore`，不要把账号、密码、令牌或真实服务地址提交到仓库。

本仓库不包含测试用例、`conftest.py`、`pytest.ini` 或 `run.py`；请按自己的项目需求补充测试和启动配置。
