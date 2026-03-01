1. Add Ruff and Jupyter

```bash
uv add --dev ruff pre-commit
uv add --dev jupyter
uv add --dev ipykernel
```

2. Edit `pyproject.toml`

```toml
[tool.ruff]
line-length = 100
target-version = "py311"
# 让 ruff 也处理 notebooks（可选但推荐）
include = ["*.py", "*.ipynb"]

[tool.ruff.lint]
# E/F: 基础错误；I: import 排序；B: bugbear 常见坑；UP: py upgrade
select = ["E", "F", "I", "B", "UP"]
fixable = ["ALL"]

[tool.ruff.format]
quote-style = "double"
indent-style = "space"
```

3. 运行从而确定ruff正常

```bash
uv run ruff check --fix .
uv run ruff format .
```

4. 配置pre-commit，让每次commit自动检查/修复

```yaml
repos:
  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.6.9
    hooks:
      - id: ruff
        args: ["--fix"]
      - id: ruff-format
```

5. 安装git hook

因为 git 默认没有 hook，你必须手动安装，所以必须执行一次：

```bash
uv run pre-commit install
```

6. 第一次全量跑一下

```bash
uv run pre-commit run --all-files
```

以后每次git commit前都会自动修复

7. 推荐的.gitignore

```.gitignore
# Python
__pycache__/
*.pyc

# venv
.venv/

# Jupyter
.ipynb_checkpoints/

# Ruff cache
.ruff_cache/

# VS Code
.vscode/

# macOS
.DS_Store
```

8. 最常用的命令

```bash
# 运行你的代码
uv run python your_script.py

# 修复 + 格式化全项目
uv run ruff check --fix .
uv run ruff format .

# 提交前手动跑一遍（可选）
uv run pre-commit run --all-files
```





