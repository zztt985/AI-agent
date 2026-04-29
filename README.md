# AI 工程效能优化系统（Multi-Agent Engineering Copilot）

这是一个可运行的示例项目，用于演示“规划 Agent + 执行 Agent + 评审 Agent + RAG 记忆 + 工具调用”的 AI 工程效能闭环。

## 功能

- 需求拆解：PlannerAgent 将复杂研发需求拆成可执行步骤
- 代码分析：CodeSearchTool 检索项目代码
- 代码生成：ExecutorAgent 生成实现方案和代码补丁建议
- 自动评审：ReviewerAgent 做风险检测、测试建议和 Code Review
- 记忆检索：基于本地向量检索的轻量 RAG
- API 服务：FastAPI 提供 `/analyze` 接口
- 命令行：支持本地直接运行

## 目录结构

```text
ai_engineering_agents/
├── app/
│   ├── main.py
│   ├── agents/
│   ├── memory/
│   ├── schemas/
│   └── tools/
├── examples/
├── tests/
├── requirements.txt
├── .env.example
└── README.md
```

## 快速开始

```bash
cd ai_engineering_agents
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate
pip install -r requirements.txt
cp .env.example .env
```

### 命令行运行

```bash
python -m app.main --task "为用户登录增加验证码校验，并补充单元测试"
```

### API 运行

```bash
uvicorn app.main:app --reload
```

请求：

```bash
curl -X POST http://127.0.0.1:8000/analyze \
  -H "Content-Type: application/json" \
  -d '{"task":"为订单模块增加异常重试机制","repo_path":"./examples/sample_repo"}'
```

## OpenAI 接入

默认使用规则引擎模拟 Agent，保证开箱可跑。若要接入真实大模型，在 `.env` 中配置：

```env
OPENAI_API_KEY=你的key
LLM_PROVIDER=openai
OPENAI_MODEL=gpt-4.1-mini
```

## 说明

这是工程化模板，不会直接修改你的生产代码。真实落地时建议接入：

- GitHub/GitLab API 自动创建 PR
- CI/CD 执行测试
- SonarQube/CodeQL 安全扫描
- 企业知识库/文档库作为 RAG 数据源
