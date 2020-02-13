## 项目模版来源文档
###[集成方案-vue-element-admin](https://github.com/PanJiaChen/vue-element-admin)
###[基础模板: vue-admin-template](https://github.com/PanJiaChen/vue-admin-template) 

## Build Setup

```bash
# 克隆项目
git clone '待定'

# 进入项目
cd yanwei-web-merchant

# 安装全局依赖
npm install

# 开发运行项目
npm run dev
```


## Build（发布代码）

```bash
# 发布测试环境
npm run build:stage

# 发布线上环境
npm run build:prod
```

##页面结构

```bash
├── build                      # 构建相关
├── plop-templates             # 基本模板
├── public                     # 静态资源
│   │── favicon.ico            # favicon图标
│   └── index.html             # html模板
├── src                        # 源代码
│   ├── api                    # 所有请求
│   ├── assets                 # 主题 字体等静态资源
│   ├── gloablComponents       # 全局公用组件
│   ├── icons                  # 项目所有 svg icons
│   ├── layout                 # 全局 layout
│   ├── router                 # 路由
│   ├── store                  # 全局 store管理
│   ├── styles                 # 全局样式
│   ├── utils                  # 全局公用方法
│   ├── views                  # views 所有页面
│   ├── App.vue                # 入口页面
│   ├── main.js                # 入口文件 加载组件 初始
│   └── permission.js          # 权限管理
|   └── setting.js             # 页面基本配置
├── tests                      # 测试
├── .env.xxx                   # 环境变量配置
├── .eslintrc.js               # eslint 配置项
├── .babelrc                   # babel-loader 配置
├── .travis.yml                # 自动化CI配置
├── vue.config.js              # vue-cli 配置
├── postcss.config.js          # postcss 配置
└── package.json               # package.json

```

##页面布局layout
![Image text](https://raw.githubusercontent.com/dbk-bestFriend/image-resource/master/page.png)

## 开发注意事项
### 1.新增页面，新增路由，新增api 保持命名统一
![Image text](https://raw.githubusercontent.com/dbk-bestFriend/image-resource/master/iview.png)
![Image text](https://raw.githubusercontent.com/dbk-bestFriend/image-resource/master/route.png)
![Image text](https://raw.githubusercontent.com/dbk-bestFriend/image-resource/master/api.png)

###2.新增路由

```bash
export default {
  path: '/m_incentive',
  component: Layout,
  meta: { title: '激励方案' },
  name: 'mIncentive',
  redirect: 'noRedirect',
  children: [
    {
      path: 'qb-manage',
      name: 'qbManage',
      component: () => import('@/views/incentive/qb-manage.vue'),
      meta: { title: '签报管理' }
    }
  ]
}
```

####路由规则注意事项

```bash
redirect：‘noRedirect’        //设置为noRedirect面包屑不会被点击
name: 'router-name'          //设定路由的名字，一定要填写不然使用<keep-alive>时会出现各种问题
meta: {
  title: 'title'             //设置该路由在侧边栏和面包屑中展示的名字
  noCache: true              //如果设置为true，则不会被 <keep-alive> 缓存(默认 false)
}
```

### 3.新增页面，组件 模版方法
```bash
npm run new
```
![Image text](https://raw.githubusercontent.com/dbk-bestFriend/image-resource/master/xinzen.png)

### 3.全局资源的引用路径 @ > 'src'

##全局组件的自动注册
###register-components.js  每次新增组件无需手动注册，新增完组件根据组件name，可直接使用

```bash
import _ from 'lodash'
const registerComponents = {
  install(Vue) {
    const requireComponent = require.context(
      // 其组件目录的相对路径(组件目录相对于当前js文件的路径)
      '../gloablComponents',
      // 是否查询其子目录
      true,
      // 匹配基础组件文件名的正则表达式(因此要注册为全局组件的组件名称约定很重要)
      /(vue)$/
    )
    requireComponent.keys().forEach(fileName => {
      // 获取组件配置
      const componentConfig = requireComponent(fileName) // 这里的componentConfig包含当前fileName对应组件的所有该组件信息,等于拿到了当前组件实例
      // 获取组件的 PascalCase 命名
      const componentName = _.upperFirst( // 这里 _ 代表main.js中引入的的lodash实例对象
        _.camelCase(
          // 获取和目录深度无关的文件名
          fileName
            .split('/')
            .pop()
            .replace(/\.\w+$/, '') // 将.(包括.)字符以后的字符用''代替
        )
      )
      // 全局注册组件
      Vue.component(
        componentName,
        // 如果这个组件选项是通过 `export default` 导出的，
        // 那么就会优先使用 `.default`，
        // 否则回退到使用模块的根。
        componentConfig.default || componentConfig
      )
    })
  }
}

export default registerComponents


```


## 代码规范

####1.统一 eslint规范

```bash
// 禁止条件表达式中出现赋值操作符
"no-cond-assign":2,
// 禁用 console
"no-console":0,
// 禁止在条件中使用常量表达式
"no-dupe-keys":2,
// 禁止重复的 case 标签
"no-duplicate-case":2,
// 禁止空语句块
"no-empty":2,
// 禁止在正则表达式中使用空字符集 (/^abc[]/)
"no-empty-character-class":2,
// 禁止对 catch 子句的参数重新赋值
"no-ex-assign":2,
// 禁止不必要的布尔转换
```

####2.统一编辑软件（vscode）带有自动修复功能，能够hin好的辅助开发

####3.代码命名规范