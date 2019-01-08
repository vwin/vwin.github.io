---
title: Jenkins高效插件整理
toc: true
date: 2019-01-07 05:18:35
tags: [Jenkins,插件]
categories: [持续集成]
description:
---
开源版本的Jenkins 具有三大能力：Master-Slave的分布式构建调度能力、Pipeline编排能力、强大的开源生态（插件）能力。

2017年4月，Jenkins创始人KK（Kohsuke Kawaguchi ）来到中国，交流中他也明确表示Jenkins的成功主要取决于其开源生态系统，Jenkins有1400多个插件可供使用。因为有开源的插件生态系统的存在，Jenkins要用得好，插件一定是不能少的，需要我们充分发现和使用插件来实现我们的需求，而不是重复造轮子，自己去实现。

但是面对林林总总的插件，到底该怎么选？我的常用需求有哪些插件可以满足，笔者根据以往在企业中管理Jenkins的经验推荐如下常用的插件列表，希望大家基于Jenkins及其插件生态实现自己的持续交付与DevOps平台。
<!--more-->

# 用户及权限
Jenkins 用户权限管理是Jenkins Administration中非常很重要的环节，由于大部分企业都会有自己的域控管理，所以和LDAP集成并基于用户组实现权限模型设计与管理是企业级Jenkins实践的重要内容。
1. [LDAP](https://plugins.jenkins.io/ldap) 这个插件允许使用LDAP对用户进行认证，LDAP 服务器可以为Active Directory 或者 OpenLDAP。
2. [Active Directory](https://plugins.jenkins.io/active-directory)  这个插件允许使用Active Directory对用户进行认证，同时结合诸如Matrix Authorization Strategy插件，可以识别用户所在的所有用户组，对用户授权进行灵活配置。
基于Windows Active Directory进行域管理的企业，推荐采用Active Directory。
3. [GitHub Authentication](https://plugins.jenkins.io/github-oauth) 这个插件提供了使用GitHub进行用户认证和授权的方案。
4. [Gitlab Authentication](https://plugins.jenkins.io/gitlab-oauth) 这个插件提供了使用GitLab进行用户认证和授权的方案。
5. [Matrix Authorization Strategy](https://plugins.jenkins.io/matrix-auth) 这个插件提供了基于矩阵的授权策略，支持全局和项目级别的配置。
6. [Role-based Authorization Strategy](https://plugins.jenkins.io/role-strategy) 这个插件提供了一种基于角色（Role）的用户权限管理策略，支持创建global角色、Project角色、Slave角色，以及给用户分配这些角色。这款插件是最常用的Jenkins权限策略和管理插件。

# 代码管理
Jenkins 项目中配置Source Code Management 去下载代码进行构建任务，是非常普遍的应用场景。Jenkins插件支持很多SCM的系统，使用最常见的是Git 和SVN。

1. [Git](https://plugins.jenkins.io/git) 支持使用Github、GitLab、Gerrit等系统管理代码仓库。
2. [Subversion](https://plugins.jenkins.io/subversion) 支持Subversion系统管理源代码。

# 项目及视图
Jenkins中对Project 和 view的管理，是用户日常工作中使用很多的功能。

1. [Folder](https://plugins.jenkins.io/cloudbees-folder) 这个插件支持用户使用目录管理项目，目录支持嵌套，并且支持目录中创建视图。
2. List view Jenkins 默认支持List类型的视图，用户可以创建List视图过滤所关心的项目。
3. [Sectioned View](https://plugins.jenkins.io/sectioned-view) 这个插件支持一种新的视图，视图可以分为多个部分，每部分可以单独配置显示所选择的项目信息。
4. [Nested View](https://plugins.jenkins.io/nested-view) 这个插件支持一种新的视图，其表示直接显示项目，而是以目录图标显示所包含的子视图，每个子视图显示所选项目信息。
5. [Build Pipeline](https://plugins.jenkins.io/build-pipeline-plugin) 这个插件提供了一种Build Pipeline 视图，用于显示上、下游项目构建的关系。

# 构建触发
Jenkins支持多种Build 触发方式，尤其一些自动化触发方式非常有用

1. Build periodically，Jenkins 内置功能，可以设置类似crontab时间，周期性地自动触发构建。
Poll SCM，Jenkins 内置功能，类似Build periodically，可以设置类似crontab时间，不同的是不是直接进行构建，而是周期性地在后台检查所配置的SCM有没有更新，只有当有代码更新时才会触发构建。
2. Trigger builds remotely (e.g., from scripts)，Jenkins 内置功能，远程触发构建，通过设置token可以支持远程脚本中触发Jenkins构建。
3. [Gerrit Trigger](https://plugins.jenkins.io/gerrit-trigger) 这个插件将Jenkins集成到Gerrit code review中，支持Jenkins配置Gerrit服务器等信息，实现Gerrit event 触发Jenkins 构建。
4. [GitLab](https://plugins.jenkins.io/gitlab-plugin) 这个插件将Jenkins 集成到GitLab web hook中，支持Gitlab 分支及Merge Request等相关事件触发Jenkins构建。
5. [GitHub Integration](https://plugins.jenkins.io/github-pullrequest) 这个插件将Jenkins集成到GitHub中，支持Gitgub分支及Pull requests 触发Jenkins 构建。
6. [JIRA Trigger](https://plugins.jenkins.io/jira-trigger) 这个插件将Jenkins集成到Jira WebHooks中，支持Jira issue的状态等变化时触发Jenkins构建。

# 构建参数
Jenkins除了支持普通的参数类型（布尔型、字符串型、多行文本型、选择型和文件型 ）外，还有一些插件支持更加丰富实用的参数类型，比如参数间动态关联、多层级参数、隐藏参数等 。

1. nodelabelparameter  https://plugins.jenkins.io/nodelabelparameter，这个插件增加了一个新的参数类型，Node 和 Label，从而使用户通过参数可以选择项目构建运行的节点。
其他插件不一一列举，可以查看插件说明
   - https://plugins.jenkins.io/hidden-parameter
   - https://plugins.jenkins.io/extended-choice-parameter
   - https://plugins.jenkins.io/validating-string-parameter
   - https://plugins.jenkins.io/extensible-choice-parameter
   - https://wiki.jenkins.io/display/JENKINS/Active+Choices+Plugin

# 构建任务及环境
围绕构建任务，有许多小的插件，却提供了一些实用的功能

1. [Workspace Cleanup](https://plugins.jenkins.io/ws-cleanup) 这个插件支持在构建前后 删除或者部分删除workspace
2. [description setter](https://plugins.jenkins.io/description-setter) 这个插件支持正则表达式匹配构建log输出，设置构建的描述
3. [build-name-setter](https://plugins.jenkins.io/build-name-setter) 这个插件支持设置构建的显示名字，而不是默认的为#1，#2，……，#buildnum
4. [Environment Injector](https://plugins.jenkins.io/envinject) 这个插件支持在构建任务的不同阶段插入环境变量，并且在构建结束导出所有的环境变量等功能。

# 构建通知
把构建状态及时地通知用户，是Jenkins的一个必不可少的功能。Jenkins支持多种主动和被动的通知方式。

1. [Mailer](https://plugins.jenkins.io/mailer) 这个插件支持基本的邮件通知功能，比如构建失败和构建恢复成功可以发送邮件通知给相关人员。
2. [Email Extension](https://plugins.jenkins.io/email-ext) 这个插件是邮件通知的扩展，支持定制邮件内容，触发条件以及邮件接收者，功能比基本邮件通知要灵活强大的多。
3. [Slack Notification](https://plugins.jenkins.io/slack) 这个插件支持把构建结果推送到Slack channel。

# 容器化Slave
Jenkins的Master-Slave架构实现了分布式构建，可以充分的横向扩展Slave来提升构建能力，将Slave容器化是目前主流的构建环境标准化、集群化和弹性化的方式。

1. https://plugins.jenkins.io/docker-plugin 这个插件可以配置docker host ，从而动态的提供Jenkins Agent（Slave），运行构建后再销毁这个slave。
2. https://plugins.jenkins.io/kubernetes 这个插件支持利用Kubernetes  cluster 动态的提供Jenkins Agent（Slave），利用Kubernetes 调度机制来优化Jenkins 负载等。

# Admin相关插件
1. Configuration Slicing  https://plugins.jenkins.io/configurationslicing 这个插件支持批量修改项目配置
2. Mask Passwords https://plugins.jenkins.io/mask-passwords 这个插件支持遮挡构建log输出的password等敏感信息
3. Backup https://plugins.jenkins.io/backup 这个插件添加备份功能到Jenkins management
