---
title: .eslintrc.js
date: 2018-05-02 08:06:41
tags:
---

.eslintrc.js常用配置
<!-- more -->

```js
module.exports = {
    "env": {
        "browser": true,
        "node": true
    },
    "extends": "eslint:recommended",
    "parserOptions": {
        "ecmaVersion": 6
    },
    "rules": {
        "no-console": 0, //禁止使用console
        "default-case": 2, //switch语句最后必须有default
        "no-magic-numbers": [1, {
            "ignore": [0, -1, 1]
        }], //没有魔鬼数字, 0, 1, -1忽略
        "no-magic-numbers": 1, //没有魔鬼数字
        "prefer-const": 0, //首选const
        "no-const-assign": 2, //禁止修改const声明的变量
        "no-redeclare": 1, //禁止重复声明变量
        "no-undef": 2, //变量未定义
        "no-unused-vars": 1,
        "no-unused-vars": [1, {
            "vars": "all",
            "args": "after-used"
        }], //不能有声明后未被使用的变量或参数
        "no-use-before-define": 2, //未定义前不能使用
        "camelcase": 1, //驼峰命名法
        "newline-before-return": 1, //return前需要空行
        "no-constant-condition": 2, //禁止在条件中使用常量表达式 if(true) if(1)
        "no-debugger": 2, //禁止使用debugger
        "no-dupe-keys": 2, //在创建对象字面量时不允许键重复 {a:1,a:1}
        "no-dupe-args": 2, //函数参数不能重复
        "no-duplicate-case": 2, //switch中的case标签不能重复
        "no-trailing-spaces": 1, //一行结束后面不要有空格
        "no-unreachable": 2, //不能有无法执行的代码
        "no-unused-expressions": 2, //禁止无用的表达式
        "curly": [2, "all"], //必须使用 if(){} 中的{}
        "space-before-blocks": [0, "always"], //不以新行开始的块{前面要不要有空格
        "no-sparse-arrays": 2, //数组中多出逗号
        "no-shadow-restricted-names": 2, //关键词与命名冲突
        "no-mixed-spaces-and-tabs": 1,
        "no-irregular-whitespace": 1
    }
};
```





<!-- 统计访问该博客次数 -->
<div class="powered-by">
<i class="fa fa-user-md"></i><span id="busuanzi_container_site_uv">
  本站访客数:<span id="busuanzi_value_site_uv"></span>
</span>
</div>

<!-- 统计访问该博客总次数 -->
<span id="busuanzi_container_site_pv">
    本站总访问量<span id="busuanzi_value_site_pv"></span>次
</span>