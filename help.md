---
title: 麻将 Copilot 帮助信息
layout: help
---
# 运行方法

1. <a href="https://download.mjcopilot.com">下载压缩包</a> 并解压；
2. 获取 AI 模型：<a href="#models">点击查看帮助</a>；
3. 运行 MahjongCopilot 程序；点击设置（齿轮）按钮，选择模型并填写配置；
5. 点击左上方雀魂按钮，启动内置浏览器和雀魂网页客户端，开始游戏。

# 使用帮助

## 游戏客户端
1. 点击左上角“雀魂”按钮，会使用内置的 Chromium 浏览器打开游戏。HUD 显示和自动打牌等功能仅支持内置浏览器。
2. 软件使用中间人 (MITM) 代理服务器获取游戏客户端和服务器之间传输的信息。设置中的"MITM 服务端口"是代理服务器端口，MITM 会开启代理 `http://localhost:{端口}`，其他客户端和设备可以使用该代理服务器（或使用第三方软件如 Proxifier）以使用本程序。
3. 操作系统需要安装 MITM 证书才能正确使用代理，否则主界面显示“MITM证书错误”，浏览器显示“不安全”，或游戏无法连接。请以管理员权限启动一次，或者手动安装证书：到`mitm_config` 目录下，安装扩展名为 .cer 的证书至“受信任的根证书颁发机构”目录下。
4. 首次加载网页客户端，或游戏有更新时，会因为下载游戏素材而导致网页加载时间变长或疑似卡住。这是正常现象。
5. “上游代理”选项：如果你需要通过其他代理服务器连接互联网和雀魂服务器，可在此填写代理地址（如：`http://1.2.3.4:5555`）。
6. “自动代理雀魂客户端”选项：程序会自动截取雀魂 Windows 客户端的网络流量并使用 MITM 代理（通过调用 <a href="https://github.com/PragmaTwice/proxinject"> proxinject </a> 实现）。注意，启用此项会使“上游代理”选项失效，并使 MITM 服务采用 socks5 协议 `socks5://localhost:{端口}`. 更改 MITM 和自动代理选项后，需要重启程序生效。
7. “启用浏览器插件”：勾选后内置浏览器启动时将加载插件。用压缩工具（比如 7zip）打开 Chrome 插件文件（.crx），将压缩包中的内容解压到程序目录 `chrome_ext` 下的单独目录。例如，要使用油猴插件，将油猴插件 .crx 文件用解压工具打开，将内容解压到 `chrome_ext\TamperMonkey\` 目录中。使用插件有风险，请自行甄别。
8. 网页客户端最小化或者标签处于后台时，将无法操作自动打牌。一个使用技巧：Windows 系统中，可使用 Win+Tab 创建虚拟桌面，将窗口放到虚拟桌面中然后隐藏至后台。

## 程序界面说明
1. “启动网页客户端”：启动内置的浏览器，打开雀魂网页客户端。
2. “设置”：打开设置窗口。设置项详情见下一节。
3. “打开日志文件”：用默认编辑器打开日志文件。
4. “帮助”：打开帮助窗口。帮助窗口中可以进行自动更新（暂时支持 Windows）。Mac 用户需要自助从 Python 源代码更新本程序，请参见本项目 Github。
5. 网页 HUD: 在网页上覆盖显示信息和 AI 选项。关闭此项可略微提升自动打牌流畅度。
6. 自动打牌：自动化操作的总开关，关闭后将停止所有自动化操作。
7. 自动加入：按设置中选择的级别和模式，自动加入下一局游戏；自动加入操作需要从主菜单或游戏中开始。可以为自动加入设置计时器，计时结束时停止自动加入。
8. 状态栏中，显示模型信息，以及主程序和游戏客户端状态（括号中显示刷新率，作为性能参考；通常数值在 100 以上说明程序运行流畅；进行自动打牌操作时，浏览器刷新率会下降）。如果程序或 AI 发生错误，状态栏中会显示错误信息。

![GUI](\assets\images\gui.png){: style="max-width: 100%"}

## 设置项说明
1. 自动启动浏览器：程序启动时，自动启动浏览器和游戏。
2. 客户端大小：浏览器中客户端的分辨率。
3. 雀魂网址：浏览器中打开雀魂的网址。
4. MITM服务端口、上游代理、自动代理雀魂客户端：见前两节游戏客户端帮助。
5. AI 模型类型：选择使用的模型类型。不同模型有不同的配置项。详见下一节说明。
6. 自动打牌设置：
   - 鼠标移动随机化：鼠标点击前，随机移动几次，避免被检测为自动化操作。
   - 鼠标空闲移动：鼠标空闲时（比如他人回合）一定概率随机移动。
   - 鼠标拖拽出牌：使用鼠标拖动代替点击来出牌。某些用户会碰到鼠标点击出牌失败的情况，请勾选此项。
   - AI选项随机化（去重）：自动打牌时，根据模型推荐的前三选项，按概率（权重）随机选取其中之一。可以在分析牌谱时，降低模型重合率。0 为关闭（仅选择权重最高项），5 为最高（按概率权重选择），1-4 的随机性位于中间。计算方式：取原概率的(5/n)次方，再归一概率，按概率选择。
   - 回复表情概率：以一定概率回复其他玩家的表情。打牌时不会触发；有最短触发间隔 (settings.json 中 auto_emoji_intervel 数值)。
   - 基础延迟随机范围：自动打牌前的延迟。自动打牌会根据场况和打的牌，会在基础延迟上增加额外延迟；例如，东家第一巡会加几秒钟延迟，让理牌动画完成。程序延迟会包含模型计算时间；若模型计算时间超过需要的延迟，则不会附加额外延迟。

![Settings](\assets\images\settings.png){: style="max-width: 100%"}

<a id="models"></a>

## 模型配置

这里介绍不同模型类型和需要的配置。

### 1. Local
兼容 Akagi 的本地 Mortal 模型，支持四麻和三麻。配置方法：
1. 获取模型文件：免费获取四麻模型文件 (mortal.pth)，请参见 Akagi 作者 Shinkuan 的 <a href="https://github.com/shinkuan/Akagi/blob/main/README_CH.md#%E5%AE%89%E8%A3%9D" target="_blank">Github说明</a> 和 <a href="https://discord.com/invite/Z2wjXUK8bN" target="_blank">Discord 服务器</a>。
2. 将模型文件(.pth文件)放到程序 `models` 目录下，并在设置中“本地模型文件”项，选择使用该模型文件。

<a id="3p"></a>

**三麻模型设置步骤：**
1. 三麻相关文件，限 <a href="https://discord.com/invite/Z2wjXUK8bN" target="_blank">Discord 服务器</a>的捐献者 (Donor) 获取。请到 Discord 服务器，获得 Donor 资格。
2. 到 #bot-3p-zip 频道下载三麻支持文件。
3. 将压缩包 bot_3p_0.1.1.zip 中 libriichi 目录中的所有文件 (pyd 和 so 文件)解压放到程序目录 `libriichi3p` 下（不要有子目录）。
4. 将另一个压缩包中的三麻模型文件 (pth文件)，放到 `models` 目录下，并在程序设置项“本地模型文件(三麻)”选择该模型文件。
5. 模型支持的游戏类型会显示在状态栏，"模型: Local" 后的括号内。

### 2. AkagiOT
Akagi 作者开发的在线模型，支持四麻和三麻。限 <a href="https://discord.com/invite/Z2wjXUK8bN" target="_blank">Discord 服务器</a> 的捐献者（Donor）获取。配置方法：
1. 去 Discord 服务器，获得 Donor 资格。
2. 到 #bot 频道，输入"!api_gen"，机器人会发送 API Key 给你。
3. 到 #online-model 频道，查看 API 服务器地址（"server"字段）。
4. 程序设置中，填写 AkagiOT 服务器地址和AkagiOT API Key, AI 模型类型选择"AkagiOT"并保存。

### 3. MJAPI
(作者 9ns4esyx) 开发的在线模型，暂时支持四麻。配置方法：
1. 在程序设置中，填写已有的 MJAPI 用户名和密钥；或者，将用户名和密钥 (Secret) 留空，软件会自动注册新用户并登录。
2. MJAPI 有不同风格的模型可选择，登陆后会刷新模型选项，并显示 API 用量。

4月13日更新的 MJAPI 地址：（临时域名有总连接数限制，而且不保证长久有效，之后也可能会变。服务器在国外，不同运营商的网络连通性不一，请自己测试）

- 临时域名：`https://cdt-authentication-consultation-significance.trycloudflare.com`
- 稳定域名：`https://mjai.7xcnnw11phu.eu.org`

