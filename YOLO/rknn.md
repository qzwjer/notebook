### compiler路径
`export TOOL_CHAIN=<交叉编译工具链的路径>`

---

### 编译rknn_yolov5_demo报错
执行 build-linux_RK3566_3568.sh 时，报错：
```
..../librockchip_mpp.so: file format not recognized; 
treating as linker script
```
librockchip_mpp.so file format 不识别，`file librockchip_mpp.so`后发现是ASCII text格式，而正确应该是`librockchip_mpp.so: symbolic link to librockchip_mpp.so.1`。
因为是先在windows下拉取的rknpu2仓库，然后copy到wsl2的ubuntu下的，动态链接库.so文件链接失效。
重新直接从wsl2拉取库即可。

---

### wsl2 连接usb

wsl2当前版本已经具备了连接usb的能力，不用在编译内核了

1. windows安装[usbipd-win](https://github.com/dorssel/usbipd-win/releases/tag/v2.4.1)
2. powershell 中运行`usbipd wsl list`查看usb列表
3. 运行`usbipd wsl attach --busid <BUSID>`连接usb到wsl
4. 在wsl中`lsusb`查看