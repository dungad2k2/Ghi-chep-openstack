# Cách build từ source project watcher trong openstack 
Khi deploy bằng kolla-ansible ta sử dụng container để chứa những service của Openstack. Do đó để build được một service từ source của nó. Ta sẽ phải build một image riêng từ source sử dụng kolla-build. Sau đây sẽ là ví dụ để build image project watcher: 
1. Cài đặt kolla-build và docker trong môi trường ảo của python engine: 
  ```
python3 -m pip install kolla
python3 -m pip install docker
  ```
2. Tạo một file `kolla-build.conf` để thực hiện config build từ local, git, tar.gz,...\\
Một file kolla-build.conf thông thường sẽ được trông như này:
  ```
[horizon]
type = url
location = https://tarballs.openstack.org/horizon/horizon-master.tar.gz

[keystone-base]
type = git
location = https://opendev.org/openstack/keystone
reference = stable/mitaka

[heat-base]
type = local
location = /home/kolla/src/heat

[ironic-base]
type = local
location = /tmp/ironic.tar.gz
enabled = False
   ```
Ở đây ta thực hiện build image của watcher. Do đó ta sẽ có một file config như sau:
   ```
[watcher]
type = local
location = /home/buidung/....
   ```
3. Thực hiện câu lệnh kolla-build để build lại image:
   ```
     kolla-build --config-file /home/buidung/kolla-build.conf -b ubuntu watcher
   ```
Khi đó image sẽ được build từ source của mình đã được git clone. 
