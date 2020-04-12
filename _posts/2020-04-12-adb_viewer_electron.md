---
tags: ["electron","project"]
title: '[electron] adb viewer'
date: 2020.04.12
category: 'electron'
---





### electron을 사용하여 adb viewer PC 프로그램 제작하기

> [소스코드](<https://github.com/dgjung0220/adb_viewer>)

<img src="/images/image-20200412212234006.png" alt="image-20200412214029758" style="zoom:50%;" />

 LG 전자에서 일할 때, VR 성능 튜닝 등의 작업을 할 때 만들었다.  간단히 현재 adb 를 통해 연결된 안드로이드 모바일 디바이스의 CPU 사용률, Frequency를 볼 수 있고, Hetrogeneous CPU 환경에서 클러스터별 동작 frequency, foreground, background 등 셋팅, 현재 각 group에서 동작하고 있는 프로세스가 어떤 건지 확인하기 위해 제작되었다. 

 그닥 어려운 작업은 아니었지만, 일일히 adb에서 명령어로 foreground, background, top-app을 후킹하면서 보는 게 짜증났었는데, electron 으로 PC 프로그램을 간단히 개발해서 튜닝 작업하면서 많이 사용했다.



#### 간단한 메모

##### Show each core's CPU Usage & Frequency
```
cat /proc/stat
cat /sys/devices/system/cpu/cpuX/cpufreq/scaling_cur_freq
```
##### Show CPUSETs Setting Configuration
```
cat /dev/cpuset/cpus
cat /dev/cpuset/foreground/cpus
cat /dev/cpuset/background/cpus
cat /dev/cpuset/system-background/cpus
cat /dev/cpuset/top-app/cpus
```
##### Show running process by CPUSETs
```
cat /dev/cpuset/foreground/tasks
cat /dev/cpuset/background/tasks
cat /dev/cpuset/system-background/tasks
cat /dev/cpuset/top-app/tasks
```

##### 실행 파일 만들기 및 압축

package.json 에 poststart 명령어 추가

```javascript
"scripts": {    
    "start": "electron .",
    "poststart" : "electron-packager . cpuViewer --asar --platform win32 --arch x64 --out dist/"
},
```

##### installer 파일 만들기 (setup.exe)

```javascript
# installer.js
var createInstaller = require('electron-installer-squirrel-windows');

createInstaller({
    name : 'cpuViewer',
    path: './dist/cpuViewer-win32-x64',
    out: './dist/installer',
    authors: 'Donggoo Jung',
    exe: 'cpuViewer.exe',
    appDirectory: './dist/cpuViewer-win32-x64',
    overwrite: true,
    setup_icon: 'favicon.ico'
}, function done (e) {
    console.log('Build success !!');
});
```

```bash
$ node installer.js
```

