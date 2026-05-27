# Phần I - Linux Cơ bản

## Cài hệ điều hành cho Linux Server

1. Chuẩn bị trước khi cài `Ubuntu 24.04 LTS`

- Link tải: `https://ubuntu.com/download/server`

- File thường có tên: `ubuntu-24.04-live-server-amd64.iso`

2. Tạo USB Boot Ubuntu ở máy tính cá nhân

- `Bước 1`: Cắm USB vào máy.

- `Bước 2`: Mở phần mềm Rufus.

- `Bước 3`:

```bash
Device: USB
Boot selection: ubuntu-24.04-live-server-amd64.iso
Partition scheme: GPT
```

- `Bước 4`: Nhấn `Start` để tạo USB boot.

3. Boot server từ `USB`

- Khởi động server và vào Boot Menu:

- Hãng máy Phím
  `Dell F12`
  `HP F9`
  `Asus F8`
  `Lenovo F12`

- Chọn Boot từ USB.

- Sau đó menu cài đặt `Ubuntu` sẽ xuất hiện.

4. Bắt đầu cài `Ubuntu Server`

- Chọn:

```bash
Install Ubuntu Server
```

- `Các bước cài đặt:`

* Chọn ngôn ngữ `English`

* Keyboard `English (US)`

* Network Ubuntu sẽ tự nhận IP DHCP (nếu không nhận DHCP thì phải config DHCP cho Server)

5. Cấu hình ổ đĩa:

- Có 2 lựa chọn:

```bash
Use entire disk
```

- Ubuntu sẽ tự tạo:

```bash
/
swap
boot
```

6. Tạo user đăng nhập

- Ví dụ:

```bash
Name: admin
Server name: ubuntu-server
Username: admin
Password: ********
```

7. Cài đặt SSH cho Server

- Tick chọn

```bash
Install OpenSSH Server
```

8. Cài thêm package (optional)

- Ubuntu sẽ hỏi cài thêm: `Docker`, `Web server`,`Nginx`

- Có thể `skip` và cài sau.

9. Hoàn tất cài đặt

- Sau khi xong:

```bash
Installation complete
```

- Chọn:

```bash
Reboot
```

10. Cập nhật hệ thống

```bash
sudo apt update
sudo apt upgrade -y
```

- Kiểm tra phiên bản Ubuntu:

```bash
lsb_release -a
```

## Fix những case treo máy Ubuntu

- Khi boot vào USB Ubuntu -> Gõ phím e -> Xuất hiện màn hình
- Thêm `nomodeset acpi=off` vào sau `---` là passing

```bash
linux /casper/vmlinuz --- nomodeset acpi=off
```

- Nhấn `Ctrl + X` để reboot máy

## Kiểm tra cấu hình Server

1. Kiểm tra CPU:

- Kiểm tra loại ổ (SSD hay HDD)

```bash
lsblk -o NAME,ROTA,SIZE,TYPE,MOUNTPOINT
```

```bash
lscpu
Architecture:             x86_64
  CPU op-mode(s):         32-bit, 64-bit
  Address sizes:          45 bits physical, 48 bits virtual
  Byte Order:             Little Endian
CPU(s):                   4
  On-line CPU(s) list:    0-3
Vendor ID:                GenuineIntel
  Model name:             Intel(R) Xeon(R) CPU E5-2696 v4 @ 2.20GHz
    CPU family:           6
    Model:                79
    Thread(s) per core:   1
    Core(s) per socket:   1
    Socket(s):            4
    Stepping:             1
    BogoMIPS:             4399.99
    Flags:                fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss syscall nx pdpe1gb rdtscp lm constant_tsc a
                          rch_perfmon nopl xtopology tsc_reliable nonstop_tsc cpuid tsc_known_freq pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt tsc
                          _deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm abm 3dnowprefetch pti ssbd ibrs ibpb stibp fsgsbase tsc_adjust bmi1 avx2 smep bmi2
                          invpcid rdseed adx smap xsaveopt arat md_clear flush_l1d arch_capabilities
Virtualization features:
  Hypervisor vendor:      VMware
  Virtualization type:    full
Caches (sum of all):
  L1d:                    128 KiB (4 instances)
  L1i:                    128 KiB (4 instances)
  L2:                     1 MiB (4 instances)
  L3:                     220 MiB (4 instances)
NUMA:
  NUMA node(s):           1
  NUMA node0 CPU(s):      0-3
Vulnerabilities:
  Gather data sampling:   Not affected
  Itlb multihit:          KVM: Mitigation: VMX unsupported
  L1tf:                   Mitigation; PTE Inversion
  Mds:                    Mitigation; Clear CPU buffers; SMT Host state unknown
  Meltdown:               Mitigation; PTI
  Mmio stale data:        Mitigation; Clear CPU buffers; SMT Host state unknown
  Reg file data sampling: Not affected
  Retbleed:               Mitigation; IBRS
  Spec rstack overflow:   Not affected
  Spec store bypass:      Mitigation; Speculative Store Bypass disabled via prctl
  Spectre v1:             Mitigation; usercopy/swapgs barriers and __user pointer sanitization
  Spectre v2:             Mitigation; IBRS; IBPB conditional; STIBP disabled; RSB filling; PBRSB-eIBRS Not affected; BHI SW loop, KVM SW loop
  Srbds:                  Not affected
  Tsx async abort:        Not affected
```

