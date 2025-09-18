
```json
{
  // 顶层配置：项目范围与基础行为
  "extends": "@tsconfig/strictest/tsconfig.json", // 继承社区严格预设（可选）
  "compileOnSave": false, // 关闭IDE保存时自动编译
  "include": [
    "src/**/*.ts", 
    "src/**/*.tsx", 
    "typings/**/*.d.ts" // 包含需要编译的文件
  ],
  "exclude": [
    "node_modules", 
    "dist", 
    "**/*.test.ts" // 排除不需要编译的文件
  ],
  "files": [
    "src/index.ts" // 显式指定核心文件（优先级高于include）
  ],
  "references": [
    { "path": "./packages/utils" }, // monorepo项目引用（指定依赖子项目）
    { "path": "./packages/core" }
  ],

  // 核心编译选项
  "compilerOptions": {
    // 1. 目标环境与语法转换
    "target": "ES2022", // 编译后JS版本（ES3/ES5/ES2020/ESNext等）
    "module": "NodeNext", // 模块系统（CommonJS/ES6/NodeNext等）
    "lib": ["ES2022", "DOM", "DOM.Iterable"], // 依赖的库文件（如DOM API、ES特性）
    "jsx": "react-jsx", // JSX处理方式（preserve/react/react-jsx等）
    "jsxImportSource": "@emotion/react", // JSX导入源（用于CSS-in-JS库）
    "moduleResolution": "NodeNext", // 模块解析策略（Node/Classic）
    "downlevelIteration": true, // 为ES5/ES3目标环境降级迭代器（如for...of）

    // 2. 输出控制
    "outDir": "dist", // 编译产物输出目录
    "rootDir": "src", // 源码根目录（用于保持目录结构）
    "outFile": "dist/bundle.js", // 合并输出为单个文件（仅支持特定module）
    "sourceMap": true, // 生成源映射文件（.map）
    "inlineSourceMap": false, // 不将源映射嵌入JS文件
    "sourceRoot": "", // 源映射中调试代码的根路径
    "mapRoot": "", // 源映射文件的输出路径
    "declaration": true, // 生成类型声明文件（.d.ts）
    "declarationDir": "dist/types", // 类型声明文件输出目录
    "declarationMap": true, // 为.d.ts生成源映射
    "emitDeclarationOnly": false, // 仅生成声明文件，不生成JS
    "removeComments": false, // 不删除编译后JS中的注释
    "noEmit": false, // 不输出编译产物（仅类型检查）
    "noEmitOnError": true, // 有错误时不输出产物
    "importHelpers": true, // 从tslib导入辅助函数（减少重复代码）
    "tsBuildInfoFile": ".tsbuildinfo", // 增量编译缓存文件

    // 3. 严格模式（strict:true会开启所有子选项）
    "strict": true, // 总开关：开启严格类型检查
    "noImplicitAny": true, // 禁止隐式any类型
    "strictNullChecks": true, // 严格检查null/undefined
    "strictFunctionTypes": true, // 严格检查函数参数类型兼容性
    "strictBindCallApply": true, // 严格检查bind/call/apply参数
    "strictPropertyInitialization": true, // 类属性必须初始化
    "useUnknownInCatchVariables": true, // catch变量默认为unknown类型
    "alwaysStrict": true, // 在输出JS中添加"use strict"

    // 4. 模块解析与路径
    "baseUrl": ".", // 模块解析基础路径
    "paths": { // 路径别名配置
      "@/*": ["src/*"],
      "~/*": ["node_modules/*"]
    },
    "rootDirs": ["src", "generated"], // 虚拟合并多个源码目录
    "typeRoots": ["node_modules/@types", "typings"], // 类型声明文件目录
    "types": ["node", "jest"], // 自动引入的类型声明（如node/jest）
    "allowUmdGlobalAccess": true, // 允许从模块中访问UMD全局变量
    "resolveJsonModule": true, // 允许导入JSON文件
    "allowImportingTsExtensions": false, // 禁止导入时带.ts扩展名
    "moduleSuffixes": [".ts", ".tsx", ".js"], // 模块解析后缀优先级

    // 5. JavaScript兼容
    "allowJs": false, // 不允许编译JS文件
    "checkJs": false, // 不检查JS文件类型
    "maxNodeModuleJsDepth": 1, // 检查node_modules中JS文件的深度

    // 6. 类型检查规则
    "esModuleInterop": true, // 允许ES模块与CommonJS互操作
    "preserveSymlinks": false, // 不保留符号链接的原始路径
    "forceConsistentCasingInFileNames": true, // 强制文件名大小写一致
    "noFallthroughCasesInSwitch": true, // 禁止switch语句中case穿透
    "noImplicitReturns": true, // 禁止函数分支未返回值
    "noUnusedLocals": true, // 禁止未使用的局部变量
    "noUnusedParameters": true, // 禁止未使用的函数参数
    "noUncheckedIndexedAccess": true, // 严格检查数组/对象索引访问
    "noImplicitOverride": true, // 禁止隐式重写父类方法（需用override关键字）
    "noPropertyAccessFromIndexSignature": true, // 禁止从索引签名访问属性
    "allowUnusedLabels": false, // 禁止未使用的标签（如循环标签）
    "allowUnreachableCode": false, // 禁止不可达代码（如return后的代码）

    // 7. 实验性特性（需谨慎使用）
    "experimentalDecorators": true, // 启用装饰器
    "emitDecoratorMetadata": true, // 为装饰器生成元数据
    "useDefineForClassFields": true, // 类字段使用ES标准定义（declare）
    "moduleDetection": "force", // 强制模块检测规则
    "verbatimModuleSyntax": false, // 保留模块语法（不自动转换）
    "exactOptionalPropertyTypes": true, // 严格检查可选属性类型
    "isolatedModules": true, // 每个文件作为独立模块（如Vite/ESBuild要求）
    "skipLibCheck": true // 跳过对.d.ts文件的类型检查（加速编译）
  }
}
```


