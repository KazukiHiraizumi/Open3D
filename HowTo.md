# Open3Dをビルドする

## 準備

1. PYBIND11
~~~
pip install pybind11 --user
~~~
または
~~~
sudo pip install pybind11
~~~

2. ipywidgets
~~~
pip install ipywidgets --user
~~~

3. Eigen3
~~~
sudo apt-get install libeigen3-dev
~~~

4. Open3Dソース
~~~
git clone https://github.com/IntelVCL/Open3D
~~~
checkout後
~~~
git submodule update --init --recursive
util/scripts/install-deps-ubuntu.sh
~~~

## Build

既にpipコマンドでインストールされているのであれば、アンインストしておく。
インストールされているかどうかは
~~~
pip uninstall open3d-python
~~~

1. libOpen3D.so
~~~
cd Open3D
mkdir build
cd build
cmake -DPYTHON_EXECUTABLE:FILEPATH=/usr/bin/python ..
make -j<thread numbers>
~~~
lib/libOpen3D.soが出来ればOK。インストする。
~~~
sudo make install
~~~

2. Pythonパッケージ
~~~
make python-package
~~~
lib/python_package/open3d/に以下のファイル構成パッケージファイルが出来ればOK。
~~~
static/
__init__.py
j_visualizer.py
open3d.so
~~~
PYTHONPATHにコピーする
~~~
cp -a lib/python_package/open3d ~/catkin_ws/devel/lib/python2.7/dist-packages/
~~~
以下でエラーなければインスト完了
~~~
python -c"import open3d"
~~~

3. PIPパッケージ  
pipで配布したいときは以下で.whlファイルも作れる
~~~
make pip-package
~~~
lib/python_package/pip_package/に.whlファイルが出来ればOK

4. インストール
~~~
pip install lib/python_package/pip_package/open3d-0.4.0.0-cp27-cp27mu-linux_x86_64.whl --user
~~~
以下でエラーなければOK
~~~
python -c"import open3d"
~~~
