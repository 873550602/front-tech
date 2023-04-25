# editorConfig相关知识点

## 官网 <https://EditorConfig.org>

## 全部配置文件

``` bash
# 官网 https://EditorConfig.org
root = true
# 路径通配符
[*]
# 行尾符号，可以是 lf（Unix/Linux）、cr（Mac）、crlf（Windows）。
end_of_line = lf
# 是否在文件末尾插入一行空行。
insert_final_newline = false
# 字符集，一般设置为 utf-8。
charset = utf-8
# 缩进风格，可以是 tab 或 space。
indent_style = space
# 缩进大小，如果 indent_style 是 space，则为空格数；如果是 tab，则为 tab 字符数。
indent_size = 4
# tab 字符的宽度，默认为 indent_size。
# tab_width = 2
# 是否删除行末空格。
```

## VSCode支持

通过在vscode安装**EditorConfig for VS Code**插件
