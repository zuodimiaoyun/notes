#### CI Environment Variables
|参数名|示例|说明|
|----|----|----|
|CI|true| 是否在CI环境执行
|CI_API_V4_URL||GitLab API V4 Url
|CI_BUILDS_DIR|/builds| 执行构建的顶级目录
|CI_COMMIT_BEFORE_SHA|fae5599e8d5c32f8bbe04daf726358e7d2526099| 合并请求前，分支上存在的最新提交，仅在合并请求与pipeline关联时填充
|CI_COMMIT_BRANCH|master|提交的分支名称，仅当在分支上构建时提供
|CI_COMMIT_TAG|v1.0.0|提交的tag名，仅当在tag上构建时提供
|CI_COMMIT_DESCRIPTION||提交描述
|CI_COMMIT_MESSAGE|Update .gitlab-ci.yml|完整的提交信息
|CI_COMMIT_REF_NAME|master|构建项目的分支或tag名称
|CI_COMMIT_REF_PROTECTED|true|是否在受保护的分支上运行
|CI_COMMIT_REF_SLUG|master|CI_COMMIT_REF_NAME的小写，缩短为63字节，并替换所有除了0-9，a-z的字符为-,并去掉前导/尾随 "-"，在url或域名中使用
|CI_COMMIT_SHA|b8d7bba5fbefa1f7098846097c2e4b2bbc22b5b0|构建项目提交的修订版本号
|CI_COMMIT_SHORT_SHA|b8d7bba5|CI_COMMIT_SHA的前8个字符
|CI_COMMIT_TITLE|Update .gitlab-ci.yml|提交的标题 - 完整的第一行信息
|CI_CONCURRENT_ID|0|单个执行器中构建的唯一ID
|CI_CONCURRENT_PROJECT_ID|0|单个执行器和项目中构建的唯一ID
|CI_CONFIG_PATH|.gitlab-ci.yml|CI配置文件的路径
|CI_DEFAULT_BRANCH|master|项目默认分支
|CI_DEPLOY_PASSWORD||Git Deploy Token 的密码
|CI_DEPLOY_USER||Git Deploy Token 的用户
|CI_DISPOSABLE_ENVIRONMENT|true|标记job是否运行在一次性环境
|CI_ENVIRONMENT_NAME||环境名，仅在 environment:name被设置时提供
|CI_ENVIRONMENT_SLUG||CI_ENVIRONMENT_NAME的简单版本
|CI_JOB_ID|414|GitLab CI 唯一job ID
|CI_JOB_NAME|test-docker|job 名称
|CI_JOB_STAGE|test|job 的构建阶段
|CI_JOB_TOKEN|\[MASKED\]|用于通过GitLab容器注册表进行身份验证和下载依赖存储库的令牌
|CI_JOB_URL| http://10.129.18.24/devops/helloworld-java/-/jobs/414 |job 详情地址
|CI_NODE_TOTAL|1|并行运行这个job的实例个数
|CI_PAGES_DOMAIN|example.com|托管GitLab Pages的域名
|CI_PAGES_URL|http://devops.example.com/helloworld-java | GitLab Pages 构建页面的Url
|CI_PIPELINE_ID|168| GitLab CI pipeline  唯一ID
|CI_PIPELINE_IID|24| 项目pipeline唯一ID
|CI_PIPELINE_SOURCE|push| pipeline触发源
|CI_PIPELINE_URL|http://10.129.18.24/devops/helloworld-java/pipelines/168 | pipeline详情地址
|CI_PROJECT_DIR|/builds/devops/helloworld-java| job运行路径
|CI_PROJECT_ID|2| 项目唯一ID
|CI_PROJECT_NAME|helloworld-java|项目名称
|CI_PROJECT_NAMESPACE|devops|项目组
|CI_PROJECT_PATH|devops/helloworld-java|项目命名空间
|CI_PROJECT_PATH_SLUG|devops-helloworld-java|CI_PROJECT_PATH的简单版本
|CI_PROJECT_REPOSITORY_LANGUAGES|c#,java,dockerfile|小写的项目语言
|CI_PROJECT_TITLE|helloworld|项目标题（human-readable）
|CI_PROJECT_URL|http://10.129.18.24/devops/helloworld-java |项目地址
|CI_PROJECT_VISIBILITY|public|项目可见性
|CI_REGISTRY|devhub.beisencorp.com|容器仓库地址
|CI_REGISTRY_PASSWORD|DevOps@123|容器仓库密码
|CI_REGISTRY_USER|devops|容器仓库用户
|CI_REPOSITORY_URL|http://gitlab-ci-token:[MASKED]@10.129.18.24/devops/helloworld-java.git |克隆git仓库的地址
|CI_RUNNER_DESCRIPTION|docker|runner的描述
|CI_RUNNER_EXECUTABLE_ARCH|darwin/amd64|runner执行的系统架构
|CI_RUNNER_ID|3|runner的唯一ID
|CI_RUNNER_REVISION|ac8e767a|runner运行当前job的版本
|CI_RUNNER_SHORT_TOKEN|o7zD1axQ|runnder的token，用于唯一ID
|CI_RUNNER_TAGS|docker|runner的标签
|CI_RUNNER_VERSION|12.6.0|runner的版本
|CI_SERVER|yes|标记是否在CI环境下执行
|CI_SERVER_HOST|10.129.18.24|CI服务器地址
|CI_SERVER_NAME|GitLab|CI服务器名称
|CI_SERVER_REVISION|70900054dfe|CI用于计划job的修订版
|CI_SERVER_VERSION|12.6.4|CI服务器版本
|CI_SERVER_VERSION_MAJOR|12|CI服务器主要版本
|CI_SERVER_VERSION_MINOR|6|CI服务器次要版本
|CI_SERVER_VERSION_PATCH|4|CI服务器修订版本