2. Kiểm tra Disk Space:

```bash
df -h
Filesystem                         Size  Used Avail Use% Mounted on
tmpfs                              1.6G  1.2M  1.6G   1% /run
/dev/mapper/ubuntu--vg-ubuntu--lv   97G  6.9G   86G   8% /
tmpfs                              7.9G     0  7.9G   0% /dev/shm
tmpfs                              5.0M     0  5.0M   0% /run/lock
/dev/sda2                          2.0G  185M  1.7G  11% /boot
tmpfs                              1.6G   12K  1.6G   1% /run/user/1000
```

- Tổng dung lượng: 97G

3. Kiểm tra Permission Account:

```bash
sudo -l
Matching Defaults entries for administrator on ttp-devops-monitoring:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty
User administrator may run the following commands on ttp-devops-monitoring:
    (ALL : ALL) ALL
```

- Toàn quyền với account administrator

4. Kiểm tra RAM

```bash
free -h
              total        used        free      shared  buff/cache   available
Mem:            15Gi       774Mi        14Gi       1.1Mi       336Mi        14Gi
Swap:          3.8Gi          0B       3.8Gi
```

5. Kiểm tra Opera System

```bash
hostnamectl
Static hostname: (unset)
Transient hostname: ttp-devops-monitoring
         Icon name: computer-vm
           Chassis: vm 🖴
        Machine ID: a9b83e6da2e040429ddb923d491fdd20
           Boot ID: cae5f836468342428ef7914471e7c03b
    Virtualization: vmware
  Operating System: Ubuntu 24.04.2 LTS
            Kernel: Linux 6.8.0-58-generic
      Architecture: x86-64
   Hardware Vendor: VMware, Inc.
    Hardware Model: VMware Virtual Platform
  Firmware Version: 6.00
     Firmware Date: Thu 2020-11-12
      Firmware Age: 5y 3month 3w 6d
```

6. Liệt kê các gói phần mềm đã cài trên hệ thống (Debian/Ubuntu) liên quan đến PHP, Nginx và MySQL/MariaDB.

```bash
dpkg -l | grep -E "php8.3|php-fpm|nginx|mariadb|mysql"
```

## Step Install Docker:

1. Cập nhật hệ thống:

```bash
sudo apt update && sudo apt upgrade -y
```

2. Cài các package cần thiết:

```bash
sudo apt install -y ca-certificates curl gnupg
```

- Tạo thư mục `keyring`:

```bash
sudo install -m 0755 -d /etc/apt/keyrings
```

3. Thêm GPG key của `Docker`:

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

- Cấp quyền:

```bash
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

4. Thêm Docker repository:

```bash
echo \
"deb [arch=$(dpkg --print-architecture) \
signed-by=/etc/apt/keyrings/docker.gpg] \
https://download.docker.com/linux/ubuntu \
$(. /etc/os-release && echo $VERSION_CODENAME) stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

- Update lại repo:

```bash
sudo apt update
```

