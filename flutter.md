# 关于怎样在mac上结合vscode使用fvm

### 安装fvm

```
brew install fvm
```

### 配置环境变量

```
# fvm
export FVM_ROOT = $HOME/.fvm
# flutter
export FLUTTER_ROOT=$FVM_ROOT/flutter_sdk
export PATH=$FLUTTER_ROOT/bin:$PATH
export PATH=$FLUTTER_ROOT/bin/cache/dart-sdk/bin:$PATH
```

### fvm 命令

```
// 查看版本列表
fvm list
// 安装版本
fvm install <version>
// 切换版本
fvm use <version>
// 删除版本
fvm remove <version>
// 查看当前fvm详细信息
fvm doctor
```
