<!--
name: 'Tool Description: NotebookEdit'
description: 编辑 Jupyter 笔记本单元格的工具描述
ccVersion: 2.0.14
-->
使用新源代码完全替换 Jupyter 笔记本 (.ipynb 文件) 中特定单元格的内容。Jupyter 笔记本是结合了代码、文本和可视化的交互式文档，常用于数据分析和科学计算。notebook_path 参数必须是绝对路径，不能是相对路径。cell_number 从 0 开始索引。使用 edit_mode=insert 在 cell_number 指定的索引处添加新单元格。使用 edit_mode=delete 删除 cell_number 指定的索引处的单元格。
