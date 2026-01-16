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
  | `no have` | Oprekin 10 22H2       | 1.2 GB   |
  | `no have` | Oprekin 10 19H2(not have win powershell )       | 1.6 GB   |

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

  Ngoài ra, bạn cũng có thể bỏ qua quá trình tải xuống và thay vào đó sử dụng tệp cục bộ bằng cách liên kết tệp đó trong tệp soạn thảo của bạn theo cách sau:
  
  ```yaml
  volumes:
    - ./example.iso:/boot.iso
  ```

### Làm cách nào để chạy tập lệnh sau khi cài đặt?

 Để chạy tập lệnh của riêng bạn sau khi cài đặt, bạn có thể tạo một tệp có tên `install.bat` và đặt nó vào một thư mục cùng với bất kỳ tệp bổ sung nào mà nó cần (ví dụ: phần mềm sẽ được cài đặt).

 Sau đó liên kết thư mục đó trong tệp soạn thảo của bạn như thế này:

  ```yaml
  volumes:
    -  ./example:/oem
  ```

 Thư mục ví dụ `./example` sẽ được sao chép vào `C:\OEM` và thư mục chứa `install.bat` sẽ được thực thi trong bước cuối cùng của quá trình cài đặt tự động.

### Làm cách nào để thực hiện cài đặt thủ công?

 Bạn nên sử dụng cài đặt tự động vì nó điều chỉnh các cài đặt khác nhau để ngăn các sự cố thường gặp khi chạy Windows trong môi trường ảo.

 Tuy nhiên, nếu bạn nhất quyết thực hiện cài đặt theo cách thủ công, bạn sẽ phải tự chịu rủi ro, hãy thêm biến môi trường sau vào tệp soạn thảo của mình:

  ```yaml
  environment:
    MANUAL: "Y"
  ```

