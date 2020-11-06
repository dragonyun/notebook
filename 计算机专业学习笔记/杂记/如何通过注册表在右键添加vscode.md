## 如何通过注册表在右键添加vscode

### 背景

---

希望在文件夹中通过右键直接打开vscode，会方便很多。



### 实现思路

---

通过修改注册列表修改鼠标右键菜单



### 实现过程

---

1. (win+r)，输入regedit.exe，打开注册表列表
2. 找到对应的注册表路径 `HKEY_CLASSES_ROOT\Directory\Background\shell`
3. 在shell文件夹右键【新建】=>【项】，输入名称。（这里写`vscode`）
4. 修改这里的`vscode`默认值，根据需要填。（这里写`Open With Code`）影响的是右键菜单里的名称。
5. 现在，可以在右键里面看到`Open With Code`这个选项了。
6. 在`vscode`下新建项**command**（***\*名称必须为command\****）
7. 修改默认值为  **D:\Microsoft VS Code\Code.exe" "%V"**，路径是你的启动路径。
8. 现在，可以用了。
9. 加图标，在vscode中，新建“**字符串值**”，输入"**Icon**"（**名称必须为Icon**），数据数值是**D:\Microsoft VS Code\Code.exe" "%V"**，依然是你的启动路径。
10. 大功告成。



### 收获

---

1. 注册表挺好用的，右键里面可以随意加启动项了。举一反三。
2. 一开始没搞定，是把shell路径看错了。粗心大意。
3. 还挺有趣的。





### 参考文档

- vscode如何添加鼠标右键打开文件夹:`https://blog.csdn.net/zzsan/article/details/79305952`