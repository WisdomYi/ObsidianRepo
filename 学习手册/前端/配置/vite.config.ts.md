以下是一份详尽的 `vite.config.ts` 配置示例，包含开发服务器、构建、插件、CSS 处理等核心配置，并附带详细功能注释。适用于大多数前端项目（React/Vue/vanilla JS），可根据具体框架调整插件
```typescript
import { defineConfig, loadEnv } from 'vite';
import path from 'path';
import react from '@vitejs/plugin-react'; // React 项目插件
// import vue from '@vitejs/plugin-vue'; // Vue 项目插件（二选一）
import { visualizer } from 'rollup-plugin-visualizer'; // 构建体积分析插件
import { VitePWA } from 'vite-plugin-pwa'; // PWA 支持插件
import lightningcss from 'vite-plugin-lightningcss'; // Lightning CSS 插件
import eslint from 'vite-plugin-eslint'; // ESLint 集成插件

// 定义路径解析函数（简化别名配置）
const resolvePath = (relativePath: string) => path.resolve(__dirname, relativePath);

// Vite 配置导出（使用 defineConfig 获得类型提示）
export default defineConfig(({ mode }) => {
  // 加载环境变量（根据 mode 区分开发/生产环境）
  const env = loadEnv(mode, process.cwd(), '');

  return {
    // 1. 项目基础配置
    base: env.VITE_BASE_URL || '/', // 部署基础路径（如 GitHub Pages 可能需要 '/repo-name/'）
    mode: mode, // 环境模式（development/production/test）
    define: {
      // 注入全局常量（生产环境移除 console）
      __DEV__: mode === 'development',
      __API_URL__: JSON.stringify(env.VITE_API_URL || '/api'),
    },

    // 2. 开发服务器配置（npm run dev 时生效）
    server: {
      port: Number(env.VITE_PORT) || 5173, // 开发服务器端口
      host: env.VITE_HOST || 'localhost', // 允许外部访问（0.0.0.0 表示所有网卡）
      open: env.VITE_OPEN === 'true', // 自动打开浏览器
      https: env.VITE_HTTPS === 'true', // 是否启用 HTTPS
      cors: true, // 允许跨域请求
      strictPort: true, // 端口被占用时直接退出
      // 代理配置（解决开发环境跨域问题）
      proxy: {
        '/api': {
          target: env.VITE_PROXY_TARGET || 'http://localhost:3000',
          changeOrigin: true, // 跨域时修改 Origin
          rewrite: (path) => path.replace(/^\/api/, ''), // 移除路径中的 /api 前缀
        },
        '/ws': {
          target: env.VITE_WS_TARGET || 'ws://localhost:3000',
          ws: true, // 启用 WebSocket 代理
          changeOrigin: true,
        },
      },
      // 开发服务器中间件（可自定义请求处理）
      middleware: [(req, res, next) => {
        // 示例：在所有响应头添加 X-Powered-By
        res.setHeader('X-Powered-By', 'Vite');
        next();
      }],
    },

    // 3. 构建配置（npm run build 时生效）
    build: {
      outDir: env.VITE_OUT_DIR || 'dist', // 输出目录
      assetsDir: 'assets', // 静态资源目录（相对于 outDir）
      assetsInlineLimit: 4096, // 小于 4kb 的资源内联为 base64
      emptyOutDir: true, // 构建前清空输出目录
      sourcemap: mode !== 'production', // 生产环境不生成 sourcemap（减小体积）
      minify: mode === 'production' ? 'esbuild' : false, // 生产环境使用 esbuild 压缩
      // 目标浏览器兼容（通过 browserslist 或直接指定）
      target: ['es2020', 'edge88', 'firefox78', 'chrome87'],
      //  chunk 分割策略（优化缓存）
      rollupOptions: {
        input: {
          main: resolvePath('index.html'), // 主入口
          // 多页面应用可添加其他入口
          // admin: resolvePath('admin.html')
        },
        output: {
          // 静态资源命名（带哈希值，利于缓存）
          entryFileNames: 'js/[name].[hash].js',
          chunkFileNames: 'js/[name].[hash].js',
          assetFileNames: 'assets/[name].[hash].[ext]',
          // 分割代码（将 node_modules 依赖单独打包）
          manualChunks: {
            vendor: ['react', 'react-dom'], // React 相关依赖
            utils: ['lodash', 'date-fns'], // 工具库
          },
        },
        // 外部依赖（不打包进产物，运行时从外部获取）
        external: mode === 'production' ? ['some-large-library'] : [],
      },
      // 关闭生产环境的 console 和 debugger
      terserOptions: {
        compress: {
          drop_console: mode === 'production',
          drop_debugger: mode === 'production',
        },
      },
    },

    // 4. 预览服务器配置（npm run preview 时生效，用于预览构建产物）
    preview: {
      port: 5174,
      host: 'localhost',
      open: false,
    },

    // 5. 模块解析配置
    resolve: {
      // 路径别名（与 tsconfig.json 中的 paths 保持一致）
      alias: {
        '@': resolvePath('src'),
        '~': resolvePath('node_modules'),
        '@components': resolvePath('src/components'),
        '@utils': resolvePath('src/utils'),
      },
      // 导入时可省略的扩展名
      extensions: ['.ts', '.tsx', '.js', '.jsx', '.json', '.scss'],
    },

    // 6. CSS 处理配置
    css: {
      // 开启 CSS Modules（默认只对 *.module.* 文件名生效）
      modules: {
        scopeBehaviour: 'local', // 局部作用域
        generateScopedName: mode === 'development' 
          ? '[name]__[local]__[hash:base64:5]' 
          : '[hash:base64:8]', // 生产环境简化类名
        hashPrefix: 'prefix', // 哈希前缀（避免冲突）
      },
      // CSS 预处理器配置
      preprocessorOptions: {
        scss: {
          additionalData: `@import "@/styles/variables.scss";`, // 全局注入 SCSS 变量
        },
        less: {
          math: 'always', // Less 数学运算模式
          globalVars: { primary: '#1890ff' }, // Less 全局变量
        },
      },
      // 使用 Lightning CSS 替代 PostCSS（需安装 vite-plugin-lightningcss）
      transformer: 'lightningcss',
      lightningcss: {
        targets: { chrome: 90, firefox: 88 }, // 自动前缀目标浏览器
        minify: mode === 'production',
        features: {
          nesting: true, // 支持 CSS 嵌套
        },
      },
    },

    // 7. 插件配置（根据框架选择）
    plugins: [
      // React 插件（支持 JSX、HMR 等）
      react({
        jsxRuntime: 'automatic', // 自动导入 React
        babel: {
          plugins: ['@emotion/babel-plugin'], // 集成 Emotion CSS-in-JS
        },
      }),
      // Vue 插件（Vue 项目启用）
      // vue(),
      
      // ESLint 集成（开发时实时检查）
      eslint({
        exclude: ['node_modules/**', 'dist/**'],
        cache: true, // 缓存检查结果
      }),
      
      // 构建体积分析（仅生产环境启用）
      mode === 'production' && visualizer({
        filename: 'stats.html', // 生成分析报告
        open: false, // 不自动打开
      }),
      
      // PWA 支持（生成 service-worker）
      VitePWA({
        registerType: 'autoUpdate',
        manifest: {
          name: 'My App',
          short_name: 'App',
          start_url: '/',
          display: 'standalone',
          background_color: '#ffffff',
          theme_color: '#41b883',
          icons: [
            {
              src: 'icon-192x192.png',
              sizes: '192x192',
              type: 'image/png',
            },
          ],
        },
      }),
      
      // Lightning CSS 插件（增强 CSS 处理）
      lightningcss(),
    ].filter(Boolean), // 过滤掉 false 的插件（如生产环境才启用的插件）

    // 8. 依赖优化配置
    optimizeDeps: {
      include: ['react', 'react-dom', 'lodash-es'], // 强制预构建的依赖
      exclude: ['some-library-that-should-not-be-optimized'], // 不预构建的依赖
      esbuildOptions: {
        target: 'es2020', // 预构建的目标语法
      },
    },

    // 9. esbuild 配置（用于开发时的快速转换）
    esbuild: {
      jsxInject: `import React from 'react'`, // 自动导入 React（React 17 以下需要）
      // 开发时移除 console.log
      drop: mode === 'production' ? [] : ['console'],
      loader: {
        '.js': 'jsx', // 允许 JS 文件中使用 JSX
      },
    },

    // 10. 日志配置
    logLevel: mode === 'development' ? 'info' : 'warn', // 生产环境减少日志输出
    clearScreen: true, // 构建时清空控制台
  };
});
```