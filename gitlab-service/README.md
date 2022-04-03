<!--
 * @Author: Jimmy.chen
 * @Date: 2021-12-12 10:43:47
 * @LastEditTime: 2022-04-03 16:38:44
 * @LastEditors: Jimmy.chen
 * @Description: 
-->

### 建立一個測試用gitlab
[參考連結](https://github.com/sameersbn/docker-gitlab)

#### postgress
docker run --name gitlab-postgresql -d \
    --env 'DB_NAME=gitlabhq_production' \
    --env 'DB_USER=gitlab' --env 'DB_PASS=password' \
    --env 'DB_EXTENSION=pg_trgm,btree_gist' \
    --volume /srv/docker/gitlab/postgresql:/var/lib/postgresql \
    sameersbn/postgresql:12-20200524

#### redis
docker run --name gitlab-redis -d \
    --volume /srv/docker/gitlab/redis:/data \
    redis:6.2

#### gitlab 
docker run --name gitlab -d \
    --link gitlab-postgresql:postgresql \
    --link gitlab-redis:redisio \
    --publish 10022:22 --publish 80:80 \
    --env 'GITLAB_HOST=gitlab.mikemomo.com' \
    --env 'GITLAB_PORT=80' --env 'GITLAB_SSH_PORT=22' \
    --volume /srv/docker/gitlab/gitlab:/home/git/data \
    --env 'GITLAB_SECRETS_DB_KEY_BASE=long-and-random-alpha-numeric-string' \
    --env 'GITLAB_SECRETS_SECRET_KEY_BASE=long-and-random-alpha-numeric-string' \
    --env 'GITLAB_SECRETS_OTP_KEY_BASE=long-and-random-alpha-numeric-string' \
    sameersbn/gitlab:14.9.2


### 移除docker container
 docker stop gitlab gitlab-postgresql && docker rm gitlab-postgresql gitlab