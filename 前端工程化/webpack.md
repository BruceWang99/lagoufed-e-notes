



### webpack(大而全)

> 适合应用程序开发中使用

#### webpack配置文件

##### 局部命令式配置打包

* 修改入口文件

  ```javascript
  npx webpack --entry ./src/main.js
  ```

* 修改打包后的文件目录

  ```javascript
  npx webpack --entry ./src/main.js --output-path ./build
  ```



##### webpack.config.js文件配置打包

```javascript
const path = require('path')

module.exports = {
	entry: './src/main.js', // 入口文件
	output: { // 输出文件
		filename: 'build.js', // 输出文件文件名
		// __dirname 总是指向被执行 js 文件的绝对路径, 这里是/webpack-demo/
		// path.resolve 从右向左构建一个绝对路径
		path: path.resolve(__dirname, './dist'), // 输出文件存放的路径
	}
}
```

* 修改webpack默认配置文件

  ```javascript
  // 把webpack.config.js改成lg.webpack.js
  webpack --config lg.webpack.js
  ```

#### CSS-loader

* 行内loader

  ```javascript
  // @import 和 url() 进行处理，就像 js 解析 import/require() 一样。但是不能把样式放到界面上使用
  import 'css-loader!../css/login.css'
  ```

* 配置文件

  ```javascript
  module.exports = {
  	module: { // 放置模块的匹配规则等
  		rules: [
  			// {
  			// 	test: /\.css$/,// 一般是一个正则表达式, 用来匹配文件类型
  			// 	use: [
  			// 		{
  			// 			loader: 'css-loader',
  			// 			// options: 
  			// 		}
  			// 	]
  			// },
  			// {
  			// 	test: /\.css$/,// 一般是一个正则表达式, 用来匹配文件类型
  			// 	loader: 'css-loader'
  			// },
  			{
  				test: /\.css$/,// 一般是一个正则表达式, 用来匹配文件类型
  				use: ['css-loader']
  			},
  		]
  	}
  }
  ```

  

#### style-loader

> 会生成一个style标签, 把对应的css添加进来

```javascript
module.exports = {
	module: { // 放置模块的匹配规则等
		rules: [
			// {
			// 	test: /\.css$/,// 一般是一个正则表达式, 用来匹配文件类型
			// 	use: [
			// 		{
			// 			loader: 'css-loader',
			// 			// options: 
			// 		}
			// 	]
			// },
			// {
			// 	test: /\.css$/,// 一般是一个正则表达式, 用来匹配文件类型
			// 	loader: 'css-loader'
			// },
			{
				test: /\.css$/,// 一般是一个正则表达式, 用来匹配文件类型
				// style-loader 会生成一个style标签, 把对应的css添加进来
				use: ['style-loader', 'css-loader'] // loader 从右往左运行
			},
		]
	}
}
```

#### less-loader

> less-loader 会把less转成css, 要结合 less、css-loader、style-loader一起使用

```
{
				test: /\.less$/,// 一般是一个正则表达式, 用来匹配文件类型
				// less-loader 会把less转成css
				use: ['style-loader', 'css-loader', 'less-loader'] // loader 从右往左运行
			},
```

#### browserslistrc

> 兼容性: CSS JS
> 如果实现: babel、postcss
> canuse.com

">1%"使用率大于1%的浏览器

"default" 默认设置

"last 2 version" 浏览器最新的几个版本

"no dead" 没有死掉的浏览器, 24个月没更新是死了

* package.json里面设置

  ```javascript
  {
    "browserslist": [
      "ie 9"
    ]
  }
  ```

  

* .browserslistrc

  ```javascript
  >1%
  last 2 version
  no dead
  ```

  

  

#### postcss

> 利用javascript转化css样式的工具

1. 告诉它浏览器平台
2. 安装postcss
3. 安装autoprefixer添加css前缀

##### autoprefixer

> 添加css前缀

##### post-preset-env

> 预设 -- 插件集合

##### postcss统一配置: postcss.config.js

```javascript
module.exports = {
	plugins:[
		require('postcss-preset-env')
	]
}
```

#### importLoaders

> 到引入的css文件, 再次从上一个loader处理下

```javascript
{
				test: /\.css$/,// 一般是一个正则表达式, 用来匹配文件类型
				// style-loader 会生成一个style标签, 把对应的css添加进来
				use: [
					'style-loader', 
					{
						loader: 'css-loader',
						options: {
							importLoaders: 1 // 碰到引入的css文件, 再次从上一个loader处理下, 这里是postcss-loader
						}
					},
					'postcss-loader'
				] // loader 从右往左运行
```

