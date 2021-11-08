Vue3 的文档地址: https://v3.vuejs.org/

## 配置 vue3 开发环境

[Vue cli](https://cli.vuejs.org/zh/)

```bash
// 安装或者升级
npm install -g @vue/cli
# OR
yarn global add @vue/cli

// 保证 vue cli 版本在 4.5.0 以上
vue --version

// 创建项目
vue create my-project
```

然后的步骤

- Please pick a preset - 选择 **Manually select features**
- Check the features needed for your project - 多选择上 **TypeScript**，特别注意点空格是选择，点回车是下一步
- Choose a version of Vue.js that you want to start the project with - 选择 **3.x (Preview)**
- Use class-style component syntax - 输入 **n**，回车
- Use Babel alongside TypeScript - 输入**n**，回车
- Pick a linter / formatter config - 直接回车
- Pick additional lint features - 直接回车
- Where do you prefer placing config for Babel, ESLint, etc.? - 直接回车
- Save this as a preset for future projects? - 输入**n**，回车

启动图形化界面创建

```shell
vue ui
```

## 项目结构和插件

推荐给大家安装的插件

**[Eslint 插件](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)**

如果 eslint 不生效，可以在根目录创建 .vscode 文件夹，然后在文件夹中创建 settings.json 然后输入

```json
{
  "eslint.validate": [
    "typescript"
  ]
}
```

**[Vetur 插件](https://marketplace.visualstudio.com/items?itemName=octref.vetur)**