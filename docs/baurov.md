# BAUROV DOKÜMAN

## Jetson'u VNC Viewer'a Bağlama
- Ethernet kablosunu bağla
- Ağ ayarlarından Alt Ağ Maskesi'ni (Subnet) 255.255.255.0 yap. Detaylı anlatım:
    - Windows için Ayarlar -> Ağ ve Internet -> Gelişmiş Ağ Ayarları -> Diğer ağ bağdaştırıcı seçenekleri -> Sol menüden Bağdaştırıcı ayarlarını değiştirin -> Bağlı olan Ethernet sürücüsüne sağ tık -> Özellikler -> IPv4 Ayarlarını değiştir -> Ip adresi için bir adres salla 192.168.1.119 gibi -> Alt Ağ Maskesi 255.255.255.0 -> Gateway 192.168.1.1 -> DNS olarak 8.8.8.8, 8.8.4.4 kullanılabilir.

    - Linux (Gnome) için Settings -> Network -> Wired Kısmındaki Çarka tıkla -> IPv4 -> Manual seçip Windowstaki değerlerle aynı değerleri giriyoruz.

- VNC Viewer'dan baurov.local'e bağlan.
    - baurov.local çalışmazsa 192.168.1.180'e bağlanmayı dene
    
## Servo Kontrol
- Pixhawk üzerindeki aux pinoutları servo ve röleler için ayrılmıştır. 6 adet aux pini bulunmaktadır ve BRD_PWM_COUNT parametresine göre giriş - çıkış veya pwm sinyali olduğuna karar verilir. Aşağıdaki tabloda detaylı olarak görebilirsiniz.


    | BRD_PWM_COUNT <td colspan=6>   Aux pin configuration |      |      |      |      |      |      |
    |------------------------------------------------------|------|------|------|------|------|------|
    |                                                      | 1    | 2    | 3    | 4    | 5    | 6    |
    | 0                                                    | GPIO | GPIO | GPIO | GPIO | GPIO | GPIO |
    | 2                                                    | PWM  | PWM  | GPIO | GPIO | GPIO | GPIO |
    | 4 (default)                                          | PWM  | PWM  | PWM  | PWM  | GPIO | GPIO |
    | 6                                                    | PWM  | PWM  | PWM  | PWM  | PWM  | PWM  |

- Parametreler kısmından SERVOn_FUNCTION (n kısmı servo numarasıdır.) 0 yani kapalı olmalıdır. Aksi taktirde bazı fonksiyonlarla çakışma yapabilir.

- Servo kanallarının Pixhawk çıkışları ile eşleşmesini aşağıdaki tabloda görebilirsiniz.

    | Pixhawk Label | Servo Channel |
    |---------------|---------------|
    | Main 1        | 1             |
    | Main 2        | 2             |
    | Main 3        | 3             |
    | Main 4        | 4             |
    | Main 5        | 5             |
    | Main 6        | 6             |
    | Main 7        | 7             |
    | Main 8        | 8             |
    | Aux 1         | 9             |
    | Aux 2         | 10            |
    | Aux 3         | 11            |
    | Aux 4         | 12            |
    | Aux 5         | 13            |
    | Aux 6         | 14            |


