### 環境
- ubuntu22.04

1. githubのmcapリポジトリのreleasesで、mcap-cliで最新versionをDLしてくる．
	[Releases · foxglove/mcap](https://github.com/foxglove/mcap/releases)
 2. `chmod +x ./mcap-linux-amd64`
 3. `sudo mv mcap-linux-amd64 /usr/bin/mcap`

### 参照
[20240925_ROSConJP2024](https://roscon.jp/2024/presentations/07.pdf)

## rosbag(1)to mcap変換コマンド
- mcapコマンドを使う
`mcap convert <input.bag> <output.mcap>`
- ros2 bag コマンドでは再生できない．
```
❯ ros2 bag play section.mcap 

closing.
[INFO] [1749898063.804701805] [rosbag2_cpp]: No plugin found providing serialization format 'ros1'. Falling back to checking RMW implementations.
[ERROR] [1749898063.815250470] [rosbag2_cpp]: Could not initialize RMWImplementedConverter: No RMW implementation found supporting serialization format ros1
[INFO] [1749898063.815288283] [rosbag2_cpp]: No plugin found providing serialization format 'cdr'. Falling back to checking RMW implementations.
Could not find converter for format ros1

```

