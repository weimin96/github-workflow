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

### Release发布流程

1. [release-drafter.yml](https://github.com/weimin96/github-workflow/blob/main/.github/release-drafter.yml) 配置

👉[release drafter官方文档](https://github.com/marketplace/actions/release-drafter)

```yml
- title: 🚀 主要功能和改进
  labels:
    - 'feature'
    - 'enhancement'
- title: 🐛 Bug 修复
  labels:
    - 'fix'
    - 'bugfix'
    - 'bug'
- title: '🧰 维护'
  label: 'chore'
- title: 📝 文档更新
  labels:
    - 'documentation'
- title: 🧪 测试
  labels:
    - 'test'
- title: 📝 其他变更
```

以上标签需要与`pull request`时勾选的标签对应上

<img src="./docs/image/pr-labels.png" width="50%" alt="pr label">

2. 编写[ci脚本](https://github.com/weimin96/github-workflow/blob/main/.github/workflows/ci.yml)

**关键代码**

当推送tag时触发执行
```yaml
on:
  push:
    tags: [ v* ]
#    支持手动执行
  workflow_dispatch:
```

通过 release-drafter 插件自动收集提交记录和自动发布 release
```yaml
- name: Extract version from commit message
  run: |
    version=${GITHUB_REF/refs\/tags\/v/}
    echo "VERSION=$version" >> $GITHUB_ENV
  env:
    COMMIT_MSG: ${{ github.event.head_commit.message }}
- name: Publish release notes
  id: last_release
  uses: release-drafter/release-drafter@v5
  with:
    config-name: release-drafter.yml
    publish: true
    name: "v${{ env.VERSION }}"
    tag: "v${{ env.VERSION }}"
    version: "v${{ env.VERSION }}"
  env:
    GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

3. 配置权限

Actions=> General=>Workflow permisssion=> Read and Write permissions

<img src="./docs/image/setting-action-1.png" width="70%" alt="setting-action-1">

<img src="./docs/image/setting-action-2.png" width="70%" alt="setting-action-2">

3. 推送tag

执行命令
```bash
# e.g. git tag -a v1.2.0 -m "Release v1.2.0"
git tag -a [NEW_VERSION] -m "Release [NEW_VERSION]"
git push origin --tags
```

4. 等待构建完成后会在`Release`页面看到发布的新版本

### github-pages 设置

通过使用[docsify](https://docsify.js.org/#/) 来构建官方文档

1. 生成docs文件夹
```bash
npm i docsify-cli -g
docsify init ./docs 
```

2. 参考官方文档个性化配置

3. 配置github-pages

<img src="./docs/image/github-pages.png" width="70%" alt="github-pages">

4. 提交后等待片刻即可查看部署的网页

### 其他

自动生成文件，如：LICENSE、CONTRIBUTING.md、CODE_OF_CONDUCT.md 和 SECURITY.md
