<!--
 * @Author: Jimmy.chen
 * @Date: 2022-04-03 16:19:17
 * @LastEditTime: 2022-04-03 16:34:32
 * @LastEditors: Jimmy.chen
 * @Description: 
-->

### 下載執行檔
sudo curl -L --output /usr/local/bin/gitlab-runner https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64
sudo chmod +x /usr/local/bin/gitlab-runner

### 新建user
useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash

### 編寫systemd file
vi /etc/systemd/system/gitlab-runner.service

[Unit]
Description=GitLab Runner
ConditionFileIsExecutable=/usr/local/bin/gitlab-runner
 
After=syslog.target network.target 

[Service]
StartLimitInterval=5
StartLimitBurst=10
ExecStart=/usr/local/bin/gitlab-runner "run" "--working-directory" "/home/gitlab-runner" "--config" "/etc/gitlab-runner/config.toml" "--service" "gitlab-runner" "--user" "gitlab-runner"

Restart=always

RestartSec=120
EnvironmentFile=-/etc/sysconfig/gitlab-runner

[Install]
WantedBy=multi-user.target


#### 儲存離開

cp config.toml /etc/gitlab-runner/config.toml

### 完成安裝

gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner

### 完成runner 註冊
sudo gitlab-runner register --url http://gitlab.mikemomo.com/ --registration-token $REGISTRATION_TOKEN











