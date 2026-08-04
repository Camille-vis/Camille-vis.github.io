---
title: "{{ replace .File.ContentBaseName "-" " " | title }}"
date: {{ .Date }}
tags: []
math: false
mermaid: false
draft: true
# ---- 写作方式声明:显示在标题下方。草稿阶段直接改下面的文字即可。 ----
#   不想显示这一篇的声明 -> 把整段换成一行:  aiNote: false
#   删掉整个 aiNote 字段 -> 退回站点默认措辞(在 layouts/_default/single.html 里)
aiNote: |
  **写作方式**：选题、取舍和最终把关由我本人负责；检索、推导、数值计算与行文由 Claude 与我共同完成。关键数据都注明了出处，数值结论我尽量交叉验证过，但不保证每一条都亲自复核。

  文章署我的名，所以出现的错误就是我的错误——欢迎写信指出，邮箱在[首页](/)。
---

Write here.
