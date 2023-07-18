# **Sanal Makineler ve Metasploit Framework kullanarak Pivoting**
### Pivoting, ortak bir ağda olduğumuz makineyi kullanarak, o makinenin bağlı olduğu bir diğer ağ varsa o ağı da ortak kullanan makineyi hackleme işlemidir. Şekil 1'de görüldüğü üzere sol tarafta saldırgan makine, ortada iki ayrı ağa bağlı olan diğer makine, sağda ise hedefteki makine bulunmaktadır.
![This is an alt text.](https://github.com/yunusemredincell/Pivoting-using-Metasploit/blob/main/pivotingimage/genelgorunum.png "Genel Gorunum")

#### _Şekil 1_

## **Kendi Laboratuvar ortamınızı oluşturma**
### Pivoting işlemini uygulayabilmek için 3 ayrı sanal makine kullanmak gerekmektedir. Bu ortamı hazırlarken ben VMware Workstation Pro 17 kullandım ve anlatımlarım VMware üzerinden olacak, ancak tüm işlemler Oracle VirtualBox ve diğer sanal makine ortamları için de geçerlidir. 
### **Kurulumlar**

### vmware.com üzerinden kurumları tamamladıktan sonra Help -> Enter a Licence Key adımlarını takip ederek internetten bulduğunuz güncel lisans anahtarını kullanarak kullanıma başlayabilirsiniz. Ardından Kali Linux ve Metasploitable2 sanal makinelerinin kurulumları tamamlanır. Metasploitable makinenin ayarları Ek 1'deki gibi olmalıdır. Kurduğumuz metasploitable makineyi görseldeki gibi klonlayarak ikinci bir metasploitable makine elde ederiz. (Ek 2)

![This is an alt text.](https://github.com/yunusemredincell/Pivoting-using-Metasploit/blob/main/pivotingimage/setup.png "Setup")

#### _Ek 1_

![This is an alt text.](https://github.com/yunusemredincell/Pivoting-using-Metasploit/blob/main/pivotingimage/clone.png "Klonlama")

#### _Ek 2_

### **Ayarların yapımı** 
### Kurduğumuz Metasploitable makinenin Ağ ayarlarını pivoting işlemine uygun hale getirmemiz gerekir. Görsellerdeki gibi iki ayrı ağ ayarlanır. (Ek 3 ve Ek 4) Birinci ağ Kali Linux ile aynı ağda olacağı için NAT ağı olarak ayarlanır. İkinci ağ ise kopyalanan metasploitable makineyle aynı ağda olması için Custom olarak vmnet19 şeklinde ayarlanarak spesifik bir ağ ayarlanmış olur. Bu sayede ip statik olarak tanımlanabilir. Kopyaladığımız makinenin de ağ ayarları şekildeki gibi ayarlanarak Pivoting işlemine hazır hale gelir.

![This is an alt text.](https://github.com/yunusemredincell/Pivoting-using-Metasploit/blob/main/pivotingimage/nat.png "NAT Ağı")

#### _Ek 3_

![This is an alt text.](https://github.com/yunusemredincell/Pivoting-using-Metasploit/blob/main/pivotingimage/vmnet19.png "VMnet19")

#### _Ek 4_

### **Sanal Makineleri Çalıştırma ve Gerekli Konfigürasyonları ayarlama**
### Üç sanal makineyi de çalıştırılır. Metasploit makinelerde kullanıcı adı ve şifre *msfadmin* dir.  Metasploitable makinemizde ikinci ağı tamamlamak için "*sudo ip link set dev eth1 down*" ve "*sudo dhclient eth1*" komutlarıyla ikinci ağımız için statik olarak ip oluşturulur. (Ek 5 ve Ek 6). 
![This is an alt text.](https://github.com/yunusemredincell/Pivoting-using-Metasploit/blob/main/pivotingimage/dhclient.png "Dhclient")

#### _Ek 5_
![This is an alt text.](https://github.com/yunusemredincell/Pivoting-using-Metasploit/blob/main/pivotingimage/eth1.png "ETH1")

#### _Ek 6_

### Oluşturlan IP'ler ve Makineler benim Pivoting işlemim için şu şekildedir:
### Kali Linux eth0: 192.168.18.128
### Metasploitable eth0: 192.168.18.130 eth1: 192.168.81.128
### Metasploitable Kopya: eth0: 192.168.81.129

## **Pivoting'e Başlama**
### İlk olarak aynı ağda olduğumuz 192.168.18.130 IP'li makineye Kali Linux üzerinden nmap sorgulaması yapılır. (Ek 7) nmap taramasının ardından açık portlar bulunur. Bu portlardan herhangi biri üzerinden hedef makineye sızma gerçekleştirilir. Benim yapacağım Pivoting işlemi için 22 port numaralı SSH portu üzerinden hedef makineye sımza testi uygulanır (Ek 8).  

![This is an alt text.](https://github.com/yunusemredincell/Pivoting-using-Metasploit/blob/main/pivotingimage/nmap.png "NMAP")

#### _Ek 7_

![This is an alt text.](https://github.com/yunusemredincell/Pivoting-using-Metasploit/blob/main/pivotingimage/ssh.png "SSH")

#### _Ek 8_
### Kali Linux üzerinden msfconsole çalıştırılır. msfconsole modülü üzerinden *search ssh_login* komutuyla devam edilir (Ek 9). 
![This is an alt text.](https://github.com/yunusemredincell/Pivoting-using-Metasploit/blob/main/pivotingimage/searchssh.png "SSHSEARCH")

#### _Ek 9_

### Çıkan seçeceklerden *use 0* komutuyla modül seçimi yapılır. Modülün içinde *options* komutu kullanılarak modül içinde konfigürasyonlar ayarlanır. Ek 10'da gözüktüğü gibi ayarlamalar yapılarak hedef makine için teknik hazırlıklar tamamlanır. Ayarlamalarınızın Ek 11'deki gözüktüğünden emin olduktan sonra saldırı kısmına geçilir.

![This is an alt text.](https://github.com/yunusemredincell/Pivoting-using-Metasploit/blob/main/pivotingimage/setssh.png "SETSSH")

#### _Ek 10_

![This is an alt text.](https://github.com/yunusemredincell/Pivoting-using-Metasploit/blob/main/pivotingimage/showssh.png "SHOWSSH")

#### _Ek 11_

### exploit komutu çalıştırarak hedef makine için bruteforce ataklara başlanır. Ayarlanan şifre ve kullanıcı adı doğruysa oturum açılır. Açılan oturumları listelemek için *sessions -l* komutu çalıştırılarak oturum seçenekleri gözükür (Ek 12).

![This is an alt text.](https://github.com/yunusemredincell/Pivoting-using-Metasploit/blob/main/pivotingimage/exploitssh.png "EXPLOITSSH")

#### _Ek 12_

### Seçilen oturum *sessions -u 1* komutuyla upgrade edilir. Upgrade sayesinde multi/handler çalıştırılarak ayrı bir meterpreter oturumu açılır (Ek 13). Bu oturumda *sessions -l* ile görüntülenir. 

![This is an alt text.](https://github.com/yunusemredincell/Pivoting-using-Metasploit/blob/main/pivotingimage/upgrade.png "UPGRADE")

#### _Ek 13_
