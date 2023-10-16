---
title: 原來有三個 Hexo NexT repo
date: 2023-10-16 23:20:19
tags:
  - Hexo
  - front-end
---

第一篇就貢獻給 Hexo 吧

## 背景~~廢話~~

今天在找 Hexo 主題的時候，看了一下[有 hexo-theme tag 的 github repo](https://github.com/topics/hexo-theme)，排第一的是 [iissnan/hexo-theme-next](https://github.com/iissnan/hexo-theme-next)，但仔細一看，最新的 release 已經是 2018 年了。

不過好在 README 有寫，"The community-maintained version is here: [NexT v6 and v7](https://github.com/theme-next/hexo-theme-next) 🚩"，雖然最後的 v7.8.0 release 是 2020，但多少比較新，於是就開始進行安裝與設定。

沒想到裝完後，在看其他關於 NexT 的文章時，其中提及了 v8.x 版。\
不是只到 v7.8.0 嗎？？？於是又找了下，原來另外又有一個 [repo](https://github.com/next-theme/hexo-theme-next) 還在維護，已經更新到 v8.18.2 。

## 原因

簡單來說，兩次換 repo 都是因爲原本的維護者不再活躍，但也沒有移交權限，導致活躍的成員另起爐竈。

而許多過往文章都指向舊版 repo ，加上 star 最多的最舊、前一版 README 沒有指向最新版，所以就跑去裝舊版了……

| version         | star  | repo                                          | 搬家說明 issue                                                                        |
| --------------- | ----- | --------------------------------------------- | ------------------------------------------------------------------------------------- |
| ~ v5.1.4        | 15.9k | https://github.com/iissnan/hexo-theme-next    |                                                                                       |
| v6.0.0 ~ v7.8.0 | 8k    | https://github.com/theme-next/hexo-theme-next | [link](https://github.com/iissnan/hexo-theme-next/issues/2061#issuecomment-354696520) |
| v8.0.0 ~        | 2.1k  | https://github.com/next-theme/hexo-theme-next | [link](https://github.com/next-theme/hexo-theme-next/issues/4#issuecomment-626205848) |