几个模型的区别：
- "baseline" 与 Mortal 在线 v4 模型重合率最高，但是打不过"defensive"和"aggressive"，在低段位场可能比较合适。
- "aggressive" 以提高四位率的代价提高一位率，但是需要对手有一定的水平，因为更加依靠读牌和对手合理的防守。
- "defensive" 更偏重防守。

## 账号安全提示

长时间使用自动打牌，以及牌谱 AI 重合率高，可能会导致账户被封禁。关闭自动打牌和 HUD 显示功能，并且打牌选择与 AI 推荐不同的选项，可以减少账号封禁风险。请将本程序用于学习和研究目的。使用者需要承担潜在风险和责任。

# 鸣谢 / Credit

- 基于 Mortal 模型和 MJAI 协议:
  <a href="https://github.com/Equim-chan/Mortal" target="_blank"> Mortal Github </a>
- 设计和功能实现基于 Akagi:
  <a href="https://github.com/shinkuan/Akagi" target="_blank"> Akagi Github </a>
- 参考项目: <a href="https://github.com/MahjongRepository/mahjong_soul_api" target="_blank"> Mahjong Soul API </a>
- MJAI协议参考:
  <a href="https://mjai.app" target="_blank"> mjai.app </a>

# 许可 / License
本项目采用 GNU GPL v3 许可协议。 协议全文请见 <a href="https://github.com/latorc/MahjongCopilot/blob/main/LICENSE" target="_blank"> LICENSE </a>

```
Mahjong Copilot Copyright (C) 2024 Latorc
This program comes with ABSOLUTELY NO WARRANTY;
This is free software, and you are welcome to redistribute it under certain conditions.
License details: https://github.com/latorc/MahjongCopilot/blob/main/LICENSE

麻将 Copilot 版权所有 (C) 2024 Latorc
本程序绝对没有任何保证；
这是自由软件，欢迎您基于某些条件重新分发它。
协议详情：https://github.com/latorc/MahjongCopilot/blob/main/LICENSE
```

# 讨论 / Discussion
* QQ群：834105526 <a target="_blank" href="https://qm.qq.com/cgi-bin/qm/qr?k=Mec5daqIyUsuZjCLojH_t88hQV6luPxl&jump_from=webapi&authKey=nNSpmIQY3ieVau/oLTF9eNO6YTqAm1+Ne3iE3zpqmFrj61iAUDu/GSpA38g93Zlx"><img border="0" src="https://pub.idqqimg.com/wpa/images/group.png" alt="加入QQ群" title="麻将 Copilot"></a>
* 欢迎提交 Github Issues 反馈问题。


![Logo](/assets/images/logo.png){: style="max-height: 150px"}