5. Cài đặt `Docker Engine`:

```bash
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

6. Kiểm tra `Docker`:

```bash
sudo docker --version
```

- Test `container`:

```bash
sudo docker run hello-world
```

7. Cho phép chạy Docker không cần sudo (khuyên dùng):

```bash
sudo usermod -aG docker $USER
```

- Sau đó logout SSH và login lại.

- Test:

```bash
docker ps -a
```

8. Kiểm tra `Docker Compose`:

```bash
docker compose version
```

- Chạy test:

```bash
docker compose up
```

9. Bật Docker tự khởi động khi reboot:

```bash
sudo systemctl enable docker
```

- Kiểm tra:

```bash
sudo systemctl status docker
```

## Docker Syntax:

- Kiểm tra tất cả Docker Container

```bash
docker ps -a
```

- Xoá container

```bash
docker rm <ten_container_hoac_hash_name>
```

- Nếu container đang chạy và muốn xoá luôn:

```bash
docker rm -f <ten_container_hoac_hash_name>
```

- Kiểm tra image

```bash
docker images
```

- Xoá image:

```bash
docker rmi <image_name>:<image_tag>
docker rmi chatbot/python:v0
```

- Dọn sạch docker rác:

```bash
docker container prune -f
```

- Xoá image không dùng:

```bash
docker image prune -a -f
```

- Xoá cả system rác:

```bash
docker system prune -a -f
```

- Vào log docker vps

```bash
docker exec -it cims_server_container sh
```

- Kiểm tra log docker app

```bash
sudo docker logs -f <ten_docker_container>
```

- Chạy docker compose

```bash
sudo docker compose -f docker-compose.yml up --build -d
sudo docker compose -f docker-compose.yml build --no-cache
sudo docker compose -f docker-compose.yml up -d
```

- Kiểm tra log docker compose

```bash
sudo docker compose -f docker-compose.yml logs -f
```

## Cách kiểm tra tất cả cổng đang hoạt động:

```bash
sudo ss -tupnel
```

## Tương tác File với Server Linux

1. Đẩy file lên Server (cần tải scp ở máy tính cá nhân)

```bash
scp "C:\laragon\www\wordpress.7z" administrator@10.0.0.10:/home/administrator/
```

2. Di chuyển từ `/home/<username>/` sang `/var/www/html/`

```bash
sudo mv /home/administrator/wordpress.7z /var/www/html/
```

3. Cài đặt 7z trên `Server Linux`

```bash
sudo apt install -y p7zip-full
```

4. Giải nén file

```bash
sudo 7z x wordpress.7z
```

5. Phân quyền thư mục:

```
sudo chown -R www-data:www-data /var/www/html/wordpress
sudo chmod -R 755 /var/www/html/wordpress
```

6. Xoá file trên VPS

```bash
rm tenfile
```

7. Xoá toàn bộ dữ liệu

```bash
sudo rm -rf /
```

8. Kiểm tra tất cả file là thư mục

```bash
sudo ls -a
```

9. Truy xuất vào folder

```bash
cd ten_thu_muc
```

10. Ra khỏi thư mục

```bash
cd ..
```

11. Tạo mới thư mục

```bash
sudo mkdir ten_thu_muc
```

12. Tạo mới file

```bash
sudo touch ten_file
```

13. Xem file

```bash
sudo cat duong_dan_file
```

14. Sửa file

```bash
sudo nano duong_dan_file
```

# Phần II - Linux nâng cao

## Swap RAM ảo:

- Ví dụ tạo 2GB swap:

```bash
sudo fallocate -l 2G /swapfile
```

- Nếu lệnh trên lỗi thì dùng:

```bash
sudo dd if=/dev/zero of=/swapfile bs=1M count=2048
```

- Phân quyền cho swap file

```bash
sudo chmod 600 /swapfile
```

- Tạo vùng swap

```bash
sudo mkswap /swapfile
```

Kết quả:

```bash
Setting up swapspace version 1
```

- Kích hoạt swap

```bash
sudo swapon /swapfile
```

- Tự động bật swap khi reboot

```bash
sudo nano /etc/fstab
```

Thêm dòng cuối:

```bash
/swapfile none swap sw 0 0
```

Lưu lại.

- Tối ưu swap:

```bash
cat /proc/sys/vm/swappiness
```

Thường là: `60`

Đối với VPS nên giảm xuống 10–20:

```bash
sudo sysctl vm.swappiness=10
```

Để lưu vĩnh viễn:

```bash
sudo nano /etc/sysctl.conf
```

Thêm:

```bash
vm.swappiness=10
```

- Gợi ý swap RAM

```bash
1GB swap 1–2GB
2GB swap 2GB
4GB swap 2–4GB
8GB+ swap 2GB
```

## Tất tấn tật về NGINX Server

### 1. Cấu hình:

#### 1.1 Cập nhật và tải Nginx về Server

```bash
sudo apt-get update && sudo apt-get install nginx
```

#### 1.2 Cấu hình tường lửa (Firewall):

```bash
sudo ufw allow 'Nginx Full'
```

1. Mở các `port` cần thiết:

- `Port`: 22 (giao thức SSH),

```bash
sudo ufw allow ssh
```

2. Bật tường lửa:

```bash
sudo ufw enable
```

3. Yêu cầu tường lửa lên mỗi khi khởi động lại server

```bash
sudo systemctl enable ufw
```

#### 1.3 Các câu lệnh kiểm tra liên quan tới Nginx:

1. Kiểm tra trạng thái tường lửa

```bash
sudo ufw status
```

2. Liệt kê danh sách các dịch vụ trên Nginx

```bash
sudo ufw app list
```

3. Kiểm tra trạng thái của Nginx

```bash
systemctl status nginx
```

#### 1.4 Cấu hình Nginx làm Reverse Proxy

1. Di chuyển tới thư mục `/etc/nginx/sites-available`

2. Tại thư mục này tạo ra file

```bash
sudo touch abcd.com
```

3. Edit file này

```bash
sudo nano abcd.com
```

- Lúc này file được sửa sẽ có dạng như sau:

```nginx
server {
        listen 80;
        listen [::]:80;

        root /var/www/html;
        index index.html index.htm index.nginx-debian.html;

        server_name abcd.com www.abcd.com;

        location / {
                proxy_pass http://localhost:3000;
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection 'upgrade';
                proxy_set_header Host $host;
                proxy_cache_bypass $http_upgrade;
        }
}
```

- Sửa `abcd.com` thành domain của bạn nhé

- Sửa `http://localhost:3000` thành đúng port của bạn nhé

