`tsconfig.node.json` 通常用于 Node.js 环境的 TypeScript 配置（如后端服务、CLI 工具等），与前端配置的核心区别在于 **不依赖 DOM 环境**、**模块解析适配 Node.js 规则**。以下是包含详细注释的完整配置示例

```json
{
  // 顶层配置：Node.js 项目范围定义
  "extends": "@tsconfig/node18/tsconfig.json", // 继承 Node.js 18 环境的社区预设
  "compileOnSave": false, // 关闭 IDE 保存自动编译
  "include": [
    "src/**/*.ts", // 包含所有 TypeScript 源文件
    "scripts/**/*.ts", // 包含脚本文件（如构建脚本）
    "typings/**/*.d.ts", // 包含自定义类型声明
    "*.config.ts" // 包含项目配置文件（如 vite.config.ts）
  ],
  "exclude": [
    "node_modules", // 排除 node_modules
    "dist", // 排除输出目录
    "**/*.test.ts", // 排除测试文件
    "**/*.spec.ts"
  ],
  "references": [
    { "path": "./packages/server" }, // monorepo 中子项目依赖（Node.js 服务）
    { "path": "./packages/cli" } // monorepo 中子项目依赖（CLI 工具）
  ],

  "compilerOptions": {
    // 1. 目标环境（Node.js 特性适配）
    "target": "ES2022", // 编译目标 JS 版本（匹配 Node.js 18+ 支持的 ES 特性）
    "module": "NodeNext", // 模块系统（NodeNext 支持 Node.js 的 package.json "type": "module"）
    "lib": ["ES2022"], // 依赖库（Node.js 环境无需 DOM，仅需 ES 标准库）
    "moduleResolution": "NodeNext", // 模块解析策略（与 Node.js 解析规则一致）
    "resolveJsonModule": true, // 允许导入 JSON 文件（Node.js 常见需求）
    "allowUmdGlobalAccess": false, // 禁止访问 UMD 全局变量（Node.js 中很少使用）

    // 2. 输出控制（Node.js 项目结构）
    "outDir": "dist", // 编译产物输出目录
    "rootDir": ".", // 源码根目录（包含 src、scripts 等）
    "rootDirs": ["src", "scripts"], // 虚拟合并多源码目录（方便模块引用）
    "sourceMap": true, // 生成源映射（Node.js 调试需要）
    "inlineSourceMap": false, // 不嵌入源映射到 JS 文件
    "declaration": true, // 生成类型声明文件（.d.ts，用于 Node.js 库开发）
    "declarationDir": "dist/types", // 类型声明输出目录
    "declarationMap": true, // 为 .d.ts 生成源映射（方便 IDE 跳转）
    "emitDeclarationOnly": false, // 不仅生成声明文件，同时生成 JS
    "removeComments": false, // 保留注释（Node.js 脚本可能依赖注释说明）
    "noEmit": false, // 启用输出（Node.js 项目需要运行编译产物）
    "noEmitOnError": true, // 有错误时不输出产物（避免运行错误代码）
    "tsBuildInfoFile": ".tsbuildinfo", // 增量编译缓存（加速 Node.js 项目编译）

    // 3. 严格模式（Node.js 类型安全）
    "strict": true, // 开启所有严格检查（推荐 Node.js 项目启用）
    "noImplicitAny": true, // 禁止隐式 any 类型（避免类型丢失）
    "strictNullChecks": true, // 严格检查 null/undefined（Node.js 中常见的错误来源）
    "strictFunctionTypes": true, // 严格检查函数参数类型兼容性
    "strictBindCallApply": true, // 严格检查 bind/call/apply（Node.js 回调常用）
    "strictPropertyInitialization": true, // 类属性必须初始化（避免运行时 undefined）
    "useUnknownInCatchVariables": true, // catch 变量默认为 unknown（强制类型判断）
    "alwaysStrict": true, // 输出 JS 带 "use strict"（Node.js 严格模式）

    // 4. 模块路径与类型（Node.js 特有配置）
    "baseUrl": ".", // 模块解析基础路径
    "paths": { // 路径别名（Node.js 项目常用）
      "@/*": ["src/*"],
      "@scripts/*": ["scripts/*"],
      "@types/*": ["typings/*"]
    },
    "typeRoots": [
      "node_modules/@types", // 第三方类型声明
      "typings" // 自定义 Node.js 类型（如扩展 global）
    ],
    "types": [
      "node", // 引入 Node.js 内置类型（必须）
      "jest", // 若使用 Jest 测试（可选）
      "reflect-metadata" // 若使用装饰器（可选）
    ],
    "moduleSuffixes": [".ts", ".mts", ".cts", ".js", ".mjs", ".cjs"], // Node.js 模块后缀优先级

    // 5. 代码质量与兼容性（Node.js 环境适配）
    "esModuleInterop": true, // 允许 ES 模块与 CommonJS 互操作（Node.js 混合模块必备）
    "allowSyntheticDefaultImports": true, // 允许默认导入无默认导出的模块（如很多 CommonJS 库）
    "preserveSymlinks": false, // 不保留符号链接（Node.js 模块解析默认行为）
    "forceConsistentCasingInFileNames": true, // 强制文件名大小写一致（避免跨系统问题）
    "skipLibCheck": true, // 跳过 .d.ts 检查（加速 Node.js 项目编译，第三方类型可能有冲突）

    // 6. 错误检查规则（Node.js 代码规范）
    "noFallthroughCasesInSwitch": true, // 禁止 switch 穿透（避免逻辑错误）
    "noImplicitReturns": true, // 禁止函数分支未返回值（Node.js 回调需确保返回）
    "noUnusedLocals": true, // 禁止未使用的局部变量（清理冗余代码）
    "noUnusedParameters": true, // 禁止未使用的函数参数（Node.js 回调常忽略参数，需显式标注 _）
    "noUncheckedIndexedAccess": true, // 严格检查对象索引访问（避免 Node.js 中 undefined 错误）
    "noImplicitOverride": true, // 禁止隐式重写父类方法（需用 override 关键字）

    // 7. 实验性特性（Node.js 可能用到的高级特性）
    "experimentalDecorators": true, // 启用装饰器（如 NestJS 等框架需要）
    "emitDecoratorMetadata": true, // 生成装饰器元数据（配合 reflect-metadata 使用）
    "useDefineForClassFields": true, // 类字段使用 ES 标准定义（符合 Node.js 对 ES 标准的支持）
    "isolatedModules": true, // 每个文件作为独立模块（避免 Node.js 模块解析歧义）
    "verbatimModuleSyntax": false, // 不强制保留模块语法（Node.js 自动处理）
    "exactOptionalPropertyTypes": true // 严格检查可选属性（避免 undefined 赋值错误）
  }
}
```
