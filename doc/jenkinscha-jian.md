开源版本的Jenkins 具有三大能力：Master-Slave的分布式构建调度能力、Pipeline编排能力、强大的开源生态（插件）能力。

2017年4月，Jenkins创始人KK（Kohsuke Kawaguchi ）来到中国，交流中他也明确表示Jenkins的成功主要取决于其开源生态系统，Jenkins有1400多个插件可供使用。因为有开源的插件生态系统的存在，Jenkins要用得好，插件一定是不能少的，需要我们充分发现和使用插件来实现我们的需求，而不是重复造轮子，自己去实现。

但是面对林林总总的插件，到底该怎么选？我的常用需求有哪些插件可以满足，笔者根据以往在企业中管理Jenkins的经验推荐如下常用的插件列表，希望大家基于Jenkins及其插件生态实现自己的持续交付与DevOps平台。

### 用户及权限

Jenkins 用户权限管理是Jenkins Administration中非常很重要的环节，由于大部分企业都会有自己的域控管理，所以和LDAP集成并基于用户组实现权限模型设计与管理是企业级Jenkins实践的重要内容。

* l  LDAP （
  [https://plugins.jenkins.io/ldap](https://plugins.jenkins.io/ldap)
  ），这个插件允许使用LDAP对用户进行认证，LDAP 服务器可以为Active Directory 或者 OpenLDAP。
* l  Active Directory 
  [https://plugins.jenkins.io/active-directory](https://plugins.jenkins.io/active-directory)
  ，这个插件允许使用Active Directory对用户进行认证，同时结合诸如Matrix Authorization Strategy插件，可以识别用户所在的所有用户组，对用户授权进行灵活配置。
* l  基于
  [Windows](http://www.ttlsa.com/windows/)
   Active Directory进行域管理的企业，推荐采用Active Directory。
* l  GitHub Authentication 
  [https://plugins.jenkins.io/github-oauth](https://plugins.jenkins.io/github-oauth)
  ，这个插件提供了使用GitHub进行用户认证和授权的方案。
* l  Gitlab Authentication 
  [https://plugins.jenkins.io/gitlab-oauth](https://plugins.jenkins.io/gitlab-oauth)
  ，这个插件提供了使用GitLab进行用户认证和授权的方案。
* l  Matrix Authorization Strategy 
  [https://plugins.jenkins.io/matrix-auth](https://plugins.jenkins.io/matrix-auth)
  ，这个插件提供了基于矩阵的授权策略，支持全局和项目级别的配置。
* l  Role-based Authorization Strategy 
  [https://plugins.jenkins.io/role-strategy](https://plugins.jenkins.io/role-strategy)
  ，这个插件提供了一种基于角色（Role）的用户权限管理策略，支持创建global角色、Project角色、Slave角色，以及给用户分配这些角色。这款插件是最常用的Jenkins权限策略和管理插件。

### 代码管理

Jenkins 项目中配置Source Code Management 去下载代码进行构建任务，是非常普遍的应用场景。Jenkins插件支持很多SCM的系统，使用最常见的是Git 和SVN。

* l  Git 
  [https://plugins.jenkins.io/git](https://plugins.jenkins.io/git)
  ，支持使用Github、GitLab、Gerrit等系统管理代码仓库。
* l  Subversion 
  [https://plugins.jenkins.io/subversion](https://plugins.jenkins.io/subversion)
  ，支持Subversion系统管理源代码。

### 项目及视图

Jenkins中对Project 和 view的管理，是用户日常工作中使用很多的功能。

* l  Folder 
  [https://plugins.jenkins.io/cloudbees-folder](https://plugins.jenkins.io/cloudbees-folder)
  ，这个插件支持用户使用目录管理项目，目录支持嵌套，并且支持目录中创建视图。
* l  List view Jenkins 默认支持List类型的视图，用户可以创建List视图过滤所关心的项目。
* l  Sectioned View 
  [https://plugins.jenkins.io/sectioned-view](https://plugins.jenkins.io/sectioned-view)
  ，这个插件支持一种新的视图，视图可以分为多个部分，每部分可以单独配置显示所选择的项目信息。
* l   Nested View 
  [https://plugins.jenkins.io/nested-view](https://plugins.jenkins.io/nested-view)
  ，这个插件支持一种新的视图，其表示直接显示项目，而是以目录图标显示所包含的子视图，每个子视图显示所选项目信息。
* l  Build Pipeline 
  [https://plugins.jenkins.io/build-pipeline-plugin](https://plugins.jenkins.io/build-pipeline-plugin)
  ，这个插件提供了一种Build Pipeline 视图，用于显示上、下游项目构建的关系。

### 构建触发

Jenkins支持多种Build 触发方式，尤其一些[自动化](http://www.ttlsa.com/auto/)触发方式非常有用

* l  Build periodically，Jenkins 内置功能，可以设置类似crontab时间，周期性地自动触发构建。
* l  Poll SCM，Jenkins 内置功能，类似Build periodically，可以设置类似crontab时间，不同的是不是直接进行构建，而是周期性地在后台检查所配置的SCM有没有更新，只有当有代码更新时才会触发构建。
* l  Trigger builds remotely \(e.g., from scripts\)，Jenkins 内置功能，远程触发构建，通过设置token可以支持远程脚本中触发Jenkins构建。
* l  Gerrit Trigger 
  [https://plugins.jenkins.io/gerrit-trigger](https://plugins.jenkins.io/gerrit-trigger)
  ，这个插件将Jenkins集成到Gerrit code review中，支持Jenkins配置Gerrit服务器等信息，实现Gerrit event 触发Jenkins 构建。
* l  GitLab 
  [https://plugins.jenkins.io/gitlab-plugin](https://plugins.jenkins.io/gitlab-plugin)
  ，    这个插件将Jenkins 集成到GitLab web hook中，支持Gitlab 分支及Merge Request等相关事件触发Jenkins构建。
* l  GitHub Integration 
  [https://plugins.jenkins.io/github-pullrequest](https://plugins.jenkins.io/github-pullrequest)
   ，这个插件将Jenkins集成到GitHub中，支持Gitgub分支及Pull requests 触发Jenkins 构建。
* l  JIRA Trigger 
  [https://plugins.jenkins.io/jira-trigger](https://plugins.jenkins.io/jira-trigger)
  ，这个插件将Jenkins集成到Jira WebHooks中，支持Jira issue的状态等变化时触发Jenkins构建。

### 构建参数

Jenkins除了支持普通的参数类型（布尔型、字符串型、多行文本型、选择型和文件型 ）外，还有一些插件支持更加丰富实用的参数类型，比如参数间动态关联、多层级参数、隐藏参数等 。

* l  nodelabelparameter  
  [https://plugins.jenkins.io/nodelabelparameter](https://plugins.jenkins.io/nodelabelparameter)
  ，这个插件增加了一个新的参数类型，Node 和 Label，从而使用户通过参数可以选择项目构建运行的节点。
* l  其他插件不一一列举，可以查看插件说明

Ø  [https://plugins.jenkins.io/hidden-parameter](https://plugins.jenkins.io/hidden-parameter)

Ø  [https://plugins.jenkins.io/extended-choice-parameter](https://plugins.jenkins.io/extended-choice-parameter)

Ø  [https://plugins.jenkins.io/validating-string-parameter](https://plugins.jenkins.io/validating-string-parameter)

Ø  [https://plugins.jenkins.io/extensible-choice-parameter](https://plugins.jenkins.io/extensible-choice-parameter)

Ø  [https://wiki.jenkins.io/display/JENKINS/Active+Choices+Plugin](https://wiki.jenkins.io/display/JENKINS/Active+Choices+Plugin)

### 构建任务及环境

围绕构建任务，有许多小的插件，却提供了一些实用的功能

* l  Workspace Cleanup 
  [https://plugins.jenkins.io/ws-cleanup](https://plugins.jenkins.io/ws-cleanup)
  ，这个插件支持在构建前后 删除或者部分删除workspace
* l  description setter 
  [https://plugins.jenkins.io/description-setter](https://plugins.jenkins.io/description-setter)
  ，这个插件支持正则表达式匹配构建log输出，设置构建的描述
* l  build-name-setter 
  [https://plugins.jenkins.io/build-name-setter](https://plugins.jenkins.io/build-name-setter)
  ，这个插件支持设置构建的显示名字，而不是默认的为\#1，\#2，……，\#buildnum
* l  Environment Injector 
  [https://plugins.jenkins.io/envinject](https://plugins.jenkins.io/envinject)
  ，这个插件支持在构建任务的不同阶段插入环境变量，并且在构建结束导出所有的环境变量等功能。

### 构建通知

把构建状态及时地通知用户，是Jenkins的一个必不可少的功能。Jenkins支持多种主动和被动的通知方式。

* l  Mailer  
  [https://plugins.jenkins.io/mailer](https://plugins.jenkins.io/mailer)
  ，这个插件支持基本的邮件通知功能，比如构建失败和构建恢复成功可以发送邮件通知给相关人员。
* l  Email Extension 
  [https://plugins.jenkins.io/email-ext](https://plugins.jenkins.io/email-ext)
  ，这个插件是邮件通知的扩展，支持定制邮件内容，触发条件以及邮件接收者，功能比基本邮件通知要灵活强大的多。
* l  Slack Notification 
  [https://plugins.jenkins.io/slack](https://plugins.jenkins.io/slack)
  ，这个插件支持把构建结果推送到Slack channel。

### 容器化Slave

Jenkins的Master-Slave架构实现了分布式构建，可以充分的横向扩展Slave来提升构建能力，将Slave容器化是目前主流的构建环境标准化、集群化和弹性化的方式。

* l  
  [https://plugins.jenkins.io/docker-plugin](https://plugins.jenkins.io/docker-plugin)
  ，这个插件可以配置docker host ，从而动态的提供Jenkins Agent（Slave），运行构建后再销毁这个slave。
* l  
  [https://plugins.jenkins.io/kubernetes](https://plugins.jenkins.io/kubernetes)
  ，     这个插件支持利用Kubernetes  cluster 动态的提供Jenkins Agent（Slave），利用Kubernetes 调度机制来优化Jenkins 负载等。

### Admin相关插件

* l  Configuration Slicing  
  [https://plugins.jenkins.io/configurationslicing](https://plugins.jenkins.io/configurationslicing)
  ，这个插件支持批量修改项目配置
* l  Mask Passwords 
  [https://plugins.jenkins.io/mask-passwords](https://plugins.jenkins.io/mask-passwords)
  ，这个插件支持遮挡构建log输出的password等敏感信息
* l  Backup 
  [https://plugins.jenkins.io/backup](https://plugins.jenkins.io/backup)
  ，这个插件添加备份功能到Jenkins management

### Company Plugin

```
 ace-editor	1.1	true
all-changes	1.5	true
analysis-core	1.95	true
android-emulator	3.0	true
android-lint	2.6	true
ansible	1.0	true
ansicolor	0.5.2	true
ant	1.8	true
antisamy-markup-formatter	1.5	true
apache-httpcomponents-client-4-api	4.5.3-2.1	true
artifactory	2.15.1	true
audit-trail	2.2	true
authentication-tokens	1.3	true
autocomplete-parameter	1.0	true
backup	1.6.1	true
blueocean	1.5.0	true
blueocean-autofavorite	1.2.2	true
blueocean-bitbucket-pipeline	1.5.0	true
blueocean-commons	1.5.0	true
blueocean-config	1.5.0	true
blueocean-core-js	1.5.0	true
blueocean-dashboard	1.5.0	true
blueocean-display-url	2.2.0	true
blueocean-events	1.5.0	true
blueocean-git-pipeline	1.5.0	true
blueocean-github-pipeline	1.5.0	true
blueocean-i18n	1.5.0	true
blueocean-jira	1.5.0	true
blueocean-jwt	1.5.0	true
blueocean-personalization	1.5.0	true
blueocean-pipeline-api-impl	1.5.0	true
blueocean-pipeline-editor	1.5.0	true
blueocean-pipeline-scm-api	1.5.0	true
blueocean-rest	1.5.0	true
blueocean-rest-impl	1.5.0	true
blueocean-web	1.5.0	true
bouncycastle-api	2.16.2	true
branch-api	2.0.19	true
build-alias-setter	0.4	true
build-history-metrics-plugin	1.2	true
build-monitor-plugin	1.12+build.201708172343	true
build-name-setter	1.6.9	true
build-timeout	1.19	true
build-token-root	1.4	true
build-user-vars-plugin	1.5	true
build-with-parameters	1.4	true
clone-workspace-scm	0.6	true
cloudbees-bitbucket-branch-source	2.2.10	true
cloudbees-folder	6.4	true
cobertura	1.12	true
command-launcher	1.2	true
conditional-buildstep	1.3.6	true
config-file-provider	2.18	true
configurationslicing	1.47	true
copy-project-link	1.5	true
copyartifact	1.39.1	true
credentials	2.1.16	true
credentials-binding	1.16	true
dashboard-view	2.9.11	true
date-parameter	0.0.3	true
depgraph-view	0.13	true
discard-old-build	1.05	true
disk-usage	0.28	true
display-url-api	2.2.0	true
docker-commons	1.11	true
docker-workflow	1.15.1	true
downstream-buildview	1.9	true
downstream-ext	1.8	true
durable-task	1.22	true
dynamic_extended_choice_parameter	1.0.1	true
email-ext	2.62	true
envfile	1.2	false
envinject	2.1.5	true
envinject-api	1.5	true
extended-choice-parameter	0.76	true
extended-read-permission	2.0	true
extensible-choice-parameter	1.5.0	true
external-monitor-job	1.7	true
favorite	2.3.1	true
filesystem-list-parameter-plugin	0.0.4	true
findbugs	4.72	true
git	3.8.0	true
git-changelog	1.57	true
git-client	2.7.1	true
git-parameter	0.9.2	true
git-server	1.7	true
git-tag-message	1.6.1	true
github	1.29.0	true
github-api	1.90	true
github-branch-source	2.3.3	true
github-organization-folder	1.6	true
gitlab-hook	1.4.2	true
gitlab-logo	1.0.3	true
gitlab-merge-request-jenkins	2.0.0	true
gitlab-oauth	1.3	true
gitlab-plugin	1.5.5	true
global-build-stats	1.5	true
gradle	1.28	true
handlebars	1.1.1	true
handy-uri-templates-2-api	2.1.6-1.0	true
hidden-parameter	0.0.4	true
htmlpublisher	1.16	true
icon-shim	2.0.3	true
ivy	1.28	true
jackson2-api	2.8.11.1	true
javadoc	1.4	true
jenkins-design-language	1.5.0	true
jira	2.5	true
job-parameter-summary	0.5	true
jobConfigHistory	2.18	true
jquery	1.12.4-0	true
jquery-detached	1.2.1	true
jquery-ui	1.0.2	true
jsch	0.1.54.2	true
junit	1.24	true
ldap	1.20	true
locale	1.2	true
m2release	0.14.0	true
mailer	1.21	true
mapdb-api	1.0.9.0	true
mask-passwords	2.11.0	true
matrix-auth	2.2	true
matrix-project	1.13	true
maven-plugin	3.1.2	true
memory-map	2.2.1 (53614)	true
mercurial	2.3	true
metrics	3.1.2.11	true
metrics-diskusage	3.0.0	true
metrics-graphite	3.0.0	true
modernstatus	1.2	true
momentjs	1.1.1	true
monitoring	1.72.0	true
multi-branch-project-plugin	0.7	true
nexus-artifact-uploader	2.10	true
nodejs	1.2.6	true
pam-auth	1.3	true
parameter-pool	1.0.3	true
parameter-separator	1.0	true
parameterized-scheduler	0.5	true
parameterized-trigger	2.35.2	true
persistent-parameter	1.1	true
pipeline-build-step	2.7	true
pipeline-github-lib	1.0	true
pipeline-graph-analysis	1.6	true
pipeline-input-step	2.8	true
pipeline-milestone-step	1.3.1	true
pipeline-model-api	1.2.8	true
pipeline-model-declarative-agent	1.1.1	true
pipeline-model-definition	1.2.8	true
pipeline-model-extensions	1.2.8	true
pipeline-npm	0.9.2	true
pipeline-rest-api	2.10	true
pipeline-stage-step	2.3	true
pipeline-stage-tags-metadata	1.2.8	true
pipeline-stage-view	2.10	true
plain-credentials	1.4	true
port-allocator	1.8	true
project-stats-plugin	0.4	true
publish-over	0.22	true
publish-over-ssh	1.19.1	true
pubsub-light	1.12	true
pyenv	0.0.7	true
pyenv-pipeline	1.0.4	true
python	1.3	true
random-string-parameter	1.0	true
readonly-parameters	1.0.0	true
rebuild	1.28	true
resource-disposer	0.8	true
role-strategy	2.7.0	true
ruby-runtime	0.12	true
run-condition	1.0	true
saferestart	0.3	true
scm-api	2.2.6	true
script-security	1.43	true
show-build-parameters	1.0	true
simple-parameterized-builds-report	1.5	true
simple-theme-plugin	0.4	true
sse-gateway	1.15	true
ssh-credentials	1.13	true
ssh-slaves	1.26	true
StashBranchParameter	0.3.0	true
statusmonitor	1.3	true
structs	1.14	true
subversion	2.10.5	true
test-results-analyzer	0.3.4	true
thinBackup	1.9	true
timestamper	1.8.9	true
token-macro	2.5	true
toolenv	1.1	false
validating-string-parameter	2.3	true
variant	1.1	true
view-job-filters	1.27	true
violation-comments-to-gitlab	1.28	true
windows-slaves	1.3.1	true
workflow-aggregator	2.5	true
workflow-api	2.27	true
workflow-basic-steps	2.6	true
workflow-cps	2.48	true
workflow-cps-global-lib	2.9	true
workflow-durable-task-step	2.19	true
workflow-job	2.19	true
workflow-multibranch	2.17	true
workflow-scm-step	2.6	true
workflow-step-api	2.14	true
workflow-support	2.18	true
ws-cleanup
```



