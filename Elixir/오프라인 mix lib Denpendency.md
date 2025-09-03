### 1. mix.exs에 프로젝트에 필요한 연관 파일 가지고 있을 것

github에서 관리하고 있음 [JJH82/itsm-lib](https://github.com/JJH82/itsm-lib)

### 2. .hex( %USERPROFILE%\.hex ) lib 추가될 때 꼭 
%USERPROFILE%\.hex 폴더
```
- packages  # denpendency tar파일 
- cache.ets # index파일
  
  
# denpendency tar파일  + index 파일
```


### 3. cache.ets 파일 안 연관 파일 명이 없을 경우 오류
```
(MachError) no match of right hand side value: {:error, "Hex is running in offline mode and the registry entry for package lib명 is not cached locally}

```