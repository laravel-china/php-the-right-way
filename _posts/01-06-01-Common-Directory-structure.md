---
isChild: true
anchor:  common_directory_structure
title: 常见目录结构
---

## 常见目录结构 {#common_directory_structure}

对于那些刚开始为web编写程序的人来说，一个常见的问题是:“我应该把我的文件放在哪里?” 多年来，这个答案一直是“DocumentRoot 指向的目录”。虽然这个答案还不完整，但这是一个很好的开始。

出于安全原因，配置文件不应该被人访问到; 因此，公共脚本保存在公共目录中，私有配置和数据保存在该目录之外。

对于 每个团队、CMS或框架，这些都有目录结构标准。但是，如果个人开发一个项目，那么使用哪种文件结构都是一件令人烦恼的事。

[Paul M. Jones] 对 github上成千上万的 PHP 项目中进行了研究。根据研究结果，他编写了一套标准文件和目录结构的标准，即 [Standard PHP Package Skeleton]。在这个目录结构中，`DocumentRoot` 应该指向 `public/`，单元测试应该在 `tests/`目录中，而 `composer` 安装的第三方包应该在 `vender/` 目录。遵循标准的PHP包和框架目录标准对项目的发展是非常有意义的。

[Paul M. Jones]: https://twitter.com/pmjones
[Standard PHP Package Skeleton]: https://github.com/php-pds/skeleton
[Composer]: /#composer_and_packagist
