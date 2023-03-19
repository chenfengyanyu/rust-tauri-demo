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
![]()
