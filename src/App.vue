<template>
  <h1>
    我是Drone自动构建后上传到又拍云的页面
  </h1>
  <pre>
kind: pipeline
type: docker
name: default

clone:
  depth: 1

steps:
- name: release
  image: registry.cn-beijing.aliyuncs.com/wa/node
  when:
    event: push
    branch: master
  environment:
    UPX_USER:
      from_secret: upx_user
    UPX_PASSWD:
      from_secret: upx_passwd
  commands:
    - echo "branch is $DRONE_BRANCH, commit id is $DRONE_COMMIT"
    - npm install
    - npm run build
    - upx login raul-web-test $UPX_USER $UPX_PASSWD
    - upx sync ./dist/ /
  </pre>
  <h3>git version: {{gitVersion}}</h3>
</template>

<script>

export default {
  name: 'App',
  data () {
    return {
      gitVersion: process.env.VUE_APP_GIT_VERSION,
    }
  },
}
</script>

<style>
#app {
  width: 800px;
  margin: 0 auto;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
h1{
  text-align: center;
}
pre{
  padding: 20px;
  border: 1px solid #2c3e50;
}
h3{
  text-align: right;
}
</style>
