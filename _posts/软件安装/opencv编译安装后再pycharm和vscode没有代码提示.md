### 解决在pycharm中无代码提示
opencv编译完成安装后在`/usr/local/opencv3.4/lib/python3.6/dist-packages/cv2/python-3.6`目录下会有`cv2.cpython-36m-x86_64-linux-gnu.so`库文件。

将其复制到python的包文件夹中，这里使用python虚拟环境的目录`/PATH/PYTHON/VENV/venv3.6/lib/python3.6/site-packages`路径，
在此路径下创建`cv2`文件夹
```sh
cd /PATH/PYTHON/VENV/venv3.6/lib/python3.6/site-packages
mkdir cv2
cp /usr/local/opencv3.4/lib/python3.6/dist-packages/cv2/python-3.6/cv2.cpython-36m-x86_64-linux-gnu.so cv2
cd cv2
chmod a+r cv2.cpython-36m-x86_64-linux-gnu.so
```
然后创建`__init__.py`文件，并输入一下内容
```python
import sys
import os
import importlib

# FFmpeg dll is not found on Windows without this
os.environ["PATH"] += os.pathsep + os.path.dirname(os.path.realpath(__file__))

# make IDE's (PyCharm) autocompletion happy
from .cv2 import *

# wildcard import above does not import "private" variables like __version__
# this makes them available
globals().update(importlib.import_module('cv2.cv2').__dict__)
```
重启pycharm

### 解决在vscode中无代码提示
在vscode中打开`setting`，搜索`pylintargs`，显示如下
![](../../assets/images/Screenshot%20from%202019-10-17%2015-53-57.png)   
在选项`Pylint Args`选项中`Add Item`， 输入`--generate-members`，结果如上图。
重启vscode