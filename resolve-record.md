### 关于vue中输入框设置选中失效的问题
设置开始位置
input.selectionStart = 0
设置结束位置
input.selectionEnd = 5
聚焦选区
input.focus()
当发现没有选择内容时，有可能是因为你的vue在你设置内容后刷新了页面导致选区被清空，可以通过this.$nextTick()后在下一次页面刷新后再去执行相应操作就可以了