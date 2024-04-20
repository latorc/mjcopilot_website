---
title: 麻将 Copilot 帮助信息
layout: page
---

## 运行方法

1. <a href="https://download.mjcopilot.com">下载压缩包</a> 并解压。
2. 配置 AI 模型：
   1. 使用本地 (Local) 模型，需要获取 Mortal 模型文件 （pth 文件），放到 `models` 目录中。本项目使用兼容 Akagi 的模型文件，获取模型请参见<a href="https://github.com/shinkuan/Akagi" target="_blank"> Akagi Github </a>. 或者
   2. 使用在线模型（MJAPI 支持自动注册）
3. 运行 MahjongCopilot，点击左上方雀魂按钮，启动内置浏览器和雀魂网页客户端。

## 使用帮助

### 游戏客户端

软件使用中间人 (MITM) 代理服务器获取游戏客户端和服务器之间传输的信息。

1. 点击左上角“雀魂”按钮，会使用内置的 Chromium 浏览器打开游戏。HUD 显示和自动打牌等功能仅支持内置浏览器。
2. 浏览器显示“不安全”，或游戏无法连接雀魂服务器，可能是由于没有管理员权限运行程序导致 MITM 证书安装失败。请以管理员权限启动一次，或者手动安装证书： 到`mitm_config` 目录下，安装扩展名为 .cer 的证书至“受信任的根证书颁发机构”目录下。
3. 首次加载网页客户端，或游戏有更新时，会因为下载游戏素材而导致网页加载时间变长或疑似卡住。
4. 网页客户端最小化或者标签处于后台时，将无法操作自动打牌。一个使用技巧：Windows 系统中，可使用 Win+Tab 创建虚拟桌面，将窗口放到虚拟桌面中然后隐藏至后台。
5. “上游代理”选项：如果你需要通过代理服务器连接到雀魂服务器，可在此填写代理服务器的地址（如：`http://1.2.3.4:5555`）。
6. “自动代理雀魂客户端”选项：程序会自动截取雀魂 Windows 客户端的网络流量并使用 MITM 代理（通过调用 <a href="https://github.com/PragmaTwice/proxinject"> proxinject </a> 实现）。
7. 其他客户端和设备也可以设置代理服务器为`socks5://localhost:{mitm端口}`（或使用第三方软件如 Proxifier)以使用本程序。

### 程序界面说明

1. “启动网页客户端”：启动内置的浏览器，打开雀魂网页客户端。
2. “设置”：打开设置窗口。设置项详情见下一节。
3. “打开日志文件”：用默认编辑器打开日志文件。
4. “帮助”：打开帮助窗口。帮助窗口中可以进行自动更新（暂时支持 Windows）。Mac 用户需要自助从 Python 源代码更新本程序，请参见本项目 Github。
5. 网页 HUD: 在网页上覆盖显示信息和 AI 选项。关闭此项可略微提升自动打牌流畅度。
6. 自动打牌：自动化操作的总开关，关闭后将停止所有自动化操作。
7. 自动加入：按设置中选择的级别和模式，自动加入下一局游戏；自动加入操作需要从主菜单或游戏中开始。可以为自动加入设置计时器，计时结束时停止自动加入。
8. 状态栏中，显示模型信息，以及主程序和游戏客户端状态（括号中显示刷新率，作为性能参考；通常数值在 100 以上说明程序运行流畅；进行自动打牌操作时，浏览器刷新率会下降）。如果程序或 AI 发生错误，状态栏中会显示错误信息。

![GUI](\assets\images\gui.png){: height="50"}

### 设置项说明