- Lưu và thoát bằng cách nhấn tổ hợp `Ctrl X` -> `Y` -> `Enter`

- Bây giờ chúng ta cần kích hoạt cái file vừa tạo thông qua thư mục `/sites-enabled`, chỉ cần chạy câu lệnh dưới đây

```bash
sudo ln -s /etc/nginx/sites-available/abcd.com /etc/nginx/sites-enabled/
```

- Để tránh vấn đề hash bucket memory trong tương lai khi chúng ta thêm app, chúng ta cần tinh chỉnh một dòng trong file `/etc/nginx/nginx.conf`

```bash
sudo nano /etc/nginx/nginx.conf
```

- Trong file đó, tìm cái dòng comment là `# server_names_hash_bucket_size 64`; và xóa cái ký tự # để mở comment cho nó.

- Lưu và đóng file.

- Tiếp theo `nginx -t` để kiểm tra lỗi cú pháp trong những file cấu hình đã được tạo hoặc edit:

```bash
sudo nginx -t
```

- Nếu không có vấn đề gì, restart Nginx để enable những thay đổi vừa rồi.

```bash
sudo systemctl restart nginx
```

### 2. Các vấn đề về Cerfiticate (SSL) port 443 (Trường hợp này mình dùng Snap):

- Có rất nhiều cách active chứng chỉ SSL cho phần mềm: `Snap repository`, `Local Cerfiticate` (dùng tên miền để xác thực giá trị Cerfiticate), `Zero SSL`, `Nginx Manager` (Tool cài SSL của Nginx), ...v.v..

- Ở tài liệu này mình giới thiệu cách phổ biến nhất mà Devops thường triển khai cho một dự án website với Snap Repository

#### Cài đặt và tiến hành triển khai Snap

