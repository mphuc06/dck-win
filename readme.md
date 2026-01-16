<h1 align="center">Windows<br />
<div align="center">
<a href="https://github.com/dockur/windows"><img src="https://github.com/dockur/windows/raw/master/.github/logo.png" title="Logo" style="max-width:100%;" width="128" /></a>
</div>
<div align="center">

[![Build]][build_url]
[![Version]][tag_url]
[![Size]][tag_url]
[![Package]][pkg_url]
[![Pulls]][hub_url]

</div></h1>

Windows inside a Docker container.

## Tính năng ✨

 - ISO downloader
 - KVM acceleration
 - Web-based viewer


## Để sử dụng 🐳

##### Với Docker Compose:

```yaml
services:
  windows:
    image: dockurr/windows
    container_name: windows
    environment:
      VERSION: "11"
    devices:
      - /dev/kvm
      - /dev/net/tun
    cap_add:
      - NET_ADMIN
    ports:
      - 8006:8006
      - 3389:3389/tcp
      - 3389:3389/udp
    volumes:
      - ./windows:/storage
    restart: always
    stop_grace_period: 2m
```

##### Với Docker CLI:

```bash
docker run -it --rm --name windows -e "VERSION=11" -p 8006:8006 --device=/dev/kvm --device=/dev/net/tun --cap-add NET_ADMIN -v "${PWD:-.}/windows:/storage" --stop-timeout 120 docker.io/dockurr/windows
```

##### Với Github Codespaces:

