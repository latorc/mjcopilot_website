---
title: 常见问题 FAQ
layout: faq
---

* Q: 软件怎么用呀？哪里下载呀？<br>  
A: 常见问题和使用帮助：https://mjcopilot.com/help <br>  
下载见https://mjcopilot.com 或者 Github 或者 QQ 群文件。
<br>
<br>
* Q: 本地模型文件哪里下载呀？<br> 
A: 本地模型使用兼容 Akagi 的 Mortal 模型文件。下载 Akagi 模型文件，请参照 <a href="https://github.com/shinkuan/Akagi/blob/main/README_CH.md#%E5%AE%89%E8%A3%9D" target="_blank"> Akagi Github </a>
<br>
<br>
* Q: Akagi 的模型文件有好几个，v2、v3、v4、best、mortal、min是什麼意思？<br> 
A:<br>
  - v2, v3, v4: Mortal的版本，版本越高也不一定越強。
  - Best: 在模擬對局中表現最好的模型。（這個模型可能只是運氣好）
  - Mortal: 訓練批次最多的模型。
  - Min: 這個模型不必要的部分被我移除了，減少模型佔用空間。
  - 如果你不知道选哪个，就选 best.
<br>
<br>
* Q: 可以用在三人麻将吗？怎么设置呀？<br>
A: 三人麻将的支持库和模型文件，由 Akagi 的作者通过 Discord 分发给捐赠者（Donor），关于如何捐赠和获取，请参见 <a href="https://github.com/shinkuan/Akagi/blob/main/README_CH.md" target="_blank"> Akagi Github </a> 和其 Discord 频道。三麻模型的设置见<a href="https://mjcopilot.com/help#3p" target="_blank"> 帮助 </a>。
<br><br>
* Q: 可以使用其他客户端吗，比如 Steam 和 Windows 客户端？<br>
A: 可以，在设置中勾选“自动代理雀魂 Windows 客户端”并重启程序。注意，其他客户端不支持自动打牌，且客户端可能含有检测代码，从而增高封号风险。
<br><br>
* Q: 打开网页客户端后，一直卡在黑屏或加载页面，怎么办呀？<br>
A:  雀魂网页端首次运行或者游戏版本更新后，需要在后台下载资源，这是正常现象，请耐心等待。
<br><br>
* Q: 界面显示 MITM 证书没有安装 / 网页打开后显示“不安全”无法连接，怎么办呀？<br>
A: 以管理员启动程序以自动安装 MITM 证书，或者手动安装：到程序 mitm_config 目录下，找到后缀.cer的证书文件，双击安装到计算机的“受信任的根证书颁发机构”目录。
<br><br>
* Q: AI选项随机化（去重）是什么？ 该怎么用呀？<br>
A: 开启后，自动打牌会有一定概率选择首选项以外的选项，以降低你的牌谱的 AI 重合率（以及别人举报你的概率）。使用 Mortal 模型分析牌谱（并显示重合率等）的网站是：<a href="https://mjai.ekyu.moe/" target="_blank"> Mortal 牌谱分析</a>。通常设置成 1 或 2 即可降低 AI 重合率约 4-8%.
<br><br>
* Q: 用这个软件会被封号吗？<br>
A: 有可能。以下措施可以极大降低封号风险：使用网页客户端；关闭自动打牌；关闭覆盖显示（HUD）；不要长时间连续打牌；不要总是打 AI 的首选项。
<br><br>
* Q: 可以用在雀魂日服、英文服吗？<br>
A: 可以。在设置中，修改雀魂网址为目标服务器网址。如果你连接服务器需要代理服务器，同时设置“上游代理”。<br>
  - 日服：https://game.mahjongsoul.com/
  - 英文服: https://mahjongsoul.game.yo-star.com/
<br><br>
* Q: 这个 AI 模型怎么这么龟 / 老吃四 / 不立直 / 经常做七对 / 经常弃和 / ... ?<br>
A: 麻将的随机性很高，仅仅几局游戏，不足以概括出可靠的结论。多打一段时间才能体现。模型是根据高段对局训练出来的，就是这样的风格，如果你不喜欢/不同意模型的打法，你可以临时关闭自动打牌，以你自己的想法打。

