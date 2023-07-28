# BAUROV DOKÜMAN

## Jetson'u VNC Viewer'a Bağlama
- Ethernet kablosunu bağla
- Ağ ayarlarından Alt Ağ Maskesi'ni (Subnet) 255.255.255.0 yap. Detaylı anlatım:
    - Windows için Ayarlar -> Ağ ve Internet -> Gelişmiş Ağ Ayarları -> Diğer ağ bağdaştırıcı seçenekleri -> Sol menüden Bağdaştırıcı ayarlarını değiştirin -> Bağlı olan Ethernet sürücüsüne sağ tık -> Özellikler -> IPv4 Ayarlarını değiştir -> Ip adresi için bir adres salla 192.168.1.119 gibi -> Alt Ağ Maskesi 255.255.255.0 -> Gateway 192.168.1.1 -> DNS olarak 8.8.8.8, 8.8.4.4 kullanılabilir.

    - Linux (Gnome) için Settings -> Network -> Wired Kısmındaki Çarka tıkla -> IPv4 -> Manual seçip Windowstaki değerlerle aynı değerleri giriyoruz.

- VNC Viewer'dan baurov.local'e bağlan.
    - baurov.local çalışmazsa 192.168.1.180'e bağlanmayı dene
    