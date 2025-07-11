
### 1. lib 다운로드 되는 시점
```
# mix.exs 파일에 설정되어 있는 dependency lib 다운받기
mix deps.get

# 피닉스 프레임워크 다운받을시
mix archive.install hex phx_new 1.8.0-rc.3
```
### 2. 폴더 구조
```
# %USERPROFILE% = C:\Users\drcto 동일

%USERPROFILE%\.hex
%USERPROFILE%\.mix
```

### 3. offline 설정
```
mix hex.config offline true
```