#### file-loader

> 将资源拷贝到指定目录, 分开请求

###### 打包 img src

* 使用 require() 导入图片, 此时如果不配置 esModule: false, 则需.default导出

  ```javascript
  oImg.src = require('../img/WechatIMG1674.jpeg').default
  ```

  ```
  oImg.src = require('../img/WechatIMG1674.jpeg')
  // webpack配置中
  {
    test: /\.(png|svg|gif|jpeg)$/,
    use: [{
     	loader: 'file-loader',
     	options: {
     		esModule: false // 不转成esModule
     	}
     }]
  }
  ```
  * 采用 import xxx from 图片资源, 此时可以直接使用 xxx

###### 打包背景图片

css-loader设置 esModule为false
file-loaderr设置 esModule为false

```javascript
{
				test: /\.css$/,// 一般是一个正则表达式, 用来匹配文件类型
				// style-loader 会生成一个style标签, 把对应的css添加进来
				use: [
					'style-loader', 
					{
						loader: 'css-loader', // // @import 和 url() 进行处理，就像 js 解析 import/require() 一样。但是不能把样式放到界面上使用
						options: {
							importLoaders: 1, // 碰到引入的css文件, 再次从上一个loader处理下, 这里是postcss-loader
							esModule: false // 不转成esModule

						}
					},
					'postcss-loader'
				] // loader 从右往左运行
			},
			{
				test: /\.(png|svg|gif|jpeg)$/,
				use: [{
					loader: 'file-loader',
					options: {
						esModule: false // 不转成esModule
					}
				}]
				// use: ['file-loader']
			}
```

###### 自定义文件名: name:

[ext]: 扩展名
[ name ]: 文件名
[ hash ]: 文件内容
[contentHash]: 文件内容

###### 自定义输出目录

```javascript
outputPath: 'img'
```

#### url-loader

> 把要打包的文件打包成base64 url

* 减少了请求次数

* 也可以调用file-loader
* limit 属性控制使用url-loader还是file-loader

#### asset 处理图片

* asset/resource ->file-loader

  ```javascript
  {
  				test: /\.(png|svg|gif|jpeg)$/,
  				type: 'asset/resource',
  				generator: {
  					filename: 'img/[name].[hash:6][ext]'
  				}
  }
  ```

  

  * asset/inline -> url-loader

    ```javascript
    {
    				test: /\.(png|svg|gif|jpeg)$/,
    				type: 'asset/inline',
    }
    ```

    

* asset/source -> raw-loader

* asset -> 综合最实用

  ```javascript
  {
  				test: /\.(png|svg|gif|jpeg)$/,
  				type: 'asset',
  				generator: {
  					filename: 'img/[name].[hash:6][ext]'
  				},
  				parser: {
  					dataUrlCondition: {
  						maxSize: 1024 * 1 // 小于这个值, 就是base64 url
  					}
  				}
  }
  ```

  ###### asset打包字体文件

  ```javascript
  {
  				test: /\.(ttf|woff2?)$/,
  				type: 'asset/resource',
  				generator: {
  					filename: 'font/[name].[hash:3][ext]'
  				},
  }
  ```

  #### babel-loader

  > 配置.browserslistrc(浏览器兼容性)、安装@babel/core、@babel/preset-env

  ```javascript
  {
    test: /\.js$/,
    use: {
      loader: 'babel-loader',
      options: { // 局部配置babel-loader
        presets: [
          ['@babel/preset-env', {
          targets: 'chrome 91'
        }]
        ]
      }
    }
  }
  ```

  Babel-loader全局配置文件(**建议使用**):babel.config.js

  ```javascript
  module.exports = {
  	// presets: [
  	// 	['@babel/preset-env', {
  	// 	targets: 'chrome 91' // 兼容的目标浏览器, 就近原则, 这个会生效, 会影响 .browserslistrc
  	//   }]
  	// ]
  	presets: [
  		['@babel/preset-env']
  	]
  }
  ```

  

  #### webpack插件

  Loader: 转换 特定类型

  Plugin: 更多的事情 ( 压缩、生成html、清除文件、资源拷贝等)

      #### clean-webpack-plugin

> 清除dist

```javascript
const { CleanWebpackPlugin } = require('clean-webpack-plugin')
module.exports = {
plugins: [// 插件
		new CleanWebpackPlugin()
	]
}

```

#### html-webpack-plugin

