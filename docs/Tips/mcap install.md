### 環境
- ubuntu22.04
- ros2 humble

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


https://github.com/tier4/bag_topic_renamer
## using rosbag-convert
そのままやると、topicに数字が入っているせい(topic名のみの問題)でmcapだと`ros2 bag run <input.bag>`をしたときに失敗する．

- そのまま変換したときのコマンド
	- `rosbags-convert --src section.bag --dst rosbags-converted-mcap.mcap --dst-storage mcap
- 対応策
	- 数字が入っているトピックは重要ではなかったので、必要なトピックをpubして、それをsubscribeした．
```
❯ rosbag info main.bag 
path:        main.bag
version:     2.0
duration:    20.0s
start:       Nov 08 2022 03:36:55.00 (1667846215.00)
end:         Nov 08 2022 03:37:14.00 (1667846235.00)
size:        743.7 MB
messages:    30040
compression: none [830/830 chunks]
types:       asv_perception_common/ObstacleArray [bd07b89cf615bfa531a2a9dace627e57]
             auvlab_msgs/AISContact              [bfede8139ccd5b6a0759dc5cd2153882]
             broadband_radar/RadarSegment        [d115b49d3471672de0df98c3d9837f90]
             diagnostic_msgs/DiagnosticArray     [60810da900de1dd6ddd437c3503511da]
             geometry_msgs/QuaternionStamped     [e57f1e547e0e1fd13504588ffc8334e2]
             geometry_msgs/TwistStamped          [98d34b0043a2093cf9d9345ab6eef12e]
             geometry_msgs/Vector3Stamped        [7b324c7325e683bf02a9b14b01090ec7]
             nmea_msgs/Sentence                  [9f221efc5f4b3bac7ce4af102b32308b]
             rosgraph_msgs/Log                   [acffd30cd6b6de30f120938c17c593fb]
             sensor_msgs/CameraInfo              [c9a58c1b0b154e0e6da7578cb991d214]
             sensor_msgs/CompressedImage         [8f7a12909da2c9d3332d540a0977563f]
             sensor_msgs/Image                   [060021388200f6f0f447d0fcd9c64743]
             sensor_msgs/Imu                     [6a62c6daae103f4ff57a132d6f95cec2]
             sensor_msgs/NavSatFix               [2d3a8cd499b9b4a0249fb98fd05cfa48]
             sensor_msgs/PointCloud2             [1158d486dd51d683ce2f1be655c3c181]
             sensor_msgs/TimeReference           [fded64a0265108ba86c3d38fb11c0c16]
             tf2_msgs/TFMessage                  [94810edda583a504dfda3829e70d7eec]
             visualization_msgs/Marker           [4048c9de2a16f4ae8e0538085ebf1b97]
topics:      /ais/contact                               45 msgs    : auvlab_msgs/AISContact             
             /ais/fix                                   45 msgs    : sensor_msgs/PointCloud2            
             /ais/received                             359 msgs    : nmea_msgs/Sentence                 
             /ais/text_marker                           45 msgs    : visualization_msgs/Marker          
             /broadband_radar/channel_0/pointcloud    1020 msgs    : sensor_msgs/PointCloud2            
             /broadband_radar/channel_0/segment       1020 msgs    : broadband_radar/RadarSegment       
             /camera0/tracking/classified/obstacles    274 msgs    : asv_perception_common/ObstacleArray
             /center_camera/camera_info                240 msgs    : sensor_msgs/CameraInfo             
             /center_camera/image_color/compressed     240 msgs    : sensor_msgs/CompressedImage        
             /diagnostics                               97 msgs    : diagnostic_msgs/DiagnosticArray    
             /filter/gnss                              649 msgs    : sensor_msgs/NavSatFix              
             /filter/positionlla                       649 msgs    : geometry_msgs/Vector3Stamped       
             /filter/quaternion                       2299 msgs    : geometry_msgs/QuaternionStamped    
             /filter/twist                             649 msgs    : geometry_msgs/TwistStamped         
             /filter/velocity                          649 msgs    : geometry_msgs/Vector3Stamped       
             /gnss                                      13 msgs    : sensor_msgs/NavSatFix              
             /imu/acceleration                         649 msgs    : geometry_msgs/Vector3Stamped       
             /imu/angular_velocity                     649 msgs    : geometry_msgs/Vector3Stamped       
             /imu/data                                2299 msgs    : sensor_msgs/Imu                    
             /imu/dq                                   649 msgs    : geometry_msgs/QuaternionStamped    
             /imu/dv                                   649 msgs    : geometry_msgs/Vector3Stamped       
             /imu/mag                                  649 msgs    : geometry_msgs/Vector3Stamped       
             /imu/time_ref                            2311 msgs    : sensor_msgs/TimeReference          
             /left_camera/camera_info                  240 msgs    : sensor_msgs/CameraInfo             
             /left_camera/image_color/compressed       240 msgs    : sensor_msgs/CompressedImage        
             /left_ir/camera_info                      600 msgs    : sensor_msgs/CameraInfo             
             /left_ir/rotated/image_rect               600 msgs    : sensor_msgs/Image                  
             /nmea_to_send                            1435 msgs    : nmea_msgs/Sentence                 
             /philos/sbg_heading                      1652 msgs    : geometry_msgs/Vector3Stamped       
             /radar0/tracking/obstacles                 47 msgs    : asv_perception_common/ObstacleArray
             /right_camera/camera_info                 239 msgs    : sensor_msgs/CameraInfo             
             /right_camera/image_color/compressed      239 msgs    : sensor_msgs/CompressedImage        
             /right_ir/camera_info                     601 msgs    : sensor_msgs/CameraInfo             
             /right_ir/rotated/image_rect              601 msgs    : sensor_msgs/Image                  
             /rosout_agg                               108 msgs    : rosgraph_msgs/Log                  
             /segmentation_ir/0/rgb                     86 msgs    : sensor_msgs/Image                  
             /segmentation_ir/1/rgb                     86 msgs    : sensor_msgs/Image                  
             /tf                                      3503 msgs    : tf2_msgs/TFMessage                 
             /tracking/fusion/obstacles                 41 msgs    : asv_perception_common/ObstacleArray
             /xsens_heading                            929 msgs    : geometry_msgs/Vector3Stamped       
             ais/raw                                   346 msgs    : nmea_msgs/Sentence                 
             imu/data                                 2299 msgs    : sensor_msgs/Imu
```
- その後、`rosbags-convert`コマンドでmcapに変換して再生した．
	- 右側がros1のrosbag,左側がros2のrosbagで欠落が発生している．

![](Screenshot%20from%202025-06-16%2010-36-36.png)