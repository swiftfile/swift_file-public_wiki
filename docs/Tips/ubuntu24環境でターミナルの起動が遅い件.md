## 環境
- thinkpad X1G6
- ubuntu 24 LTS
- kernel 6.14x
- CPU:inteli7-13800H 
- GPU: nvidia RTX3500 Ada gen
- nvidia-driver:

## 症状
- `ctrl + Alt + t`を押してターミナルが起動するまでに時間がかかる.

## 解決法
- `sudo apt remove xdg-desktop-portal-gnome`