> 生成 html

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin')

plugins: [// 插件
		new HtmlWebpackPlugin({
			title: '学习webpack',
			template: './public/index.html'
}),
```

#### DefinePlugin

> 定义一个在编译时可以配置的全局常量

```javascript
const { DefinePlugin} = require('webpack')
plugins: [// 插件
		new DefinePlugin({ // 允许创建一个在编译时可以配置的全局常量
			BASE_URL: "'./'" // 把所赋的值原封不动的给常量, 如果希望是字符串, 就再包一层
		})
	]
```

#### cope-webpack-plugin

> 复制文件

```javascript
// 注意webpack版本和CopyWebpackPlugin版本, 有出现报错情况
const CopyWebpackPlugin = require('copy-webpack-plugin')
new CopyWebpackPlugin({
			patterns: [
				{
					from: 'public',
					globOptions: {
						ignore: ['**/index.html']
					}
				}
			]
}),
```

#### polyfill

> 打补丁,做填充方面的兼容,  promise、map、set、async等

安装core-js、regenerator-runtime这两个依赖
core-js/stable: 核心填充ES的兼容代码
regenerator-runtime: 生成器函数、async、await函数经babel编译后，regenerator-runtime模块用于提供功能实现。

```javascript
// babel.config.js
module.exports = {
	presets: [
		[
			'@babel/preset-env',
			{
				// useBuiltIns: false // 不对当前js做polyfill填充
				// useBuiltIns: 'usage', // 依据用户源代码使用的新语法做填充
				useBuiltIns: 'entry', // 根据当前配置的兼容浏览器去填充
				corejs: 3 // 默认版本是2, 看你安装的版本
		    }
	    ]
	]
}
// useBuiltIns: 'entry'时, 需要手动引入两个依赖包
import "core-js/stable";
import "regenerator-runtime";
```

#### webpack-dev-server

* Watch + live server(观察模式)

  ```javascript
  // webpack.config.js
  module.exports = {
  	watch: true,
  }
  或者package.json script
  webpack --config lg.webpack.js --watch
  ```

不足

1. 所有源代码都会重新编译
2. 每次编译成功都需要文件读写
3. live server 是vs code插件
4. 不能局部刷新

webpack-dev-server没有上述问题

```javascript
// npm i webpack-dev-server -D
// package.json scripts
"serve": "webpack serve --config lg.webpack.js"
```

##### devServer常用配置

- publicPath: 指定本地服务所在目录

  > output中的publicPath 最好和devServer中的publicPath配置成一样, 这样访问页面的时候不容易出错

  ```javascript
  output: { // 输出文件
  		publicPath: '/lg' // index.html内部的引用路径 域名 + publicPath + filename
  },
  devServer: {
  		hot: true, // 热更新
  		static: {
  			directory: path.join(__dirname, 'public'), // 告诉服务器从哪里提供内容。只有在你希望提供静态文件时才需要这样做
  			publicPath: '/lg', // 指定本地服务所在目录
  			watch: true, // 监听directory中的文件, 触发整个页面重新加载。
  		},
  },
  ```

  其他配置

  ```javascript
  		open: true, // 运行时, 自动打开浏览器tāb页面
  		hot: 'only', // 热更新
  		port: 9999, // 端口
  		compress: true, // gzip资源压缩
  		historyApiFallback: true, // 当路径访问404时, 用index.html 替代, 这时候前端自己的路由就能正常
      proxy: { // 代理服务器
  			'/api': { // 代理路径
  				target: 'https://api.github.com', // 目标代理路径
  				pathRewrite: { // 重写路径
  					"^/api" : ""
  				},
  				changeOrigin: true // 改变源
  			}
  		
  ```

  

#### Webpack-dev-middleware

> 可以做打包服务做自由度更高的定制

![image-20211129215938104](https://bluenote.oss-cn-shanghai.aliyuncs.com/img/20211129215938.png)

#### HMR功能使用

> 模块热替换功能

```javascript
module.exports = {
	devServer: { // 默认是刷新页面
		hot: true
	},
}
// 局部刷新不影响页面, 需要在js中配置
if(module.hot) {
	module.hot.accept(['./this_module.js'], ()=>{
		console.log('模块更新了')
	})
}

```

* 热替换如果代码错误的话, 会触发自动刷新页面
  可以使用hotOnly: true

#### resolve 模块解析规则

```javascript
resolve: { // 解析规则
		extensions: [".js", ".json", '.ts', '.jsx', '.vue'], // 这些后缀名, 没有添加后缀时, 做解析
		alias: {
			'@': path.resolve(__dirname, 'src') // 设置解析别名
		}
}
```

#### source map

> 映射--》 在调试时, 定位到源代码的信息

```javascript
mode: 'development',
devtool: 'source-map'
```

#### devtool

* eval
  每个模块都使用 `eval()` 执行，并且都有 `//@ sourceURL`。此选项会非常快地构建。主要缺点是，由于会映射到转换后的代码，而不是映射到原始代码（没有从 loader 中获取 source map），所以不能正确的显示行数。

* source-map
   整个 source map 作为一个单独的文件生成。它为 bundle 添加了一个引用注释，以便开发工具知道在哪里可以找到它。

* eval-source-map
  每个模块使用 `eval()` 执行，并且 source map 转换为 DataUrl 后添加到 `eval()` 中。初始化 source map 时比较慢，但是会在重新构建时提供比较快的速度，并且生成实际的文件。行数能够正确映射，因为会映射到原始代码中。它会生成用于开发环境的最佳品质的 source map。

* inline-source-map
  source map 转换为 DataUrl 后添加到 bundle 中。

* cheap-source-map
   没有列映射(column mapping)的 source map，忽略 loader source map。

* hidden-source-map

   与 `source-map` 相同，但不会为 bundle 添加引用注释。如果你只想 source map 映射那些源自错误报告的错误堆栈跟踪信息，但不想为浏览器开发工具暴露你的 source map，这个选项会很有用。
   
   ![image-20211212210452586](https://bluenote.oss-cn-shanghai.aliyuncs.com/img/20211212210454.png)

​	eval - 是否使用eval 执行模块代码
​	cheap - Source Map 是否包含行信息, 没有列的信息
​	module - 是否能够得到Loader处理之前的源代码

​	开发环境:
​	cheap-module-eval-source-map: 
​	一般代码行字符不会很多, 没有列的信息也可以定位错误-cheap
​	使用框架打包后和打包前区别很大, -module
   热更新速度比较快

​	生产环境:
​	一般不使用source map
​	也可以使用nosources-source-map // 定位错误行列, 没有具体代码





#### ts-loader

> 把ts转成js, 在编译的时候, 就可以检查代码中的语法错误, babel-loader不行

#### ts最佳实践

1. Package.json里面写做语法校验的命令

   ```javascript
   "scripts" : {
     "ck": "tsc --noEmit",
     "build": "npm run ck && webpack"
   }
   ```

   

2. 不用ts-loader, 用babel-loader做兼容填充

#### 代码拆分方式

##### 多入口

```javascript
entry: {
	main1: {
    import: './src/main1.js',
    dependOn: 'shared'
  }
	main2: {
    import: './src/main2.js',
    dependOn: 'shared'
  },
  shared: ['lodash', 'jquery']
},
output: {
  filename: '[name].bundle.js'
},
plugins: [
	new HtmlWebpackPlugin({
    title: 'main1',
    template: './src/main1/html',
    filename: 'main1.html',
    chunks:['main1']
  }),
  new HtmlWebpackPlugin({
    title: 'main2',
    template: './src/main2.html',
    filename: 'main2.html',
    chunks:['main2']
  })
]
```

##### splitchunks配置

```javascript
		splitChunks: {
			chunks: 'initial', // 这表明将选择哪些 chunk 进行优化。当提供一个字符串，有效值为 all，async 和 initial。设置为 all 可能特别强大，因为这意味着 chunk 可以在异步和非异步 chunk 之间共享。
			// minSize: 20000, // 最小这个大小, 否则就不拆包
			// maxSize: 20000,// 最大这个大小, 否则就不拆包
			minChunks: 1, // 至少引用多少次, 才拆包
			cacheGroups: { // 缓存组可以继承和/或覆盖来自 splitChunks.* 的任何选项。但是 test、priority 和 reuseExistingChunk 只能在缓存组级别上进行配置。将它们设置为 false以禁用任何默认缓存组。
				syVendors: {
					test: /[\\/]node_modules[\\/]/, // [\\/] 兼容不同系统 windows \ mac / 第一个\是转义
				    filename: 'js/[id]_vendor.js',
					// priority: -10 // 优先级
				},
				default: {
					minChunks: 2,
					filename: 'js/syy_[id].js',
					// priority: -20
				}
			}
		}
```

#### runtimeChunk优化配置

> 把依赖文件的runtime部分逻辑抽离出来, 在生产环境如果这部分没有变动, 就不用再次到服务器请求资源

```javascript
 optimization: {
    runtimeChunk: true
  },
```

#### 代码懒加载

```javascript
// 按需加载
oBtn.addEventListener('click', ()=> {
	import('./js/utils').then(({default: element})=> {
		console.log(element);
	})
})
```

#### 预获取/预加载模块(prefetch/preload module)

- **prefetch**(预获取)：将来某些导航下可能需要的资源
- **preload**(预加载)：当前导航下可能需要资源

只要父 chunk 完成加载，webpack 就会添加 prefetch hint(预取提示)。

与 prefetch 指令相比，preload 指令有许多不同之处：

- preload chunk 会在父 chunk 加载时，以并行方式开始加载。prefetch chunk 会在父 chunk 加载结束后开始加载。
- preload chunk 具有中等优先级，并立即下载。prefetch chunk 在浏览器闲置时下载。
- preload chunk 会在父 chunk 中立即请求，用于当下时刻。prefetch chunk 会用于未来的某个时刻。

```javascript
import(/* webpackPrefetch: true */ './path/to/LoginModal.js');
import(/* webpackPreload: true */ 'ChartingLibrary');

```

#### 第三方扩展CDN 

```javascript
externals: { // 外部扩展
	lodash: '_' // lodash 不打包到dist, 别名是_
}
```

#### 打包DLL库 (优化打包速度)

> webpack里面有dll动态连接库,  项目里面有一些共享的资源, 比较大, 没有必要经常变动,去打包, 就有了dll动态连接库

```javascript
const resolveApp = require('./paths.js')
const { DllPlugin } = require('webpack')
module.exports = {
	mode: "production",
	entry: {  // 入口文件
		react: ['react', 'react-dom']
	},
	output: { // 输出文件
		path: resolveApp('dll'), // 输出文件存放的路径
		filename: 'dll_[name].js',
		library: 'dll_[name]'
	},
	plugins: [// 插件
		new DllPlugin({
			name: 'dll_[name]',
			path: resolveApp('./dll/[name].manifest.json'), 
		})
	]
}
```

#### 使用DLL库

```javascript
const { DllReferencePlugin } = require('webpack')
const AddAssetHtmlPlugin = require('add-asset-html-webpack-plugin');
{
  plugins: [// 插件
		new DllReferencePlugin({
			context: resolveApp('./'),
			manifest: resolveApp('./dll/react.manifest.json')
		}),
		new AddAssetHtmlPlugin({ 
			outputPath: 'js',
			filepath: resolveApp('./dll/dll_react.js') 
		}),
	]
}

```

#### CSS抽离和压缩

抽离

```javascript
  const MiniCssExtractPlugin = require("mini-css-extract-plugin");

		{
				test: /\.css$/,// 一般是一个正则表达式, 用来匹配文件类型
				// style-loader 会生成一个style标签, 把对应的css添加进来
				use: [
					MiniCssExtractPlugin.loader, 
					{
						loader: 'css-loader',
						options: {
							importLoaders: 1, // 碰到引入的css文件, 再次从上一个loader处理下, 这里是postcss-loader
							esModule: false // 不转成esModule

						}
					},
					'postcss-loader'
				] // loader 从右往左运行
			},
      plugins: [
        // 插件
         new MiniCssExtractPlugin({
            filename: '[name].[hash:8].css'
          })
      ]
```

压缩

```javascript
const CssMinimizerPlugin = require("css-minimizer-webpack-plugin");
optimization: {
		minimizer: [
			// 在 webpack@5 中，你可以使用 `...` 语法来扩展现有的 minimizer（即 `terser-webpack-plugin`），将下一行取消注释
			// `...`,
			new CssMinimizerPlugin(),
		],
},
```

#### TerserPlugin压缩JS

```javascript
const TerserPlugin = require("terser-webpack-plugin");
optimization: {
		minimizer: [
			new TerserPlugin()
		],
},
```

#### scope hoisting (作用域优化)

```javascript
const webpack = require("webpack")
plugins: [// 插件
		  new webpack.optimize.ModuleConcatenationPlugin() // 把依赖文件放到相同作用域中
]
```

#### usedExports配置(实现tree shaking: 移除 JavaScript 上下文中的未引用代码)

> 标记没有使用的代码

```javascript
optimization: {
    usedExports: true, // 打包时,标记没有使用的代码
    minimize:true, // 开启使用minimizer给定的条件
    minimizer: [
			new TerserPlugin() // TerserPlugin就可以tree shaking
		],
},
```

#### sideEffects配置(实现tree shaking: 移除 JavaScript 上下文中的未引用代码)

> 模块的副作用, 可以理解为引入了它但没有使用, webpack 默认不会清除这个模块, 因为不知道这个模块, 是否有用处, 比如使用了window对象, 引入了这个模块, 最好不要清除, 使用这个选项,能优化代码的体积, 但是使用时, 要小心, 不要剔除掉有用的代码了

* `package.json` 文件中配置sideEffects

  ```javascript
  sideEffects: false // 取消所有模块的副作用
  sideEffects: [
  	"./src/title.js",
    "*.css" // 也可以在webpack中配置
  ] // 只有title.js可以使用副作用
  ```

* webpack配置中:

```javascript
{
  test: /\.css$/
  use: [
    // loader...
  ], // loader 从右往左运行
  sideEffects: true // 在这也可以使用sideEffects
},
```

#### CSS tree shaking

使用purges-webpack-plugin

```javascript
const PurgeCSSPlugin = require('purgecss-webpack-plugin')
const glob = require('glob')
const PATHS = {
  src: path.join(__dirname, 'src')
}

module.exports = {
  entry: './src/index.js',
  plugins: [
    new PurgeCSSPlugin({
      paths: glob.sync(`${PATHS.src}/**/*`,  { nodir: true }),
      safelist:function () { // 这是不会被 tree shaking
        return {
          standard: ['safelisted', /^safelisted-/],
        }
      }
    }),
  ]
}
```

#### 资源压缩

> 处理打包后的文件进行压缩, 浏览器支持的压缩格式

使用compress-webpack-plugin

```javascript
var CompressPlugin = require("compress-webpack-plugin");
module.exports = {
    plugins: [
        new CompressPlugin({
            asset: "[path].gz[query]",
            algorithm: "gzip",
            test: /\.(css|js|html)$/,
            threshold: 10240, // 处理多大的资源 defaults to 0
            minRatio: 0.8 // 最小压缩比例, 没达到就不压缩
        })
    ]
}
```

####  inlineChunkHtmlPlugin 使用

> html中注入一些内容, 减少http请求的发送, 改成直接放在html中

```javascript
const InlineChunkHtmlPlugin = require('@qunhe/inline-chunk-html-plugin');
module.exports = {
    plugins: [
        new InlineChunkHtmlPlugin(HtmlWebpackPlugin, 					[runtime]); // 把runtime文件放到html中
    ]
}
```

#### webpack 打包 Library(库)

```javascript
output: {
  filename: 'sy_utils.js', // 入口文件
  path: path.resolve(__dirname, 'dist'), // 打包后的文件
  library: 'syUtil', // 库的名字
  globalObject: 'this' // 全局对象
}
```

#### 打包时间和内容分析

speed-measure-webpack-plugin (打包时间分析)

```javascript
const SpeedMeasurePlugin = require("speed-measure-webpack-plugin");
const smp = new SpeedMeasurePlugin();
const webpackConfig = smp.wrap({
  plugins: [new MyPlugin(), new MyOtherPlugin()],
});
```

webpack-bundle-analyser (打包内容分析)

```javascript
module.exports = {
  // ...config
  plugins: [
    new BundleAnalyzerPlugin(),
  ],
};
```

#### 开发一个webpack插件

```javascript
class MyPlugin {
	apply (compiler) {
		compiler.hooks.emit.tap('MyPlugin', compilation => {
      // compilation => 可以理解为此次打包的上下文
      for(const name in compilation.assets) {
        console.log(name)
      }
    })
	}
}
module.exports = {
	plugins: [
    new MyPlugin()
  ]
}
```

#### Webpack 输出文件名 Hash

* hash
  整个项目级的hash, 一个文件改变了, 所有文件的hash都会变化

  出口是hash，那么一旦针对项目中任何一个文件的修改，都会构建整个项目，重新获取hash值，缓存的目的将失效。

  图片这些静态资源使用hash

* chunkhash
  chunk级的, 打包到相同chunk的文件hash会变,更精准点
  以项目主入口文件main.js及其对应的依赖文件main.css由于被打包在同一个模块，所以共用相同的chunkhash
  修改了js文件, 相对应的css的chunkhash也会变, 修改了css文件，js的hash值也会变
  所以css文件可以使用contenthash, 更加合适

*  contenthash 
  由文件内容产生的hash值，内容不同产生的contenthash值也不一样,更更精准
