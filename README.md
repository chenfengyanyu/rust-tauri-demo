# 前置安装
[说明文档](https://tauri.app/zh-cn/v1/guides/getting-started/prerequisites)
# Rust + Tauri + Yew
```bash
cargo install create-tauri-app
# 初始化项目
cargo create-tauri-app
```
如果你还没有安装`Tauri CLI`
```bash
cargo install tauri-cli
```
# 运行项目
```bash
cargo tauri dev
```
日志信息如下：
```
Running BeforeDevCommand (`trunk serve`)
2023-03-19T12:51:19.075591Z  INFO 📦 starting build
2023-03-19T12:51:19.075822Z  INFO spawning asset pipelines
2023-03-19T12:51:19.486709Z  INFO building rust-tauri-app-ui
2023-03-19T12:51:19.486759Z  INFO copying directory path="public"
2023-03-19T12:51:19.486782Z  INFO copying & hashing css path="styles.css"
2023-03-19T12:51:19.487151Z  INFO finished copying & hashing css path="styles.css"
2023-03-19T12:51:19.487281Z  INFO finished copying directory path="public"
    Finished dev [unoptimized + debuginfo] target(s) in 0.17s
2023-03-19T12:51:19.701213Z  INFO fetching cargo artifacts
2023-03-19T12:51:19.940618Z  INFO processing WASM for rust-tauri-app-ui
2023-03-19T12:51:19.961661Z  INFO calling wasm-bindgen for rust-tauri-app-ui
2023-03-19T12:51:20.120952Z  INFO copying generated wasm-bindgen artifacts
2023-03-19T12:51:20.122868Z  INFO applying new distribution
2023-03-19T12:51:20.123718Z  INFO ✅ success
2023-03-19T12:51:20.124694Z  INFO 📡 serving static assets at -> /
2023-03-19T12:51:20.124755Z  INFO 📡 server listening at http://127.0.0.1:1420
        Info Watching /Users/jartto/Documents/Project/rust-tauri-app/src-tauri for changes...
   Compiling rust-tauri-app v0.0.0 (/Users/jartto/Documents/Project/rust-tauri-app/src-tauri)
    Finished dev [unoptimized + debuginfo] target(s) in 2.29s
```
# 效果预览
如果看到如下窗口，意味着应用已经正常启动了。
![效果预览](https://raw.githubusercontent.com/chenfengyanyu/rust-tauri-demo/master/src-tauri/img/demo.png)

# 代码说明
Tauri 为您的前端开发提供了其他系统原生功能。 我们将其称作指令，这使得您可以从 JavaScript 前端调用由 Rust 编写的函数。 由此，您可以使用性能飞快的 Rust 代码处理繁重的任务或系统调用。

以下是一个简单示例：
```rust
// src-tauri/src/main.rs
#[tauri::command]
fn greet(name: &str) -> String {
   format!("Hello, {}!", name)
}
```
一个指令等于一个普通的 Rust 函数，只是还加上了 #[tauri::command] 宏来让其与您的 JavaScript 环境交互。
最后，我们需要让 Tauri 知悉您刚创建的指令才能让其调用。 我们需要使用 .invoke_handler() 函数及 Generate_handler![] 宏来注册指令：
```rust
// src-tauri/src/main.rs
fn main() {
  tauri::Builder::default()
    .invoke_handler(tauri::generate_handler![greet])
    .run(tauri::generate_context!())
    .expect("error while running tauri application");
}
```
现在前端可以调用刚注册的指令了！
我们通常会推荐使用 @tauri-apps/api 包，但由于本指南至此还没有使用打包工具，所以需要在 tauri.conf.json 文件中启用 withGlobalTauri 选项。
```rust
{
    "build": {
    "beforeBuildCommand": "",
    "beforeDevCommand": "",
    "devPath": "../ui",
    "distDir": "../ui",
    "withGlobalTauri": true
},
```
此选项会将已打包版本的 API 函数注入到前端中。接下来，可以修改 index.html 文件来调用指令：
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <h1 id="header">Welcome from Tauri!</h1>
    <script>
      // access the pre-bundled global API functions
      const { invoke } = window.__TAURI__.tauri

      // now we can call our Command!
      // You will see "Welcome from Tauri" replaced
      // by "Hello, World!"!
      invoke('greet', { name: 'World' })
        // `invoke` returns a Promise
        .then((response) => {
          window.header.innerHTML = response
        })
    </script>
  </body>
</html>
```
# 应用调试
[应用程序调试](https://tauri.app/zh-cn/v1/guides/debugging/application)

# 打包应用
执行命令
```bash
cargo tauri build
```
会报如下错误：
```
You must change the bundle identifier in `tauri.conf.json > tauri > bundle > identifier`. The default value `com.tauri.dev` is not allowed as it must be unique across applications.
您必须在 `tauri.conf.json > tauri > bundle > identifier` 中更改包标识符。 默认值 com.tauri.dev 是不允许的，因为它在应用程序中必须是唯一的。
```
按照提示修改标识符：com.tauri.dev ——> com.tauri.jartto，重新运行打包命令：
```bash
cargo tauri build
```
此时，程序已经开始编译：
```bash
Compiling serialize-to-javascript v0.1.1
   Compiling rust-tauri-app v0.0.0 (/Users/jartto/Documents/Project/rust-tauri-app/src-tauri)
   Compiling ignore v0.4.18
   Compiling tauri-macros v1.2.1
   Compiling serde_repr v0.1.12
   Compiling encoding_rs v0.8.32
   Compiling state v0.5.3
   Compiling embed_plist v1.2.2
    Building [=======================> ] 376/377: rust-tauri-app(bin)
```
我们重点关注最后两行：
```bash
Finished 2 bundles at:
/Users/jartto/Documents/Project/rust-tauri-app/target/release/bundle/macos/rust-tauri-app.app
/Users/jartto/Documents/Project/rust-tauri-app/target/release/bundle/dmg/rust-tauri-app_0.0.0_aarch64.dmg
```
顺着目录可以找到最终打包生成的文件地址，如下：
![构建包地址](https://raw.githubusercontent.com/chenfengyanyu/rust-tauri-demo/master/src-tauri/img/dmginfo.png)

查看 APP 文件大小：
![文件大小](https://raw.githubusercontent.com/chenfengyanyu/rust-tauri-demo/master/src-tauri/img/size.png)

选择 rust-tauri-app_0.0.0_aarch64.dmg 并双击安装，如下：
![安装 APP](https://raw.githubusercontent.com/chenfengyanyu/rust-tauri-demo/master/src-tauri/img/setup.png)

运行效果就不用说了，和上文效果预览一致。至此，起手项目大功告成！