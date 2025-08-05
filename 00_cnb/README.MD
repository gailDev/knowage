# cnb 和 github相互同步问题
参考以下代码进行配置

```yml
$:
  vscode:
    - runner:
      stages:
        - name: sync to github
          image: tencentcom/git-sync
          settings:
            target_url: https://github.com/gailDev/knowage.git
            auth_type: https
            username: $GITHUB_USER
            password: $GITHUB_TOKEN
            branch: $CNB_BRANCH
```
注意：如果提交失败，尝试使用以下命令重试
```bash
docker run --rm \
  -e PLUGIN_TARGET_URL="https://github.com/gailDev/knowage.git" \
  -e PLUGIN_AUTH_TYPE="https" \
  -e PLUGIN_USERNAME="gailDev" \
  -e PLUGIN_PASSWORD="github_p" \
  -e PLUGIN_BRANCH="main" \
  -v $(pwd):$(pwd) \
  -w $(pwd) \
  tencentcom/git-sync
```
1. username 必须为gailDev，这是git地址路径中的用户名
2. 创建后推测至少需要授权commit和push两个权限，否则会推送失败
![git token config](<img/Pasted Graphic.png>)

## 可以设置push时自动推送
```yml
# .cnb.yml
$:
  push:
    - services:
        - docker
      imports: 
        - https://cnb.cool/keys.yml
      stages:
        - name: sync to github
          image: tencentcom/git-sync
          settings:
            target_url: https://github.com/gailDev/knowage.git
            auth_type: https
            username: $GITHUB_USER
            password: $GITHUB_TOKEN
            branch: $CNB_BRANCH
```
