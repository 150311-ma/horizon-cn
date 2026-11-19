# Horizon 国产版

[Horizon](https://github.com/horizon-rust/horizon) 是 Minecraft: Java 版用 Rust 重写的实现。
这个仓库是它的中国大陆适配分支，只放**改动**（补丁）和每天的自动构建，不放上游代码。

## 下载

到 [Releases](../../releases/latest) 下载 `Horizon-CN-*.zip`，解压后双击 `horizon-launcher.exe`。

> **需要你自己拥有 Minecraft: Java 版。** 启动器会登录你的微软账号，并从中准备游戏文件——
> 这一点和上游一致，本分支不改变它。

## 改了什么

改动都在 [`patches/horizon-cn.patch`](patches/horizon-cn.patch) 里，可以逐行看。

**1. 国内网络**

实测（广州，2026-11-16，绕开本地代理）只有 `maven.neoforged.net` 和 `discord.com` 完全不通，
GitHub 反而直连就通。所以不做「一律走镜像」那种省事但更慢的事：

- 镜像列表从**本分支自己的站点**取（`cyan-blog-2fk.pages.dev/horizon/config.json`），
  改一行就能换，不用重新发版；拿不到就用内置的，不影响任何本来能下的东西
- 顺序是「原址 → 站点给的镜像 → 内置镜像」，报错用原址的错误（镜像失败不代表文件不存在）
- 死掉的镜像进黑名单，断点续传不会每个分片都等它超时
- NeoForged 的 Maven **没有可用镜像**，所以不硬塞一个会 403 的「镜像」伪装成功

**2. 简体中文**

上游中文表缺 357 条（约四分之一），界面上直接冒英文。补齐后另有 173 条机翻腔重写：

- 术语纠正：`Packs` 不是「资源包」、`profile` 不是「配置文件」、`cosmetics` 是「装扮」
- 「您」统一成「你」；半角标点改全角
- 错误码（`E_*`）和命令原样保留——那些要能搜、能复制

**3. 商业内容**

- 商店、衣柜、装扮购买**整个移除**（导航从 7 项减到 5 项）
- 上游那个第三方服务器托管商的广告位移除，换成自己的轮播：项目宣传图 / GitHub 上的
  Minecraft 项目（每次启动随机一个）/ 作者收款码

## 每天自动构建

`.github/workflows/build.yml` 每天检查上游有没有新版本，有就重新打补丁、构建、发布。
补丁打不上（上游改了相关代码）时构建会失败并留日志，不会发一个坏包出去。

## 许可

上游是 **LGPL-3.0-or-later**，本分支沿用同一许可。补丁即修改后的源码，随本仓库提供。

「Minecraft」是 Mojang Studios 的商标。本项目与 Mojang、Microsoft、Horizon 官方均无隶属关系。