1. 自动启动浏览器：程序启动时，自动启动浏览器和游戏。
2. 客户端大小：浏览器中客户端的分辨率。
3. 雀魂网址：浏览器中打开雀魂的网址。
4. MITM服务端口、上游代理、自动代理雀魂客户端：见前两节游戏客户端帮助。
5. AI 模型类型：选择使用的模型类型。不同模型有不同的配置项。详见下一节说明。
6. 自动打牌设置：
   - 鼠标移动随机化：鼠标点击前，随机移动几次，避免被检测为自动化操作。还有概率以鼠标拖拽出牌。
   - 鼠标空闲移动：鼠标空闲时（比如他人回合）一定概率随机移动。
   - 鼠标拖拽出牌：使用鼠标拖动代替点击来出牌。
   - AI选项随机化（去重）：自动打牌时，根据模型推荐的前三选项，按概率（权重）随机选取其中之一。可以在分析牌谱时，降低模型重合率。0 为关闭（仅选择权重最高项），5 为最高（按概率权重选择），1-4 的随机性位于中间。计算方式：取原概率的(5/n)次方，再归一概率，按概率选择。
   - 回复表情概率：以一定概率回复其他玩家的表情。打牌时不会触发；有最短触发间隔 (settings.json 中 auto_emoji_intervel 数值)。
   - 基础延迟随机范围：自动打牌前的延迟。根据场况和打的牌，会在基础延迟上增加额外延迟（例如：东家第一巡会加几秒钟延迟，让理牌动画完成）。

![Settings](\assets\images\settings.png){: height="50"}

### 模型配置

这里介绍不同模型类型和需要的配置。

1. Local: 兼容 Akagi 的本地 Mortal 模型。需要获取模型文件(.pth文件)并放到 `models` 目录下，并在设置中“本地模型文件”项，选择使用该模型文件。模型文件的获取和介绍，请参见 Akagi 作者 shinkuan 的 <a href="https://github.com/shinkuan/Akagi">Github</a> 和 <a href="https://discord.com/invite/Z2wjXUK8bN">Discord 频道</a>。
   
   三麻相关文件，限 Discord 频道的捐献者 (Donor) 获取。三麻模型设置步骤：
   1）获取三麻支持库 libriichi3p 相关库文件 (bot_3p_0.1.1.zip) 。解压后，将所有 libriichi3p 目录中的文件 (pyd 和 so 文件)放到 `libriichi3p` 目录下。
   2）获取三麻模型文件 (pth文件)，放到 `models` 目录下，并在程序设置项“本地模型文件(三麻)”选择该模型文件。
   3）模型支持的游戏类型会显示在状态栏，"模型: Local" 后的括号内。
2. MJAPI: (作者 9ns4esyx) 开发的在线麻将 AI API. 使用 MJAPI 时，可以填写已有的用户名和密钥。或者，将用户名和密钥 (Secret) 留空，软件会自动注册新用户并登录。MJAPI 有不同风格的模型可选择，登陆后会刷新模型选项，并显示 API 用量。暂时只支持四麻模式。
   4月13日更新的 MJAPI 地址：（临时域名有总连接数限制，而且不保证长久有效，之后也可能会变。服务器在国外，不同运营商的网络连通性不一，请自己测试）

- 临时域名：`https://cdt-authentication-consultation-significance.trycloudflare.com`
- 稳定域名：`https://mjai.7xcnnw11phu.eu.org`

### 账号安全提示

长时间使用自动打牌，以及牌谱 AI 重合率高，可能会导致账户被封禁。关闭自动打牌和 HUD 显示功能，并且打牌选择与 AI 推荐不同的选项，可以减少账号封禁风险。请将本程序用于学习和研究目的。使用者需要承担潜在风险和责任。

## 鸣谢 / Credit

- 基于 Mortal 模型和 MJAI 协议:
  <a href="https://github.com/Equim-chan/Mortal" target="_blank"> Mortal Github </a>
- 设计和功能实现基于 Akagi:
  <a href="https://github.com/shinkuan/Akagi" target="_blank"> Akagi Github </a>
- 参考项目: <a href="https://github.com/MahjongRepository/mahjong_soul_api" target="_blank"> Mahjong Soul API </a>
- MJAI协议参考:
  <a href="https://mjai.app" target="_blank"> mjai.app </a>

QQ群：834105526 <a target="_blank" href="https://qm.qq.com/cgi-bin/qm/qr?k=Mec5daqIyUsuZjCLojH_t88hQV6luPxl&jump_from=webapi&authKey=nNSpmIQY3ieVau/oLTF9eNO6YTqAm1+Ne3iE3zpqmFrj61iAUDu/GSpA38g93Zlx"><img border="0" src="//pub.idqqimg.com/wpa/images/group.png" alt="加入QQ群" title="麻将 Copilot"></a>

