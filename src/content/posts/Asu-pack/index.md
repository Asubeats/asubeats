---
title: AsuPack for 1.21.4 Fabric v1.0.5 Release (轻量版)
published: 2025-07-18
description: 本文是AsuPack for 1.21.4 v1.0.5 Release (Fabric轻量版)的发布页面
tags: [Minecraft,整合包,红石科技,使用教程,教程,mod]
category: Pack
draft: false
---

# Intro&Release&Download
## Intro
本pack是Asu在服务器即将迈入1.21.4时自制的一个整合包，旨在平衡非重度生电和建筑以及性能优化，也就是说，如果是重度生电那么这个包肯定是不足的

由于是第一次制作整合包，如果有不妥的地方还请指出，我会去修改的

## Release
Minecraft版本: `1.21.4 Fabric`  
整合包版本:`v1.0.5`  
Mod数量: `55个`(包括Mod本体及其依赖库)

## Download 
[点此进入分享链接](https://drive.google.com/file/d/1ueTDMklx2pZpq3TVArsy5_lLDpADabls/view?usp=drive_link)

# 整合包介绍
## Mod介绍
### 性能优化
* 经验机制改革(Clumps)  
    合并经验球，减少计算量
* 钠 及其 附属性能优化  
    包括：Sodium本体+动态光源+附属+土径阴影+扩展
* 锂(Lithium)
* Bobby  
    缓存区块服务器，减少渲染压力
* 实体渲染机制优化(Entity Culling)
* 树叶渲染优化 (Cull leaves)
* Very Many Players
* Tweakerwoo的部分禁用项
* BadOptimizations
* 动态FPS(Dynamic FPS)  
    后台/闲时自动限制客户端渲染帧数，减少能耗&电脑压力
* C^2M引擎  
    优化区块
* 方块实体优化(Enhanced block entities)  
    减少实体计算量
* 修复GPU内存泄漏(fix GPU memory leak)  
* Debugify  
    修复mc已知的bug

### 体验优化
* 现代化UI(Modern UI)  
    优化字体，界面等
* 强制关闭加载屏幕(forceloseloadingscreen)  
    这能让你舒服点
* 苹果皮(Apple Skin)  
    让玩家能够清晰的查看目前饱食度情况
* 更好的统计信息界面(Bettrer Statistics Screen)  
    修改统计信息界面，使其更加清晰易于查看
* 聊天头像(Chat Heads)  
    大家都是萌妹！好耶！
* 输入法冲突修复(IMBlocker)

### 美化Mod
* 3D皮肤层(Skin Layers 3D)
* Continuity  
    让玻璃等材质能连接起来
* Iris Shaders  
    著名光影办法，内置了`BSL_v8.4.01.2` 和 `ComplementaryReimagined_r5.5.1`
:::tip
注意，本pack并未内置Distant Horizons这是因为dh对于生电机器缓存质量过低会很鬼畜,如果需要的话自己装上即可，不冲突
:::

### 功能性Mod
* Bundle Inventory  
    收纳袋GUI管理
* Xaero全家桶  
    包括Xaero's World Map 和Xaero's Mini Map
* 地毯(Carpet)
* 迷你HUD(Mini HUD)  
    实话说这玩意比F3好用
* 投影(Litematica)  
    著名的mc mod，无论是生电还是建筑都离不开的mod，搭配轻松放置修复使用。生电玩家大爹之一
* 轻松放置修复  
    修复投影自带的轻松放置无法放置某些方块的朝向的问题
* 物品滚轮(Item Scroller)  
    喷射合成必备，当然他的批量管理容器内物品同样也是必备
* 玉(Jade)  
    显示方块信息(虽然我觉得投影的更好用 ，但是更多人习惯这个)
* Fast Trading  
    快速与村民进行交易
* Flashback  
    新的录制mod，更少的存储占用
:::tip
建议不使用的时候禁用掉Flashback，这玩意很拖慢启动速度
:::
* Lightweight Inventory Sorting  
    哦我们的OMMC居然停更了，找了个也能一键整理的
* Tweakeroo&TweakerMore  
    也是生电大爹之一，虽然做建筑也很有用，提供丰富的客户端修改项和优化项以及禁用项
* ViaFabircPlus  
    使玩家能够加入多版本的服务器
