---
title: API Checklist
permalink: Develop/Apps/Harbour/API_Checklist/
parent: Harbour
grand_parent: Apps
layout: default
nav_order: 100
---

# 在 Harbour 允许的列表中添加一个 API

我们不时收到要求，允许在 Harbour 中批准的应用程序中使用某些 API 的请求。

尽管我们希望允许尽可能多的API，但我们必须采取一些预防措施。我们应该明白，API在应用程序和底层框架之间形成了一个合同。一个应用程序期望框架会实现文档中的功能，并且至少在一段时间内持续这样做。

为了简化决定我们是否应该将一个API添加到允许的列表中的过程，我们整理了以下检查表。一般来说，如果一个API满足了所有的检查点，我们就没有理由不允许它。另一方面，每一个API都应该被单独考虑--在某些情况下，做出例外是合理的，也就是说，即使某些要点没有得到满足，我们也可以添加一个API。

## API检查表

  - API应该被认为是稳定的
      - 如果以下情况属实，一个API可以被认为是稳定的。
          - API被上游认为是稳定的
          - API已经一年没有变化了
          - 我们不打算在不久的将来将API升级到不兼容的版本。
      - 例如，我们可以开放官方的Qt模块，并承诺与API兼容。

<!-- end list -->

  - API不被认为是废弃的

<!-- end list -->

  - API不能暴露敏感的用户数据

<!-- end list -->

  - API需要是可测试的
      - 最好有自动测试
      - 如果我们没有测试案例，至少要有一个使用该API的应用程序，可以用于验证。

<!-- end list -->

  - API需要为预期的用途工作

<!-- end list -->

  - API在发布前由水手（最好是区域所有者）审查

<!-- end list -->

  - 存在API文档并且可以发布
      - 记录特定平台的限制（例如，Sailfish OS缺乏对更高级的QtFeedback触觉功能的支持）

<!-- end list -->

  - API对多个应用程序是有用的
      - 这并不是要求我们的图像中包含这样的应用程序

<!-- end list -->

  - API必须已经存在于我们的存储库中

<!-- end list -->

  - 如果我们已经有了用于特定目的的C++/QML API，我们就不应该允许相同目的的C API。
      - 低级别的C语言API通常是不允许的

<!-- end list -->

  - Sailfish OS是用C、C++和QML编写的，没有新的语言运行机制
