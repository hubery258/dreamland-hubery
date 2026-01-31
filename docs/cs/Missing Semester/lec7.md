# lec7: Debugging and Profiling

性能分析晚点再考虑，记录一点调试问题。

## 调试代码

### 打印调试与日志

无需多言

### 调试器

调试器很多功能vscode都可以装不同语言插件完成。

### 静态分析

静态分析会将程序的源码作为输入然后基于规则对其进行分析并对代码的正确性进行推理，也就是代码检查，vscode基本也能直接显示。

风格检查就是检查代码的风格是否规范，也有[**代码格式化工具**](https://missing-semester-cn.github.io/2020/debugging-profiling/#:~:text=%E4%B8%8E%E9%A3%8E%E6%A0%BC%E6%A3%80%E6%9F%A5%E7%9B%B8%E8%BE%85%E7%9B%B8%E6%88%90%E7%9A%84%E6%98%AF%E4%BB%A3%E7%A0%81%E6%A0%BC%E5%BC%8F%E5%8C%96%E5%B7%A5%E5%85%B7%EF%BC%88code%20formatters%EF%BC%89%EF%BC%8C%E4%BE%8B%E5%A6%82%20Python%20%E7%9A%84%20black%E3%80%81Go%20%E8%AF%AD%E8%A8%80%E7%9A%84%20gofmt%E3%80%81Rust%20%E7%9A%84%20rustfmt%20%E4%BB%A5%E5%8F%8A%20JavaScript%E3%80%81HTML%20%E5%92%8C%20CSS%20%E7%9A%84%20prettier%E3%80%82%E8%BF%99%E4%BA%9B%E5%B7%A5%E5%85%B7%E4%BC%9A%E8%87%AA%E5%8A%A8%E6%A0%BC%E5%BC%8F%E5%8C%96%E6%82%A8%E7%9A%84%E4%BB%A3%E7%A0%81%EF%BC%8C%E4%BD%BF%E5%85%B6%E7%AC%A6%E5%90%88%E8%AF%A5%E7%BC%96%E7%A8%8B%E8%AF%AD%E8%A8%80%E9%80%9A%E7%94%A8%E7%9A%84%E9%A3%8E%E6%A0%BC%E8%A7%84%E8%8C%83%E3%80%82%E8%99%BD%E7%84%B6%E6%82%A8%E5%8F%AF%E8%83%BD%E4%B8%8D%E5%A4%AA%E6%83%85%E6%84%BF%E5%B0%86%E4%BB%A3%E7%A0%81%E9%A3%8E%E6%A0%BC%E7%9A%84%E6%8E%A7%E5%88%B6%E6%9D%83%E4%BA%A4%E7%BB%99%E5%B7%A5%E5%85%B7%EF%BC%8C%E4%BD%86%E6%A0%87%E5%87%86%E5%8C%96%E7%9A%84%E4%BB%A3%E7%A0%81%E6%A0%BC%E5%BC%8F%E4%B8%8D%E4%BB%85%E6%9C%89%E5%8A%A9%E4%BA%8E%E4%BB%96%E4%BA%BA%E9%98%85%E8%AF%BB%E6%82%A8%E7%9A%84%E4%BB%A3%E7%A0%81%EF%BC%8C%E4%B9%9F%E8%83%BD%E8%AE%A9%E6%82%A8%E6%9B%B4%E8%BD%BB%E6%9D%BE%E5%9C%B0%E9%98%85%E8%AF%BB%E4%BB%96%E4%BA%BA%EF%BC%88%E5%90%8C%E6%A0%B7%E7%BB%8F%E8%BF%87%E6%A0%BC%E5%BC%8F%E5%8C%96%EF%BC%89%E7%9A%84%E4%BB%A3%E7%A0%81%E3%80%82)

[静态分析工具列表](https://github.com/analysis-tools-dev/static-analysis)<br>
[代码检查工具](https://github.com/caramelomartins/awesome-linters)

## 性能分析

计时只需要`time`指令，时间有三种:
- 真实时间 **Real** :程序从开始到结束流逝的墙上时间，包括其他进程使用的时间以及阻塞（例如等待 I/O 或网络）的时间
- 用户时间 **User** :CPU 执行用户态代码所花费的时间
- 系统时间 **Sys** :CPU 执行内核态代码所花费的时间

例如:
``` bash
$ time curl https://missing.csail.mit.edu &> /dev/null
real    0m2.561s
user    0m0.015s
sys     0m0.012s
```
请求花费了 2 秒多才完成，但是进程仅花费了 15 毫秒的 CPU 用户时间和 12 毫秒的 CPU 内核时间,网络就不太好。<br>

[原文](https://missing-semester-cn.github.io/2020/debugging-profiling/)

## 补充与说明

lec7之后的lec8~10感觉都走远了...没有看到在大一完成的必要性与紧急性，这个笔记暂时宣告完结，哪天需要又会回来更新吧。

补充一个lec10笔记里提到的知识点:

**API（应用程序接口）**<br>
关于如何使用计算机有效率地完成 本地 任务，我们这堂课已经介绍了很多方法。这些方法在互联网上其实也适用。大多数线上服务提供的 API（应用程序接口）让你可以通过编程方式来访问这些服务的数据。比如，美国国家气象局就提供了一个可以从 shell 中获取天气预报的 API。

这些 API 大多具有类似的格式。它们的结构化 URL 通常使用 `api.service.com` 作为根路径，用户可以访问不同的子路径来访问需要调用的操作，以及添加查询参数使 API 返回符合查询参数条件的结果。

以美国天气数据为例，为了获得某个地点的天气数据，你可以发送一个 `GET` 请求（比如使用 curl）到 (https://api.weather.gov/points/42.3604,-71.094)。返回中会包括一系列用于获取特定信息（比如小时预报、气象观察站信息等）的 URL。通常这些返回都是 `JSON` 格式，你可以使用 [jq](https://stedolan.github.io/jq/) 等工具来选取需要的部分。

有些需要认证的 API 通常要求用户在请求中加入某种私密令牌（secret token）来完成认证。请阅读你想访问的 API 所提供的文档来确定它请求的认证方式，但是其实大多数 API 都会使用 [OAuth](https://www.oauth.com/)。OAuth 通过向用户提供一系列仅可用于该 API 特定功能的私密令牌进行校验。因为使用了有效 OAuth 令牌的请求在 API 看来就是用户本人发出的请求，所以请一定保管好这些私密令牌。否则其他人就可以冒用你的身份进行任何你可以在这个 API 上进行的操作。

[IFTTT](https://ifttt.com/) 这个网站可以将很多 API 整合在一起，让某 API 发生的特定事件触发在其他 API 上执行的任务。IFTTT 的全称 If This Then That 足以说明它的用法，比如在检测到用户的新推文后，自动发布在其他平台。但是你可以对它支持的 API 进行任意整合，所以试着来设置一下任何你需要的功能吧！

最后的最后，或许最珍贵的思想是尽可能提高效率，尽可能让一些任务自动化完成，尽可能有一个清晰的工作流。