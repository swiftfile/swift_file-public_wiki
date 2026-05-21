### 環境
- ubuntu 22.04
-  thinkpad p1g6
	- intel CPU
	- nvidia RTX3500 Ada

### 手順
- `sudo snap install cloudcompare`
- `sudo apt install mesa-utils`
- `glxinfo | grep "OpenGL renderer"
	- で値が出ることを確認する．
	- 例：`OpenGL renderer string: Mesa Intel(R) Graphics (RPL-P)
- `cloudcompare.ccViewer` で起動