1. Cài đặt

```bash
sudo apt update
sudo apt install snapd
```

2. Nếu trước đó đã cài Certbot, hãy gỡ nó đi (nếu VPS mới toanh hoặc không biết gì thì bỏ qua bước này)

```bash
sudo apt-get remove certbot
```

3. Cài Certbot

```bash
sudo snap install --classic certbot
```

4. Chuẩn bị cerbot command

```bash
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

5. Cài và lấy chứng chỉ

```bash
sudo certbot --nginx
```

- Bởi vì đây là lần đầu bạn chạy Cerbot trên server này, nó sẽ yêu cầu bạn enter địa chỉ email và yêu cầu bạn đồng ý điều khoản. Sau đó, Certbot sẽ giao tiếp với server của `Let's Encrypt` để verify domain của bạn. Nếu thành công, Certbot sẽ hỏi bạn muốn cấu hình `HTTPS` như thế nào.

```bash
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Enter email address (used for urgent renewal and security notices)
 (Enter 'c' to cancel): duthanhduoc@gmail.com

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Please read the Terms of Service at
https://letsencrypt.org/documents/LE-SA-v1.3-September-21-2022.pdf. You must
agree in order to register with the ACME server. Do you agree?
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: y

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Would you be willing, once your first certificate is successfully issued, to
share your email address with the Electronic Frontier Foundation, a founding
partner of the Let's Encrypt project and the non-profit organization that
develops Certbot? We'd like to send you email about our work encrypting the web,
EFF news, campaigns, and ways to support digital freedom.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: y
Account registered.

Which names would you like to activate HTTPS for?
We recommend selecting either all domains, or all domains in a VirtualHost/server block.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
1: edu.abcd.com
2: api.edu.abcd.com
3: www.api.edu.abcd.com
4: www.edu.abcd.com
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Select the appropriate numbers separated by commas and/or spaces, or leave input
blank to select all options shown (Enter 'c' to cancel):
Requesting a certificate for edu.abcd.com and 3 more domains

Successfully received certificate.
Certificate is saved at: /etc/letsencrypt/live/edu.abcd.com/fullchain.pem
Key is saved at:         /etc/letsencrypt/live/edu.abcd.com/privkey.pem
This certificate expires on 2024-03-17.
These files will be updated when the certificate renews.
Certbot has set up a scheduled task to automatically renew this certificate in the background.

Deploying certificate
Successfully deployed certificate for edu.abcd.com to /etc/nginx/sites-enabled/edu.abcd.com
Successfully deployed certificate for api.edu.abcd.com to /etc/nginx/sites-enabled/api.edu.abcd.com
Successfully deployed certificate for www.api.edu.abcd.com to /etc/nginx/sites-enabled/api.edu.abcd.com
Successfully deployed certificate for www.edu.abcd.com to /etc/nginx/sites-enabled/edu.abcd.com
Congratulations! You have successfully enabled HTTPS on https://edu.abcd.com, https://api.edu.abcd.com, https://www.api.edu.abcd.com, and https://www.edu.abcd.com
We were unable to subscribe you the EFF mailing list because your e-mail address appears to be invalid. You can try again later by visiting https://act.eff.org.

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
If you like Certbot, please consider supporting our work by:
 * Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
 * Donating to EFF:                    https://eff.org/donate-le
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

```

- Nếu in ra `Successfully` là thành công rồi.

- Để test tiến trình làm mới tự động, anh em có thể chạy câu lệnh dưới

```bash
sudo certbot renew --dry-run
```

- Nếu không thấy bất cứ lỗi gì tức là thành công.

### 3. CI/CD Pipeline

1. Cài đặt Gitlab runner

```bash
curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh | sudo bash
sudo apt install gitlab-runner -y
```

- Kiểm tra version Gitlab runner

```bash
gitlab-runner --version
```

- Kiểm tra trạng thái Gitlab runner

```bash
sudo systemctl status gitlab-runner
```

- Vì CI/CD của bạn cần chạy với user `gitlab-runner`

```bash
sudo usermod -aG docker gitlab-runner
```

- Kết nối runner với server GitLab

```bash
gitlab-runner register
```

