---
layout: post
title: "Vim 与中文输入法"
subtitle: 'Using Vim with non-english input method'
author: "Hux"
header-style: text
tags:
  - Vim
---

1. vector::push_back与emplace_back
- 差异1：如果插入vector的 类型 的 构造函数 接受多个参数，那么push_back只能 接受 该类型的对象（实例），emplace_back 还能 接受 该类型的构造函数的参数.
- 内置类型（built-in type）：效率都一样；用户自定义的类（User-defined type）：emplace_pack仅在通过使用构造参数列表的方式传入的时候更高效，而传入对象或者临时对象时，两者效率仍然一样！
- 不管是struct还是class，一定要有public类型的构造函数如帖子中的New(int ii, string s):i(ii) , st(s) {}才能使用emplace_back加构造参数列表的形式，否则会编译错误。
[C++姿势点: push_back和emplace_back](https://zhuanlan.zhihu.com/p/183861524)
