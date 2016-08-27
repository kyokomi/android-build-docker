docker-android-build
==========================

Androidのbuildとemulatorの実行を行えるDockerfileです。

## Werckerで使う場合

```yml:wercker.yml
build:
  box:
    id: <DockerHub等のID>
    username: $DOCKER_USERNAME
    password: $DOCKER_PASSWORD
    tag: latest
  steps:
    - script:
        name: "show base information"
        code: |
          ./gradlew -v
          echo $ANDROID_HOME
          echo $ANDROID_SDK_VERSION
          echo $ANDROID_BUILD_TOOLS
          echo $ANDROID_UPDATE_FILTER
    - script:
      name: start emurator
      code: |
        start-emulator ./gradlew connectedAndroidTest
    - script:
      name: "test debug"
      code: |
        ./gradlew --full-stacktrace -q --project-cache-dir=$WERCKER_CACHE_DIR testDebug
    - script:
      name: "test debug spoon"
      code: |
        ./gradlew --full-stacktrace -q --project-cache-dir=$WERCKER_CACHE_DIR spoonDebugAndroidTest
```


## 参考

- [gfx/docker-android-project - GitHub.com](https://github.com/gfx/docker-android-project)
- [ksoichiro/dockerfiles/android-emulator/Dockerfile - GitHub.com](https://github.com/ksoichiro/dockerfiles/blob/master/android-emulator/Dockerfile)
