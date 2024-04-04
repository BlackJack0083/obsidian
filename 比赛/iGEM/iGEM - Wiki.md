[iGEM竞赛 --- iGEM Competition](https://competition.igem.org/deliverables/team-wiki)

您的团队 Wiki 是一个向世界传达您的整个项目的网站。这包括提供**背景信息、描述项目目标和展示实验结果**。所有内容（文本、图像、iGEM团队和其他研究小组的先前工作）都必须正确引用和引用。团队 Wiki 是 iGEM 竞赛的关键成果，将于 12 月存档，以便未来的团队和 iGEM 社区成员可以看到您的作品。

### Wiki Freeze Wiki 冻结
22:00(CST) 10.2 (北京时间)

在维基冻结前的几天里，iGEM的流量高于正常水平，这可能导致我们的服务器紧张，用户会话超时。我们尽最大努力缓解这种压力，但在此期间可能有超过 6,000 名用户编辑他们的页面。用户不应该等到最后几天才开始工作和/或完成他们的wiki。在最后一周进行的编辑应该是非常小的编辑（即修复拼写错误、更新图像等）。

### 交付结果
[Project Attribution](https://competition.igem.org/deliverables/project-attribution)
[Project Description](https://competition.igem.org/deliverables/project-description)

### Requirements
#### 1. GitLab Repository link
在每个页面上，团队必须在页脚处添加一个清晰可见的**团队 iGEM GitLab 存储库链接**（例如，gitlab.igem.org/2024/example 示例团队）。这是为了确保未来的团队和评委能够轻松找到生成 wiki 的源代码。这也意味着团队必须使用他们在 iGEM GitLab （gitlab.igem.org） 上分配的存储库来创建他们的团队 wiki（**不接受其他 GitLab 或 Github 存储库**）。

gitlab: https://gitlab.igem.org/2024/example
#### 2. Licensing
所有由iGEM团队创建并托管在iGEM网站上的内容都必须在知识共享署名4.0许可（或任何更高版本）下提供，并且该许可必须**显示在每个页面的页脚**上。团队 wiki 存储库中的原始 LICENSE 文件必须保持不变。

外部资源，如受版权保护的图像、字体和其他材料，通常**不允许在iGEM维基上使用**，除非它们被适当地许可为重复使用，例如开源代码或知识共享许可的材料。

请注意，团队wiki上使用的任何外部资源都必须符合其各自的许可证，并且只有在允许重复使用和重新分发的情况下才被接受。使用外部资产时，请确保其许可证允许将其上传到 iGEM 的服务器（igem.org 和 igem.wiki 的任何子域）。
#### 3. Standard URL Pages Prizes and Medals Eligibility
我们创建了标准 URL 页面，其中包含用于特殊奖品和大多数奖牌标准的特定 URL 地址。这些标准网址页面会自动链接到您团队的评审表单。

如果您的团队希望获得特殊奖项或奖牌标准，您需要在相关的标准 URL 页面上记录您与奖项和/或奖牌标准相关的成就。
#### 4. All content must be hosted on iGEM's server
所有团队页面、图像、文档、代码，包括 CSS、JavaScript 和字体文件，都必须托管在 iGEM 服务器上，可在 igem.org 或 igem.wiki 的任何子域中使用

HTML 和团队编写的所有源代码必须提交到 iGEM GitLab 上的团队 Wiki 存储库;
图像、附加文档（.pdf、.csv等）、字体必须使用 uploads.igem.org 上传到 iGEM 自己的 CDN static.igem.wiki;

视频必须上传到 iGEM Video Universe 并从那里嵌入。请按照以下说明操作: https://tools.igem.org/wiki/non-deliverable-videos

托管在 igem.org 上的iGEM基金会内容也可以在团队wiki上使用。

- 使用[外部内容检查工具](https://tools.igem.org/wiki/external-content-check)验证 Wiki 页面的所有内容和请求是否都符合此要求。如果未能通过此检查，您的团队将失去最佳 Wiki 和最佳软件工具奖的资格。
- 评委不会评估iGEM服务器之外的任何内容。此外，在 iGEM 服务器之外托管内容是一种**作弊形式**，可能不会获得奖牌，这包括外部 CDN 和外部字体提供商。

>[!note] 注意
> 包管理器注意事项：当我们迁移到 GitLab 时，使用库的首选方式是通过包管理器（npm、yarn、pip）。在幕后，包管理器下载库，构建团队 wiki 并在我们的服务器上发布生成的资产。由于此选项需要更多的编码经验，因此另一种方法是**使用 uploads.igem.org 下载并提供库的 igem-server 存储版本**。

#### 5. iframes can only be used to show content hosted on iGEM servers
您可以使用 iframe 显示您上传到 iGEM 服务器（igem.org 或 igem.wiki）或 iGEM Video Universe （video.igem.org） 的文件。iGEM wiki 上不允许 iframe 显示来自其他网站或服务器的内容（见上文）。iframe 中不允许的一些常见示例包括 YouTube 或 Bilibili 上托管的视频，以及 Google Drive 或 Microsoft Teams 上托管的文档。

> `<iframe>` 是 HTML 中的一个标签，用于在当前页面中嵌入另一个页面。它可以在网页中创建一个内联框架（inline frame），用来显示来自同一服务器或者其他服务器的网页内容。

>通过 `<iframe>` 标签，你可以将其他网页嵌入到当前页面中，这样用户就可以在当前页面中直接浏览嵌入的页面，而无需离开当前页面。这种技术通常用于嵌入地图、视频、广告等内容。
#### 6. All source code used to _generate_ your wiki must be provided in your GitLab repository  
在创建 wiki 时，如果您决定使用不同的 HTML、CSS 和 JS 生成器而不是提供的默认模板，则需要在 GitLab 存储库中**共享源代码**。这允许评委、其他团队和任何感兴趣的人在未来重复使用代码来生成他们的 wiki。通过这样做，我们可以培养一个强大的社区，该社区年复一年地不断改进。
### URL naming convention URL 命名约定
iGEM 采用了一种新的 URL 命名约定：为了提高可用性，页面 URL 应为小写，并且除 和 `a-z` 之外 `0-9` 的所有内容都替换为 `-` .没有前导/尾随 `-` 。例如，以前的格式 `2021.igem.org/Team:TeamName/Page_Name` 变为 `2024.igem.wiki/team-name/page-name`

### Using Software Tools or other git repositories  使用软件工具或其他 git 存储库
如果您使用单独的程序或软件工具（即 Dreamweaver、Github 存储库）创建 Wiki 页面，强烈建议您**测试将代码导入 Team Wiki 存储库**。导入在其他程序中设计或托管在其他 git 平台上的页面通常需要时间和精力来了解如何将正确的文件带过来。不要等到 Wiki 冻结的那一周才测试一下！您可能会遇到严重的问题，并且可能需要比预期更长的时间才能正确上传和构建您的网站。

### React requirements React 需求
如果你使用 React 来构建你的团队 wiki，你必须将 `{ "homepage": "/team-name" }` （`team-name`指你的团队 Wiki 的基本 URL 在哪里  ）添加到 `package.json`文件中。您还必须设置 `basename={({}).PUBLIC_URL}` 路线。


