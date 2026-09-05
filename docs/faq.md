# 常见问题FAQ

## Q：刷完无法访问192.168.1.1
A：
1. 确认电脑网卡自动获取IP；
2. 清理浏览器缓存，换浏览器；
3. 硬改检查SPI闪存焊接，有无虚焊；
4. Breed重刷固件，不要保留配置   

## Q：网口颠倒、WAN/LAN反了
A：设备树网口定义问题，需要修改 `02_network` 脚本重新编译固件。

## Q：WiFi不工作
A：检查硬改焊接，确认固件mt76驱动是否完整。

## Q：编译Action报错403 Release发布失败
A：
1. PAT必须是Tokens(classic)，勾选完整repo权限；
2. 仓库设置开启 Read and write permissions；
3. Secrets名字与yml完全一致。

## Q：变砖救回
A：使用CH347F/T48编程器重新烧录SPI闪存Breed。
