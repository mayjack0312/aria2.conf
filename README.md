# Aria2 完美配置

[![LICENSE](https://img.shields.io/github/license/mashape/apistatus.svg?style=flat-square&label=License)](https://github.com/P3TERX/aria2.conf/blob/master/LICENSE)
[![GitHub Stars](https://img.shields.io/github/stars/P3TERX/aria2.conf.svg?style=flat-square&label=Stars&logo=github)](https://github.com/P3TERX/aria2.conf/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/P3TERX/aria2.conf.svg?style=flat-square&label=Forks&logo=github)](https://github.com/P3TERX/aria2.conf/fork)

一套 Aria2 配置方案，包含了配置文件、附加功能脚本等文件，用于实现 Aria2 功能的增强和扩展，提升 Aria2 的下载速度与使用体验。

## 功能特性

* BT 下载率高、速度快
* 重启后不丢失任务进度、不重复下载
* 删除正在下载的任务自动删除未完成的文件
* 下载错误自动删除未完成的文件
* 下载完成自动删除控制文件(`.aria2`后缀名文件)
* 下载完成自动删除种子文件(`.torrent`后缀名文件)
* 下载完成自动删除空目录
* 下载完成自动移动文件到指定目录
* 下载完成自动上传到 Google Drive 和 OneDrive 等网盘(RCLONE 联动功能)
* BT 下载完成自动清除垃圾文件(文件类型过滤功能)
* BT 下载完成自动清除小文件(文件大小过滤功能)
* 一键自动更新 BT tracker，进一步加速 BT 下载
* 有一定的防版权投诉、防迅雷吸血效果
* 更好的 PT 下载支持

## 进阶玩法

* [高级进阶组合玩法——Aria2+Rclone实现OneDrive、Google Drive等网盘离线下载](https://github.com/mayjack0312/my-blog/blob/main/%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6%E7%BB%84%E5%90%88%E7%8E%A9%E6%B3%95%E2%80%94%E2%80%94Aria2%2BRclone%E5%AE%9E%E7%8E%B0OneDrive%E3%80%81Google%20Drive%E7%AD%89%E7%BD%91%E7%9B%98%E7%A6%BB%E7%BA%BF%E4%B8%8B%E8%BD%BD.md)
* [附加玩法——百度网盘转存OneDrive、Google Drive等其他网盘](https://github.com/mayjack0312/my-blog/blob/main/%E9%99%84%E5%8A%A0%E7%8E%A9%E6%B3%95%E2%80%94%E2%80%94%E7%99%BE%E5%BA%A6%E7%BD%91%E7%9B%98%E8%BD%AC%E5%AD%98OneDrive%E3%80%81Google%20Drive%E7%AD%89%E5%85%B6%E4%BB%96%E7%BD%91%E7%9B%98.md)

## 文件说明

> **TIPS:** 附加功能脚本需配合配置文件使用，仅适用于 GNU/Linux ，理论上 macOS 安装 GNU 工具套件可以使用。依赖：`bash`、`curl`、`findutils`、`jq`

| 文件                    | 说明                                                                                                                                                                                                                                                                                      |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `aria2.conf`            | Aria2 配置文件。建议配合 1.35.0 及以上版本使用。内附可能比官方文档还详细的中文注释说明和修改建议，在不了解的情况下随意修改可能导致本方案的特性失效。                                                                                                                                      |
| `script.conf`           | Aria2 附加功能脚本配置文件。                                                                                                                                                                                                                                                              |
| `core`                  | Aria2 附加功能脚本核心文件。所有脚本都依赖于此文件运行。                                                                                                                                                                                                                                  |
| `clean.sh`              | 文件清理脚本。在下载完成后执行([on-download-complete](https://aria2.github.io/manual/en/html/aria2c.html#cmdoption-on-download-complete))，自动清理控制文件(`*.aria2`)、种子文件(`*.torrent`)和空目录。（默认启用）                                                                       |
| `delete.sh`             | 文件删除脚本。在下载停止后执行([on-download-stop](https://aria2.github.io/manual/en/html/aria2c.html#cmdoption-on-download-stop))，当下载错误或删除正在下载的任务后自动删除相关文件，并自动清理控制文件(`*.aria2`)、种子文件(`*.torrent`)和空目录，防止不必要的磁盘空间占用。（默认启用） |
| `upload.sh`             | 文件上传脚本。在下载完成后执行([on-download-complete](https://aria2.github.io/manual/en/html/aria2c.html#cmdoption-on-download-complete))，自动调用 RCLONE 上传(move)下载完成的文件到网盘，并自动清理控制文件(`*.aria2`)、种子文件(`*.torrent`)和空目录。（默认不启用）                   |
| `rclone.env`            | [RCLONE 环境变量](https://rclone.org/docs/#environment-variables)文件。配合`upload.sh`使用，用于配置 RCLONE 的选项参数。                                                                                                                                                                  |
| `move.sh`               | 文件移动脚本。在下载完成后执行([on-download-complete](https://aria2.github.io/manual/en/html/aria2c.html#cmdoption-on-download-complete))，自动将下载完成的文件移动到指定目录，并自动清理控制文件(`*.aria2`)、种子文件(`*.torrent`)和空目录。（默认不启用）                               |
| `tracker.sh`            | BT tracker 列表更新脚本。在 Aria2 配置文件(`aria2.conf`)所在目录执行即可获取[最新 tracker 列表](https://raw.githubusercontent.com/XIU2/TrackersListCollection/master/all.txt)并添加到配置文件中。其它使用方法详见 [tracker.md](./tracker.md)                                              |
| `dht.dat`<br>`dht6.dat` | DHT 网络节点数据文件。提升 BT 下载率和下载速度的关键之一。|
