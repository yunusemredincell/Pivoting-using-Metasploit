# **Sanal Makineler ve Metasploit Framework kullanarak Pivoting**
### Pivoting, ortak bir ağda olduğumuz makineyi kullanarak, o makinenin bağlı olduğu bir diğer ağ varsa o ağı da ortak kullanan makineyi hackleme işlemidir. Şekilde görüldüğü üzere sol tarafta saldırgan makine, ortada iki ayrı ağa bağlı olan diğer makine, sağda ise hedefteki makine bulunmaktadır. (görsel1.png)
## Kendi Laboratuvar ortamınızı oluşturma
### Pivoting işlemini uygulayabilmek için 3 ayrı sanal makine kullanmak gerekmektedir. Bu ortamı hazırlarken ben VMware Workstation Pro 17 kullandım ve anlatımlarım VMware üzerinden olacak, ancak tüm işlemler Oracle VirtualBox ve diğer sanal makine ortamları için de geçerlidir. 
### ** Kurulumlar**
### vmware.com üzerinden kurumları tamamladıktan sonra Help -> Enter a Licence Key adımlarını takip ederek internetten bulduğunuz güncel lisans anahtarını kullanarak kullanıma başlayabilirsiniz. Ardından Kali Linux ve Metasploitable2 sanal makinelerinin kurulumları tamamlanır. Metasploitable makinenin ayarları şekildeki gibi olmalıdır. (setup) Kurduğumuz metasploitable makineyi görseldeki gibi klonlayarak ikinci bir metasploitable makine elde ederiz. (clone)
### **Ayarların yapımı** 
### Kurduğumuz Metasploitable makinenin Ağ ayarlarını pivoting işlemine uygun hale getirmemiz gerekir. Görsellerdeki gibi iki ayrı ağ ayarlanır. Birinci ağ Kali Linux ile aynı ağda olacağı için NAT ağı olarak ayarlanır. İkinci ağ ise kopyalanan metasploitable makineyle aynı ağda olması için Custom olarak vmnet19 şeklinde ayarlanarak spesifik bir ağ ayarlanmış olur. Bu sayede ip statik olarak tanımlanabilir. Kopyaladığımız makinenin de ağ ayarları şekildeki gibi ayarlanarak Pivoting işlemine hazır hale gelir.
### **Sanal Makineleri Çalıştırma ve Gerekli Konfigürasyonları ayarlama**
### Üç sanal makineyi de çalıştırılır. Metasploit makinelerde kullanıcı adı ve şifre *msfadmin* dir.  Metasploitable makinemizde ikinci ağı tamamlamak için "*sudo ip link set dev eth1 down*" ve "*sudo dhclient eth1*" komutlarıyla ikinci ağımız için statik olarak ip oluşturulur. (dhclient ve eth1). Oluşturlan IP'ler ve Makineler benim Pivoting işlemim için şu şekildedir:
### Kali Linux eth0: 192.168.18.128
### Metasploitable eth0: 192.168.18.130 eth1: 192.168.81.128
### Metasploitable Kopya: eth0: 192.168.81.129
### **Pivoting'e Başlama**
### 
