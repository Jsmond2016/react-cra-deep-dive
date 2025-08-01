# test.js 文件功能文档

## 1. 环境初始化
- **设置测试环境变量**：
  - `BABEL_ENV=test`
  - `NODE_ENV=test`
  - `PUBLIC_URL=''` (空字符串，避免测试时路径问题)
- **错误处理**：将未处理的Promise拒绝转换为错误抛出
- **加载环境配置**：通过 `require('../config/env')` 加载测试环境变量

## 2. 版本控制检测
- **Git仓库检测**：通过 `git rev-parse --is-inside-work-tree` 命令
- **Mercurial仓库检测**：通过 `hg --cwd . root` 命令
- **目的**：确定项目是否处于版本控制环境中

## 3. Jest 配置与执行
- **参数处理**：获取命令行参数 `process.argv.slice(2)`
- **智能监听模式**：
  - 非CI环境且未指定 `--watchAll` 时自动选择监听模式
  - 在版本控制环境中使用 `--watch` 模式（仅监听变更文件）
  - 无版本控制时使用 `--watchAll` 模式（监听所有文件）
- **Jest执行**：通过 `jest.run(argv)` 启动测试运行器

## 4. 使用场景
- **本地开发**：自动启用监听模式，提高开发效率
- **CI环境**：禁用监听模式，一次性运行所有测试
- **手动控制**：支持通过命令行参数覆盖默认行为

## 5. 命令行使用示例
```bash
npm test                    # 默认行为，根据环境选择监听模式
npm test -- --watchAll      # 强制监听所有文件
npm test -- --watchAll=false # 禁用监听模式
CI=true npm test           # CI环境下运行
```

该文件是 Create React App 的测试入口脚本，负责设置测试环境并智能选择最适合的 Jest 运行模式。