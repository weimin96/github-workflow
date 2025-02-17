# github 工作流示例

本项目涵盖了`Github`上标准的团队开发流程和最佳实践，涵盖分支管理、版本控制、Issue/PR 处理、自动化工具配置等核心环节。

## 功能

### 分支管理

- `main`/`master`: 仅存放稳定生产环境代码，**禁止直接提交**，只能通过 PR 合并
- `develop`: 日常开发分支，所有功能分支合并到此分支
- `feature/*`: 新功能开发分支（例：`feature/user-auth`）
- `release/*`: 预发布分支（例：`release/v1.2.0`），用于测试和修复

### Issue 管理

**分类与模板**

- [bug报告模板](https://github.com/weimin96/github-workflow/blob/main/.github/ISSUE_TEMPLATE/bug-report.yml)
- [功能请求](https://github.com/weimin96/github-workflow/blob/main/.github/ISSUE_TEMPLATE/feature-request.md)

**自动关闭陈旧issue**

[Close stale issues工作流](https://github.com/weimin96/github-workflow/blob/main/.github/workflowsE/close-stale-issues.yml)

### PR 流程

- 使用 PR 模板（创建 [.github/PULL_REQUEST_TEMPLATE.md](https://github.com/weimin96/github-workflow/tree/main/.github/PULL_REQUEST_TEMPLATE.md)）
- 通过 CI/CD 检查（测试、代码规范）
- 至少 1 名 Reviewer 批准

### 分支保护规则

### 工作流

- CI脚本
- 自动发布
- 合并changelog

### 发布新版本

```bash
git tag -a v1.2.0 -m "Release v1.2.0"  # 语义化版本（SemVer）
git push origin --tags
```

### 其他

自动生成文件，如：LICENSE、CONTRIBUTING.md、CODE_OF_CONDUCT.md 和 SECURITY.md