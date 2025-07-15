### 1.경로
```
# 프로젝트 폴더에 _build
\_build\esbuild-linux-x64
\_build\tailwind-linux-x64
```

### 2. 다운로드 스크립트
```
curl -fsSL https://github.com/tailwindlabs/tailwindcss/releases/download/v3.4.6/tailwindcss-linux-x64 -o tailwindcss-linux-x64

curl -fsSL https://registry.npmjs.org/@esbuild/linux-x64/-/linux-x64-0.23.0.tgz -o linux-x64-0.23.0.tgz

tar -xzf linux-x64-0.23.0.tgz
mv /app/package/bin/esbuild /app/esbuild-linux-x64
rm -f *.tgz
m -Rf package
chmod 755 *-x64
```