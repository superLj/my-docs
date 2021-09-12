#基于gitlab&jenkins的自动化部署实践

> jenkins的配置
1. jenkins -> 系统配置

jenkins location, jenkins url: 自己的服务ip地址
gitlab
  gitlab host url: https://gitlab.com/
  Credentials: gitlab Api token / api token

2. 自己工程的配置页:

源码管理: 
  repository url: 项目的git地址
  Credentials: Gitlab API token -> API token
  指定分支

构建触发器:
  Build when a change is pushed to GitLab. GitLab webhook URL: http://121.41.104.96/project/lanui

  Secret token

构建环境:
  Provide Node & npm bin/ folder to PAT

构建
```
执行shell

cd /root/.jenkins/workspace/lanui // 服务器jenkins工作目录
npm install
npm run build
cd dist
rm -rf lanui.tar.gz
tar -zcvf lanui.tar.gz
cp lanui.tar.gz /usr/local/nginx
cd /usr/local/nginx
tar -xzvf lanui.tar.gz
```
3. 插件管理
  NodeJS Plugin
  Gitlab Hook Plugin
  Gitlab Hook Plugin
  Generic Webhook Trigger

> gitlab 的配置

1. settings -> intergrations 
  Active interations, jenkins

  Jenkins server URL: jenkins 构建触发器的GitLab webhook URL

2. settings -> webhooks
   URL: jenkins 构建触发器的GitLab webhook URL
   Secret token

3. user settings
    Access Tokens
      Select scopes 只勾选api