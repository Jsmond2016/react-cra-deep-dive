# start.js 文件功能文档

## 1. 环境初始化
- 设置开发环境变量：`BABEL_ENV=development`, `NODE_ENV=development`
- 加载环境配置：通过 `require('../config/env')` 加载 .env 文件
- 错误处理：将未处理的 Promise 拒绝转换为错误抛出

## 2. 文件检查与验证
- **必需文件检查**：验证 `public/index.html` 和 `src/index.js` 是否存在
- **浏览器兼容性检查**：通过 `checkBrowsers` 验证项目配置的浏览器支持

## 3. 端口与网络配置
- **默认端口**：3000
- **主机配置**：支持通过 `HOST` 环境变量自定义 (默认 0.0.0.0)
- **端口冲突处理**：使用 `choosePort` 自动寻找可用端口
- **协议选择**：根据 `HTTPS` 环境变量选择 http/https

## 4. Webpack 配置流程
1. **获取配置**：`configFactory('development')` 获取开发环境 webpack 配置
2. **创建编译器**：`createCompiler` 创建带自定义消息的 webpack 编译器
3. **代理配置**：读取 `package.json` 中的 proxy 配置并准备代理
4. **服务器配置**：合并 `webpackDevServer.config.js` 中的开发服务器配置

## 5. 开发服务器启动
- **启动命令**：`devServer.startCallback()`
- **自动操作**：
  - 清屏（交互式终端）
  - 检查 React 版本兼容性（Fast Refresh 需要 React ≥16.10.0）
  - 自动打开浏览器访问本地地址

## 6. 进程管理
- **信号处理**：监听 `SIGINT` 和 `SIGTERM` 信号优雅关闭服务器
- **CI 模式**：非 CI 环境下监听 stdin 结束事件

## 7. 关键依赖
- **必需文件**：`public/index.html`, `src/index.js`
- **配置文件**：`package.json`（代理配置）, `tsconfig.json`（TypeScript）
- **工具**：`webpack`, `webpack-dev-server`, `react-dev-utils`

## 8. 使用方式
```bash
npm start              # 基本使用
PORT=3001 npm start    # 自定义端口
HOST=localhost npm start  # 自定义主机
HTTPS=true npm start   # 启用 HTTPS
```

该文件是 Create React App 开发模式的完整启动流程，负责从环境检查到服务器启动的全生命周期管理。