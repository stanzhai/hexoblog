title: Sublime Text 2折腾记录
date: 2013-08-27 18:19:54
categories: 工具
tags: sublime text
---

## 安装Package Control

1. 打开 Sublime Text 2，按下 Control + ` 调出 Console，通常这个快捷键会与PC上的其它软件起冲突，需要修改其它软件的这个快捷键。
2. 将以下代码粘贴进命令行中并回车：
	```
	import urllib2,os;pf='Package Control.sublime-package';ipp=sublime.installed_packages_path();os.makedirs(ipp) if not os.path.exists(ipp) else None;open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read())
	```
3. 重启 Sublime Text 2，如果在 Preferences -> Package Settings中见到Package Control这一项，就说明安装成功了。

<!--more-->

## 安装Nil主题

`Ctrl+Shift+P`输入install package，选择Nil，安装即可。

## 我的用户配置

```
{
  "caret_style": "phase",
  "color_scheme": "Packages/Color Scheme - Default/Cobalt.tmTheme",
  "font_size": 12.0,
  "highlight_line": true,
  "ignored_packages":
  [
  ],
  "line_padding_bottom": 1,
  "line_padding_top": 1,
  "margin": 4,
  "tab_size": 2,
  "theme": "Nil.sublime-theme",
  "translate_tabs_to_spaces": true,
  "word_wrap": true
}
```

## 我的插件

* MarkdownEditing，编辑Markdown必备
* MarkdownPreview，以html的形式预览markdown标签
* DocBlockr，自动补全注释，在function上一行输入/**，然后按Tab就自动补全注释
* PlainTasks，任务管理，支持通过快捷键来添加任务、标记完成、归档，支持添加标签等等功能
* Alignment，代码对齐，尤其是对C++的程序员较为有用，按照=自动左右对其，快捷键ctrl+alt+A