- Runner sẽ hỏi:

```bash
GitLab URL: https://gitlab.com/
Registration token: glrt-mRHcE45qMW3PCInqOrkQAW86MQpwOjE4c3c4ZQp0OjMKdTppZjM5bBg.01.1j1pzzyk0
Executor: shell
```

- Executor bạn chọn `shell`

- Tạo file `.gitlab-ci.yml` trong thư mục gốc dự án

- Edit file `.gitlab-ci.yml`

```bash
sudo nano .gitlab-ci.yml
```

```yaml
stages:
  - deploy

deploy:
  stage: deploy
  tags:
    - docker-runner

  script:
    - cp "$ENV_FILE" .env
    - docker compose up -d --build

  only:
    - master
```

- Tạo env trên gitlab variable -> chọn type file -> tick log hidden -> KEY là ENV_FILE -> DESCRION là Env file -> VALUE paste file .env dự án vào là xong.

- Danh sách Git runner:

```bash
sudo gitlab-runner list
```

- Cách xoá bớt Git runner trùng nhau:

```bash
sudo nano /etc/gitlab-runner/config.toml
```

- Xoá

```toml
[[runners]]
  name = "cims-docker-runner"
  url = "https://gitlab.com/"
  token = "glrt-gehoc__Xk..."
  executor = "shell"
```

- Khởi động lại runner

```bash
sudo systemctl restart gitlab-runner
```

- Xoá toàn bộ runner

```bash
sudo gitlab-runner unregister --all-runners
```

## Các cài đặt PHP-FPM 8.3

```bash
sudo apt update
sudo apt install php8.3-fpm
systemctl status php8.3-fpm
```

## Cài đặt PHPMyAdmin với MariaDB với `Nginx`

```bash
sudo apt install -y phpmyadmin
sudo apt install mariadb-server mariadb-client -y
mysql --version
sudo systemctl start mariadb
sudo systemctl enable mariadb
sudo systemctl status mariadb
```

- Kiểm tra socket MariaDB

```bash
mysqladmin variables | grep socket
```

- Hiển thị kết quả

```bash
/run/mysqld/mysqld.sock
```

- Chạy bảo mật MariaDB

```bash
sudo mysql_secure_installation
```

Sau đó trả lời các câu hỏi bảo mật của `PHPMyadmin`

1. Cấu hình `Nginx` cho `PHPMyAdmin`

- Giả sử cần map Port `8083`
- Trường hợp này tương ứng với Phpmyadmin sẽ thuộc folder `/usr/share/phpmyadmin`

```nginx
server {
    listen 8083 default_server;
    listen [::]:8083 default_server;

    server_name _;

    root /usr/share/phpmyadmin;
    index index.php index.html index.htm;

    access_log /var/log/nginx/phpmyadmin_8083_access.log;
    error_log /var/log/nginx/phpmyadmin_8083_error.log;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php8.3-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg|woff|woff2|ttf)$ {
        expires 30d;
        access_log off;
        log_not_found off;
    }

    location ~ /\.ht {
        deny all;
    }
}
```

2. Tương tác Database `MariaDB`

- Login MariaDB trên Server Ubuntu (Mặc định trên Server Ubuntu username: `root` và password là rỗng)

```bash
mysql -u root -p
```

- Xem danh sách Database

```sql
SHOW DATABASES;
```

- Kết quả hiển thị

```bash
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| phpmyadmin         |
| abc_db             |
+--------------------+
```

- Chọn `abc_db` database hiện tại

```sql
USE abc_db;
```

- Xem dữ liệu bảng

```sql
SELECT * FROM users;
```

- Tạo `Database` mới:

```sql
CREATE DATABASE abc_db;
```

- Tạo mới user cho `Database` nếu server kết nối từ bất kỳ IP nào

```sql
CREATE USER 'ttptelecom123'@'%' IDENTIFIED BY 'TTPtelecom321';
```

- Tạo mới user cho `Database` nếu chỉ cho server `172.38.11.0`:

```sql
CREATE USER 'ttptelecom123'@'172.38.11.0' IDENTIFIED BY 'TTPtelecom321';
```

- Cấp toàn quyền cho `database`