[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://github.com/codespaces/new?hide_repo_select=true&ref=master&repo=1132827945&skip_quickstart=true)

## FAQ 💬

### Làm thế nào để tôi sử dụng nó?

  Rất đơn giản, hãy làm theo tôi:
  
  - Vào nút "Code", chọn nút "..." ở phần Codespace và chọn nút "New with options...".

  - Chọn phiên bản cần cài đặt, cấu hình Codespace và bấm tạo Codespace.

  - Hãy thư giãn vì nó sẽ tự động cài đặt cho bạn.
   
  - Chỉ một lúc thôi, bạn sẽ được trải nghiệm Windows trên Github Codespace với port 8006
  
  Don't forget to star this repo!

### How do I select the Windows version?

  Mặc định khi cài, Windows 11 Pro sẽ được cài mặc định, nếu bạn muốn thay đổi thì bạn có thể file VERSION trong file yaml

  ```yaml
  environment:
    VERSION: "11"
  ```

  Chọn từ các giá trị dưới đây:
  
  | **Value** | **Version**            | **Size** |
  |---|---|---|
  | `11`   | Windows 11 Pro            | 7.2 GB   |
  | `11l`  | Windows 11 LTSC           | 4.7 GB   |
  | `11e`  | Windows 11 Enterprise     | 6.6 GB   |
  ||||
  | `10`   | Windows 10 Pro            | 5.7 GB   |
  | `10l`  | Windows 10 LTSC           | 4.6 GB   |
  | `10e`  | Windows 10 Enterprise     | 5.2 GB   |
  ||||
  | `8e`   | Windows 8.1 Enterprise    | 3.7 GB   |
  | `7u`   | Windows 7 Ultimate        | 3.1 GB   |
  | `vu`   | Windows Vista Ultimate    | 3.0 GB   |
  | `xp`   | Windows XP Professional   | 0.6 GB   |
  | `2k`   | Windows 2000 Professional | 0.4 GB   | 
  ||||  
  | `2025` | Windows Server 2025       | 6.7 GB   |
  | `2022` | Windows Server 2022       | 6.0 GB   |
  | `2019` | Windows Server 2019       | 5.3 GB   |
  | `2016` | Windows Server 2016       | 6.5 GB   |
  | `2012` | Windows Server 2012       | 4.3 GB   |
  | `2008` | Windows Server 2008       | 3.0 GB   |
  | `2003` | Windows Server 2003       | 0.6 GB   |
  | `no have` | Oprekin 10 21H2(200)       | 1.1 GB   |
  | `no have` | Oprekin 11 23H2(170)       | 1.5 GB   |
  | `no have` | Oprekin 11 25H2(Tiny 11 )       | 1.6 GB   |

> [!MẸO]
> Để cài đặt phiên bản ARM64 của Windows, hãy sử dụng [dockur/windows-arm](https://github.com/dockur/windows-arm/).

### Làm cách nào để thay đổi vị trí lưu trữ?

  Để thay đổi vị trí lưu trữ, hãy đưa liên kết gắn kết sau vào tệp soạn thảo của bạn:

  ```yaml
  volumes:
    - ./windows:/storage
  ```

  Thay thế đường dẫn ví dụ `./windows` bằng thư mục lưu trữ hoặc ổ đĩa được đặt tên mong muốn.

### Làm cách nào để thay đổi kích thước của đĩa?

 Để mở rộng kích thước mặc định là 64 GB, hãy thêm cài đặt `DISK_SIZE` vào tệp soạn thảo của bạn và đặt nó theo dung lượng ưa thích của bạn:

  ```yaml
  environment:
    DISK_SIZE: "256G"
  ```
  
> [!MẸO]
> Điều này cũng có thể được sử dụng để thay đổi kích thước ổ đĩa hiện có thành dung lượng lớn hơn mà không bị mất dữ liệu. Tuy nhiên, bạn sẽ cần [mở rộng phân vùng ổ đĩa theo cách thủ công](https://learn.microsoft.com/en-us/windows-server/storage/disk-management/extend-a-basic-volume?tabs=disk-management) vì dung lượng ổ đĩa đã thêm sẽ xuất hiện dưới dạng chưa được phân bổ.

### Làm cách nào để chia sẻ tập tin với máy chủ?

 Sau khi cài đặt sẽ có một thư mục tên là `Shared` trên màn hình của bạn, thư mục này có thể được sử dụng để trao đổi tập tin với máy chủ.

 Để chọn một thư mục trên máy chủ cho mục đích này, hãy đưa phần gắn kết liên kết sau vào tệp soạn thảo của bạn:

  ```yaml
  volumes:
    -  ./example:/shared
  ```

  Thay thế đường dẫn mẫu `./example` bằng thư mục dùng chung mà bạn mong muốn, sau đó thư mục này sẽ hiển thị dưới dạng `Được chia sẻ`.

### Làm cách nào để thay đổi dung lượng CPU hoặc RAM?

 Theo mặc định, Windows sẽ được phép sử dụng 2 nhân CPU và 4 GB RAM.

 Nếu bạn muốn điều chỉnh điều này, bạn có thể chỉ định số lượng mong muốn bằng cách sử dụng các biến môi trường sau:
  ```yaml
  environment:
    RAM_SIZE: "8G"
    CPU_CORES: "4"
  ```

### Làm cách nào để định cấu hình tên người dùng và mật khẩu?

 Theo mặc định, người dùng có tên `Docker` được tạo và mật khẩu của nó là `admin`.

 Nếu muốn sử dụng các thông tin xác thực khác trong khi cài đặt, bạn có thể định cấu hình chúng trong tệp soạn thảo của mình:

  ```yaml
  environment:
    USERNAME: "bill"
    PASSWORD: "gates"
  ```

### Làm cách nào để chọn ngôn ngữ Windows?

 Theo mặc định, phiên bản tiếng Anh của Windows sẽ được tải xuống.

 Nhưng bạn có thể thêm biến môi trường `LANGUAGE` vào tệp soạn thảo của mình để chỉ định ngôn ngữ thay thế sẽ được tải xuống:
  ```yaml
  environment:
    LANGUAGE: "French"
  ```
  
 Bạn có thể chọn giữa: 🇦🇪 tiếng Ả Rập, 🇧🇬 tiếng Bungari, 🇨🇳 tiếng Trung, 🇭🇷 tiếng Croatia, 🇨🇿 tiếng Séc, 🇩🇰 tiếng Đan Mạch, 🇳🇱 tiếng Hà Lan, 🇬🇧 tiếng Anh, 🇪🇪 tiếng Estonia, 🇫🇮 Tiếng Phần Lan, 🇫🇷 Tiếng Pháp, 🇩🇪 Tiếng Đức, 🇬🇷 Tiếng Hy Lạp, 🇮🇱 Tiếng Do Thái, 🇭🇺 Tiếng Hungary, 🇮🇹 Tiếng Ý, 🇯🇵 Tiếng Nhật, 🇰🇷 Tiếng Hàn, 🇱🇻 Tiếng Latvia, 🇱🇹 Tiếng Litva, 🇳🇴 Na Uy, 🇵🇱 Ba Lan, 🇵🇹 Bồ Đào Nha, 🇷🇴 Rumani, 🇷🇺 Nga, 🇷🇸 Serbia, 🇸🇰 Slovak, 🇸🇮 Slovenia, 🇪🇸 Tây Ban Nha, 🇸🇪 Thụy Điển, 🇹🇭 Thái, 🇹🇷 Thổ Nhĩ Kỳ và 🇺🇦 Ukraina.

### Làm cách nào để chọn bố cục bàn phím?

 Nếu bạn muốn sử dụng bố cục bàn phím hoặc ngôn ngữ không phải là mặc định cho ngôn ngữ đã chọn của mình, bạn có thể thêm các biến `KEYBOARD` và `REGION` như thế này:

  ```yaml
  environment:
    REGION: "en-US"
    KEYBOARD: "en-US"
  ```

### Làm cách nào để cài đặt hình ảnh tùy chỉnh?

 Để tải xuống ảnh ISO không được hỗ trợ, hãy chỉ định URL của nó trong biến môi trường `VERSION`:
  
  ```yaml
  environment:
    VERSION: "https://example.com/win.iso"
  ```

  Alternatively, you can also skip the download and use a local file instead, by binding it in your compose file in this way:
  
  ```yaml
  volumes:
    - ./example.iso:/boot.iso
  ```

  Replace the example path `./example.iso` with the filename of your desired ISO file. The value of `VERSION` will be ignored in this case.

### How do I run a script after installation?

  To run your own script after installation, you can create a file called `install.bat` and place it in a folder together with any additional files it needs (software to be installed for example).
  
  Then bind that folder in your compose file like this:

  ```yaml
  volumes:
    -  ./example:/oem
  ```

  The example folder `./example` will be copied to `C:\OEM` and the containing `install.bat` will be executed during the last step of the automatic installation.

### How do I perform a manual installation?

  It's recommended to stick to the automatic installation, as it adjusts various settings to prevent common issues when running Windows inside a virtual environment.

  However, if you insist on performing the installation manually at your own risk, add the following environment variable to your compose file:

  ```yaml
  environment:
    MANUAL: "Y"
  ```

### How do I connect using RDP?

  The web-viewer is mainly meant to be used during installation, as its picture quality is low, and it has no audio or clipboard for example.

  So for a better experience you can connect using any Microsoft Remote Desktop client to the IP of the container, using the username `Docker` and password `admin`.

  There is a RDP client for [Android](https://play.google.com/store/apps/details?id=com.microsoft.rdc.androidx) available from the Play Store and one for [iOS](https://apps.apple.com/nl/app/microsoft-remote-desktop/id714464092?l=en-GB) in the Apple Store. For Linux you can use [FreeRDP](https://www.freerdp.com/) and on Windows just type `mstsc` in the search box.

### How do I assign an individual IP address to the container?

  By default, the container uses bridge networking, which shares the IP address with the host. 

  If you want to assign an individual IP address to the container, you can create a macvlan network as follows:

  ```bash
  docker network create -d macvlan \
      --subnet=192.168.0.0/24 \
      --gateway=192.168.0.1 \
      --ip-range=192.168.0.100/28 \
      -o parent=eth0 vlan
  ```
  
  Be sure to modify these values to match your local subnet. 

  Once you have created the network, change your compose file to look as follows:

  ```yaml
  services:
    windows:
      container_name: windows
      ..<snip>..
      networks:
        vlan:
          ipv4_address: 192.168.0.100

  networks:
    vlan:
      external: true
  ```
 
  An added benefit of this approach is that you won't have to perform any port mapping anymore, since all ports will be exposed by default.

> [!IMPORTANT]  
> This IP address won't be accessible from the Docker host due to the design of macvlan, which doesn't permit communication between the two. If this is a concern, you need to create a [second macvlan](https://blog.oddbit.com/post/2018-03-12-using-docker-macvlan-networks/#host-access) as a workaround.

### How can Windows acquire an IP address from my router?

  After configuring the container for [macvlan](#how-do-i-assign-an-individual-ip-address-to-the-container), it is possible for Windows to become part of your home network by requesting an IP from your router, just like a real PC.

  To enable this mode, in which the container and Windows will have separate IP addresses, add the following lines to your compose file:

  ```yaml
  environment:
    DHCP: "Y"
  devices:
    - /dev/vhost-net
  device_cgroup_rules:
    - 'c *:* rwm'
  ```

### How do I add multiple disks?

  To create additional disks, modify your compose file like this:
  
  ```yaml
  environment:
    DISK2_SIZE: "32G"
    DISK3_SIZE: "64G"
  volumes:
    - ./example2:/storage2
    - ./example3:/storage3
  ```

### How do I pass-through a disk?

  It is possible to pass-through disk devices or partitions directly by adding them to your compose file in this way:

  ```yaml
  devices:
    - /dev/sdb:/disk1
    - /dev/sdc1:/disk2
  ```

  Use `/disk1` if you want it to become your main drive (which will be formatted during installation), and use `/disk2` and higher to add them as secondary drives (which will stay untouched).

### How do I pass-through a USB device?

  To pass-through a USB device, first lookup its vendor and product id via the `lsusb` command, then add them to your compose file like this:

  ```yaml
  environment:
    ARGUMENTS: "-device usb-host,vendorid=0x1234,productid=0x1234"
  devices:
    - /dev/bus/usb
  ```

  If the device is a USB disk drive, please wait until after the installation is fully completed before connecting it. Otherwise the installation may fail, as the order of the disks can get rearranged.

### How do I verify if my system supports KVM?

  First check if your software is compatible using this chart:

  | **Product**  | **Linux** | **Win11** | **Win10** | **macOS** |
  |---|---|---|---|---|
  | Docker CLI        | ✅   | ✅       | ❌        | ❌ |
  | Docker Desktop    | ❌   | ✅       | ❌        | ❌ | 
  | Podman CLI        | ✅   | ✅       | ❌        | ❌ | 
  | Podman Desktop    | ✅   | ✅       | ❌        | ❌ | 

  After that you can run the following commands in Linux to check your system:

  ```bash
  sudo apt install cpu-checker
  sudo kvm-ok
  ```

  If you receive an error from `kvm-ok` indicating that KVM cannot be used, please check whether:

  - the virtualization extensions (`Intel VT-x` or `AMD SVM`) are enabled in your BIOS.

  - you enabled "nested virtualization" if you are running the container inside a virtual machine.

  - you are not using a cloud provider, as most of them do not allow nested virtualization for their VPS's.

  If you did not receive any error from `kvm-ok` but the container still complains about a missing KVM device, it could help to add `privileged: true` to your compose file (or `sudo` to your `docker` command) to rule out any permission issue.

### How do I run macOS in a container?

  You can use [dockur/macos](https://github.com/dockur/macos) for that. It shares many of the same features, except for the automatic installation.

### How do I run a Linux desktop in a container?

  You can use [qemus/qemu](https://github.com/qemus/qemu) in that case.

### Is this project legal?

  Yes, this project contains only open-source code and does not distribute any copyrighted material. Any product keys found in the code are just generic placeholders provided by Microsoft for trial purposes. So under all applicable laws, this project will be considered legal.

## Disclaimer ⚖️

*The product names, logos, brands, and other trademarks referred to within this project are the property of their respective trademark holders. This project is not affiliated, sponsored, or endorsed by Microsoft Corporation.*

[build_url]: https://github.com/dockur/windows/
[hub_url]: https://hub.docker.com/r/dockurr/windows/
[tag_url]: https://hub.docker.com/r/dockurr/windows/tags
[pkg_url]: https://github.com/dockur/windows/pkgs/container/windows

[Build]: https://github.com/dockur/windows/actions/workflows/build.yml/badge.svg
[Size]: https://img.shields.io/docker/image-size/dockurr/windows/latest?color=066da5&label=size
[Pulls]: https://img.shields.io/docker/pulls/dockurr/windows.svg?style=flat&label=pulls&logo=docker
[Version]: https://img.shields.io/docker/v/dockurr/windows/latest?arch=amd64&sort=semver&color=066da5
[Package]: https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fipitio.github.io%2Fbackage%2Fdockur%2Fwindows%2Fwindows.json&query=%24.downloads&logo=github&style=flat&color=066da5&label=pulls