### Làm cách nào để kết nối bằng RDP?

 Trình xem web chủ yếu được sử dụng trong quá trình cài đặt, vì chất lượng hình ảnh của nó thấp và không có âm thanh hoặc bảng nhớ tạm chẳng hạn.

 Vì vậy, để có trải nghiệm tốt hơn, bạn có thể kết nối bằng bất kỳ ứng dụng khách Microsoft Remote Desktop nào với IP của vùng chứa, sử dụng tên người dùng `Docker` và mật khẩu `admin`.

 Có một ứng dụng khách RDP cho [Android](https://play.google.com/store/apps/details?id=com.microsoft.rdc.androidx) có sẵn trên Cửa hàng Play và một ứng dụng dành cho [iOS](https://apps.apple.com/nl/app/microsoft-remote-desktop/id714464092?l=en-GB) trong Apple Store. Đối với Linux, bạn có thể sử dụng [FreeRDP](https://www.freerdp.com/) và trên Windows, chỉ cần nhập `mstsc` vào hộp tìm kiếm.

### Làm cách nào để gán địa chỉ IP riêng lẻ cho vùng chứa?

 Theo mặc định, container sử dụng mạng cầu nối, chia sẻ địa chỉ IP với máy chủ.

 Nếu bạn muốn gán một địa chỉ IP riêng cho vùng chứa, bạn có thể tạo mạng macvlan như sau:

  ```bash
  docker network create -d macvlan \
      --subnet=192.168.0.0/24 \
      --gateway=192.168.0.1 \
      --ip-range=192.168.0.100/28 \
      -o parent=eth0 vlan
  ```
  
  Hãy nhớ sửa đổi các giá trị này để phù hợp với mạng con cục bộ của bạn.

 Khi bạn đã tạo mạng, hãy thay đổi tệp soạn thảo của bạn trông như sau:

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
 
 Một lợi ích bổ sung của phương pháp này là bạn sẽ không phải thực hiện bất kỳ ánh xạ cổng nào nữa vì tất cả các cổng sẽ được hiển thị theo mặc định.

> [! QUAN TRỌNG]
> Địa chỉ IP này sẽ không thể truy cập được từ máy chủ Docker do thiết kế của macvlan không cho phép liên lạc giữa hai địa chỉ. Nếu đây là vấn đề đáng lo ngại, bạn cần tạo [macvlan thứ hai](https://blog.oddbit.com/post/2018-03-12-USE-docker-macvlan-networks/#host-access) để giải quyết.

### Làm cách nào Windows có thể lấy địa chỉ IP từ bộ định tuyến của tôi?

 Sau khi định cấu hình vùng chứa cho [macvlan](#how-do-i-sign-an-individual-ip-address-to-the-container), Windows có thể trở thành một phần của mạng gia đình của bạn bằng cách yêu cầu IP từ bộ định tuyến của bạn, giống như một PC thực.

 Để bật chế độ này, trong đó vùng chứa và Windows sẽ có các địa chỉ IP riêng biệt, hãy thêm các dòng sau vào tệp soạn thảo của bạn:

  ```yaml
  environment:
    DHCP: "Y"
  devices:
    - /dev/vhost-net
  device_cgroup_rules:
    - 'c *:* rwm'
  ```

### Làm cách nào để thêm nhiều đĩa?

Để tạo thêm đĩa, hãy sửa đổi tệp soạn thảo của bạn như thế này:

  ```yaml
  environment:
    DISK2_SIZE: "32G"
    DISK3_SIZE: "64G"
  volumes:
    - ./example2:/storage2
    - ./example3:/storage3
  ```

### Làm cách nào để chuyển qua đĩa?

Có thể chuyển trực tiếp các thiết bị đĩa hoặc phân vùng bằng cách thêm chúng vào tệp soạn thảo của bạn theo cách này:

  ```yaml
  devices:
    - /dev/sdb:/disk1
    - /dev/sdc1:/disk2
  ```

 Sử dụng `/disk1` nếu bạn muốn nó trở thành ổ đĩa chính của mình (sẽ được định dạng trong khi cài đặt) và sử dụng `/disk2` trở lên để thêm chúng làm ổ đĩa phụ (sẽ không bị ảnh hưởng).

### Làm cách nào để chuyển qua thiết bị USB?

 Để chuyển qua thiết bị USB, trước tiên hãy tra cứu id nhà cung cấp và sản phẩm của thiết bị đó thông qua lệnh `lsusb`, sau đó thêm chúng vào tệp soạn thảo của bạn như sau:

  ```yaml
  environment:
    ARGUMENTS: "-device usb-host,vendorid=0x1234,productid=0x1234"
  devices:
    - /dev/bus/usb
  ```

 Nếu thiết bị là ổ đĩa USB, vui lòng đợi cho đến khi quá trình cài đặt hoàn tất trước khi kết nối. Nếu không, quá trình cài đặt có thể không thành công vì thứ tự của các đĩa có thể bị sắp xếp lại.

### Làm cách nào để xác minh xem hệ thống của tôi có hỗ trợ KVM hay không?

 Trước tiên hãy kiểm tra xem phần mềm của bạn có tương thích hay không bằng biểu đồ này:

  | **Product**  | **Linux** | **Win11** | **Win10** | **macOS** |
  |---|---|---|---|---|
  | Docker CLI        | ✅   | ✅       | ❌        | ❌ |
  | Docker Desktop    | ❌   | ✅       | ❌        | ❌ | 
  | Podman CLI        | ✅   | ✅       | ❌        | ❌ | 
  | Podman Desktop    | ✅   | ✅       | ❌        | ❌ | 

  Sau đó, bạn có thể chạy các lệnh sau trong Linux để kiểm tra hệ thống của mình:

  ```bash
  sudo apt install cpu-checker
  sudo kvm-ok
  ```

  Nếu bạn nhận được lỗi từ `kvm-ok` cho biết không thể sử dụng KVM, vui lòng kiểm tra xem:

- các tiện ích mở rộng ảo hóa (`Intel VT-x` hoặc `AMD SVM`) được bật trong BIOS của bạn.

- bạn đã bật "ảo hóa lồng nhau" nếu bạn đang chạy vùng chứa bên trong máy ảo.

- bạn hiện không sử dụng nhà cung cấp đám mây vì hầu hết họ không cho phép ảo hóa lồng nhau cho VPS của họ.

Nếu bạn không nhận được bất kỳ lỗi nào từ `kvm-ok` nhưng vùng chứa vẫn phàn nàn về việc thiếu thiết bị KVM, bạn có thể thêm `privileged: true` vào tệp soạn thảo của mình (hoặc `sudo` vào lệnh `docker` của bạn) để loại trừ mọi vấn đề về quyền.

### Làm cách nào để chạy macOS trong vùng chứa?

Bạn có thể sử dụng [dockur/macos](https://github.com/dockur/macos) để làm việc đó. Nó có nhiều tính năng giống nhau, ngoại trừ việc cài đặt tự động.

### Làm cách nào để chạy máy tính để bàn Linux trong vùng chứa?

Bạn có thể sử dụng [qemus/qemu](https://github.com/qemus/qemu) trong trường hợp đó.

###Dự án này có hợp pháp không?

Có, dự án này chỉ chứa mã nguồn mở và không phân phối bất kỳ tài liệu có bản quyền nào. Mọi khóa sản phẩm được tìm thấy trong mã chỉ là các phần giữ chỗ chung do Microsoft cung cấp cho mục đích dùng thử. Vì vậy, theo tất cả các luật hiện hành, dự án này sẽ được coi là hợp pháp.

## Tuyên bố từ chối trách nhiệm ⚖️

*Tên sản phẩm, logo, nhãn hiệu và các nhãn hiệu khác được đề cập trong dự án này là tài sản của chủ sở hữu nhãn hiệu tương ứng. Dự án này không được liên kết, tài trợ hoặc chứng thực bởi Tập đoàn Microsoft.*

[build_url]: https://github.com/dockur/windows/
[hub_url]: https://hub.docker.com/r/dockurr/windows/
[tag_url]: https://hub.docker.com/r/dockurr/windows/tags
[pkg_url]: https://github.com/dockur/windows/pkgs/container/windows

[Build]: https://github.com/dockur/windows/actions/workflows/build.yml/badge.svg
[Size]: https://img.shields.io/docker/image-size/dockurr/windows/latest?color=066da5&label=size
[Pulls]: https://img.shields.io/docker/pulls/dockurr/windows.svg?style=flat&label=pulls&logo=docker
[Version]: https://img.shields.io/docker/v/dockurr/windows/latest?arch=amd64&sort=semver&color=066da5
[Package]: https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fipitio.github.io%2Fbackage%2Fdockur%2Fwindows%2Fwindows.json&query=%24.downloads&logo=github&style=flat&color=066da5&label=pulls