```sql
GRANT ALL PRIVILEGES ON *.* TO 'ttptelecomDb1506'@'localhost' WITH GRANT OPTION;
```

- Cấp toàn quyền cho `database` nếu dùng IP:

```sql
GRANT ALL PRIVILEGES ON *.* TO 'ttptelecomDb1506'@'172.38.11.0' WITH GRANT OPTION;
```

- Áp dụng quyền

```sql
FLUSH PRIVILEGES;
```

- Kiểm tra user vừa tạo

```sql
SELECT user, host FROM mysql.user;
```

- Kiểm tra kết nối (lưu ý 3306 trong TH này Prisma cần mở Port)

```bash
mysql -u ttptelecom123 -p -h 172.38.11.0 -P 3306
```

- Mở/cho phép MariaDB listen trên tất cả IP bằng cách chỉnh bind-address = 0.0.0.0

```bash
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```

Tìm phần [mysqld] và thay hoặc thêm:

```bash
[mysqld]
bind-address = 0.0.0.0
```

Khởi động lại MariaDB

```bash
sudo systemctl restart mariadb
```

- Thoát khỏi chế độ truy xuất Mysql

```bash
exit
```

- Xoá 1 user mariadb

```sql
DROP USER 'username'@'host';
```

## Gitlab

### 1.1 Xử lý TH có 2 ssh key pub trên VPS

1. Cấu hình config trên VPS

```bash
# Cấu hình cho user dltphat301
Host gitlab-dltphat301
HostName gitlab.com
User git
IdentityFile ~/.ssh/id_ed25519_dltphat301

# Cấu hình cho user victordangdontuse
Host gitlab-victordangdontuse
HostName gitlab.com
User git
IdentityFile ~/.ssh/id_ed25519_victordangdontuse
```

2. Kiểm tra lại xem VPS đã connect tới Gitlab chưa

```bash
ssh -T git@gitlab-dltphat301
ssh -T git@gitlab-victordangdontuse
```

3. Thêm remote cho từng user account

```bash
git remote add gitlab-dltphat301 git@gitlab-dltphat301:ttp-group-software/cims/cims-ttp-api.git
git remote add gitlab-victordangdontuse git@gitlab-victordangdontuse:ttp-group-software/cims/cims-ttp-api.git
git remote -v
```

4. Sau đó tiến hành Fetching

- Git đã lấy tất cả các nhánh từ remote gitlab-dltphat301 và lưu chúng dưới dạng remote tracking branches:

```bash
git fetch gitlab-dltphat301
```

- Hiển thị như sau:

```bash
From gitlab-dltphat301:ttp-group-software/cims/cims-ttp-api
* [new branch] backup -> gitlab-dltphat301/backup
* [new branch] development -> gitlab-dltphat301/development
* [new branch] feat/add-deadline-approve-PersonalLeave -> gitlab-dltphat301/feat/add-deadline-approve-PersonalLeave
* [new branch] feat/refactor-notification -> gitlab-dltphat301/feat/refactor-notification
* [new branch] hieu_qua_kinh_doanh_moi -> gitlab-dltphat301/hieu_qua_kinh_doanh_moi
* [new branch] master -> gitlab-dltphat301/master
* [new branch] refactor/ticket-declare-v0 -> gitlab-dltphat301/refactor/ticket-declare-v0
```

5. Checkout nhánh development từ remote

```bash
git checkout -b development gitlab-dltphat301/development
```

6. Sau đó chỉ tiến hành pull code về

```bash
git pull
```

## Cài PostgreSQL trên Linux

### 1. Cài các gói cần thiết

```bash
sudo apt update
sudo apt install postgresql postgresql-contrib -y
```

### 2. Kiểm tra PostgreSQL chạy chưa

```bash
sudo systemctl status postgresql
```

### 3. Đăng nhập PostgreSQL

```bash
sudo -i -u postgres
psql
```

### 4. Tạo database + user

```sql
CREATE DATABASE phat_db;

CREATE USER phat WITH PASSWORD '123456';
ALTER USER phat WITH SUPERUSER CREATEDB CREATEROLE REPLICATION BYPASSRLS;
ALTER ROLE phat SET client_encoding TO 'utf8';
ALTER ROLE phat SET timezone TO 'Asia/Ho_Chi_Minh';

GRANT ALL PRIVILEGES ON DATABASE phat_db TO phat;

\l
\du
```

