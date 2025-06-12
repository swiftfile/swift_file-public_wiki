## 環境
- ubuntu 22.04
- Thinkpad T14s(AMD)
- google-chromeのversion:多分最新版

## 事象
- 上の環境でchromeのパスワード補完を使おうとしても使えない
- 新しくパスワードを入力して、パスワード保存をしようとしても、保存されない
- パスワードマネージャを見に行っても一つも表示されない

## 解決方法

```
sudo apt purge google-chrome-stable
sudo chown -Rc $USER:$USER $HOME
sudo rm -rf ~/.config/google-chrome
sudo rm -rf ~/.cache
```

## 参考
[login - Google Chrome does not sync passwords - Ask Ubuntu](https://askubuntu.com/questions/1151243/google-chrome-does-not-sync-passwords)