---
title: 不蒜子问题总结
date: 2019-02-01 16:56:02
tags:
  - busuanzi
  - nexT
categories: skills
---
## busuanzi 出现问题，无法显示，乱行。
   busuanzi更换服务器域名了，所以要修改下nexT里面js引入的路径，需改的文件如下：
   xxx/themes/next/layout/_third-party/analytics/busuanzi-counter.swig
   
   before:
   ```javascript
   <script async src="https://dn-lbstatics.qbox.me/busuanzi/2.3/busuanzi.pure.mini.js"></script>
   ```
   alter:
   ```javascript
   <script async src="https://busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js"></script>
   ```

   官网地址：[busuanzi](http://ibruce.info/2015/04/04/busuanzi/)
## busuanzi footer 中文显示 ？？？？ 乱码处理
   site_uv_header的值使用中文的话，可能会显示成？？？？这样乱码的样子，是由于nexT配置文件_config.yml的编码格式导致的，修改为UTF-8，然后重新修改这部分乱码部分。然后 hexo g -d 重新发布即可。