- Thoát:

```sql
exit;
```

### 5. Cho phép connect từ bên ngoài VPS

```bash
sudo nano /etc/postgresql/*/main/postgresql.conf
```

- Tìm tới dòng

```conf
#listen_addresses = 'localhost'
```

- Đổi thành

```conf
listen_addresses = '*'
```

### 6. Sửa quyền truy cập:

```bash
sudo nano /etc/postgresql/*/main/pg_hba.conf
```

- Thêm dòng này vào

```conf
host    all             all             0.0.0.0/0               md5
```

- Có thể thay chỗ này bằng ipv4 của VPS

```conf
host    all             all             <server_id>/32               md5
```

### 7. Restart PostgreSQL

```bash
sudo systemctl restart postgresql
```

### 8. Mở port VPS

```bash
sudo ufw allow 5432/tcp
```

- Kiểm tra lại port `postgres`

```bash
ss -tunlp | grep 5432
```

### 9. Mở `PgAdmin` để connect tới VPS bằng giao diện

### 10. Cấp full quyền

```sql
ALTER ROLE golive WITH SUPERUSER;
```


## VPN (Sercurity Server)

### Phần mềm WireGuard

1. Cấu hình phần mềm:

```conf
[Interface]
PrivateKey = <PrivateKey-Local>
Address = 100.0.0.23/32
DNS = 8.8.8.8

[Peer]
PublicKey = <PublicKey-Server-VPN>
AllowedIPs = <Allow-IP1>, <Allow-IP2>, <Allow-IP3>
Endpoint = 103.205.96.2:51820
PersistentKeepalive = 15
```

2. Chú thích:

- Khi mở phần mềm `WireGuard` sẽ có `Public key` phần `Key` này sẽ được whitelist bởi nhà cung cấp dịch vụ hoặc đội hạ tầng cấp Server của doanh nghiệp

- ```<Allow-IP1>```: IPv4 này thường sẽ được cấu hình giả sử như lớp mạng 10.0.0.0/24 (tức là phải có luôn prefix length)

- ```<PrivateKey-Local>``` giá trị này thường sẽ random theo từng thiết bị 

- ```<PublicKey-Server-VPN>``` sẽ được cung cấp bởi nhà cung cấp dịch vụ hoặc đội hạ tầng cấp Server của doanh nghiệp

## Cài đặt Flutter cho MacOS

1. Cài đặt và kiểm tra phiên bản

```bash
brew install --cask flutter
flutter --version
```

2. Kiểm tra PATH của flutter

```bash
which flutter
```

- Kết quả phải hiển thị

```bash
/opt/homebrew/bin/flutter
```

3. Setup flutter doctor

- Phải Dowload Xcode + Simulator + Android Studio và setup các tools cần thiết (HĐH: MacOS)

- Phải Dowload Android Studio và setup các tools cần thiết (HĐH: Window)

- Dowload flutter doctor

```bash
flutter doctor
flutter doctor --android-licenses
```

- Gõ y và nhấn Enter

- Dowload CocoaPods

```bash
brew install cocoapods
pod --version
```

## Lập trình viên Mobile và những công cụ

### Dowload Android Studio (Window / MacOS)

#### Setup 

- Mở Android Studio thao tác như sau: 

```bash
Android Studio > Settings > Languages & Frameworks > Android SDK
```

- Vào tab `SDK Tools` và tích chọn

- Bấm `Apply` để cài đặt tools cần thiết cho quá trình lập trình App.

```bash
Android SDK Command-line Tools
Android SDK Platform-Tools
Android SDK Build-Tools
Android Emulator
```

### Dowload Xcode + Dowload Simulator (MacOS)

```cmd
Alt + Shift + 2 (để tiến hành cài virtual tunnel)
```

### Khởi tạo Flutter

1. Trường hợp đã có tên thư mục dự án rồi

```bash
flutter create .
```

2. Hoặc trường hợp chưa có tên thư mục dự án

```bash
flutter create <ten_du_an>
```

