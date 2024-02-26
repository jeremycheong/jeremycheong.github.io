### pip 只下载不安装
```
pip3 download -d /save/path package-name
or
pip3 download -d /save/path -r requirements.txt
```

### 离线安装
```
pip3 install --no-index --find-links=/save/path package-name
```