# **Sanal Makineler ve Metasploit Framework kullanarak Pivoting Saldırısı**
### Pivoting, ortak bir ağda olduğumuz makineyi kullanarak, o makinenin bağlı olduğu bir diğer ağ varsa o ağı da ortak kullanan makineyi hackleme işlemidir. Şekil 1'de görüldüğü üzere sol tarafta saldırgan makine, ortada iki ayrı ağa bağlı olan diğer makine, sağda ise hedefteki makine bulunmaktadır.
![This is an alt text.](https://github.com/yunusemredincell/Pivoting-using-Metasploit/blob/main/pivotingimage/genelgorunum.png "Genel Gorunum")

#### _Şekil 1_

## **Kendi Laboratuvar Ortamınızı Oluşturma**
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

### Seçilen oturum *sessions -u 1* komutuyla upgrade edilir. Upgrade sayesinde multi/handler çalıştırılarak ayrı bir meterpreter oturumu açılır (Ek 13). Bu oturumda *sessions -l* ile görüntülenir. *use2* diyerek meterpreter oturumu açılır. *sessions -i 2* ile de etkileşim (interaction) başlatılır (Ek 14). Böylece hedef makineye erişmiş oluruz. *sysinfo* çalıştırarak işletim sistemi ve mimarisi hakkında bilgiler edinebiliriz. 

![This is an alt text.](https://github.com/yunusemredincell/Pivoting-using-Metasploit/blob/main/pivotingimage/upgrade.png "UPGRADE")

#### _Ek 13_

![This is an alt text.](https://github.com/yunusemredincell/Pivoting-using-Metasploit/blob/main/pivotingimage/interaction.png "INTERACTION")

#### _Ek 14_
### Bu adımdan sonra *ifconfig* çalıştırarak makinenin ağ bağlantılarıyla alakalı IP bilgilerini görüntülenir (Ek 15). Burada bağlı olunan başka bir ağın olduğu tespit edilir ve bu ağ için saldırılar yapılır. 

![This is an alt text.](https://github.com/yunusemredincell/Pivoting-using-Metasploit/blob/main/pivotingimage/ipinfo.png "IPINFO")

#### _Ek 15_

### Ek 16'da görüldüğü gibi *background* komutuyla session 2'nin arka planda çalışmaya devam etmesi sağlanmıştır. Bunun ardından msfconcole üzerinden *search autoroute* ile otomatik yönlendirme yapılması amaçlanmıştır. Session bilgisi olarak arka planda çalışan session 2 tanımlandıktan sonra *run* komutuyla modül çalıştırılmıştır. Bunun sonucunda diğer ağ için bir route oluşturulmuştur (Ek 17).
![This is an alt text.](https://github.com/yunusemredincell/Pivoting-using-Metasploit/blob/main/pivotingimage/autoroute.png "AUTOROUTE")

#### _Ek 16_

![This is an alt text.](https://github.com/yunusemredincell/Pivoting-using-Metasploit/blob/main/pivotingimage/routeadd.png "ROUTEADD")

#### _Ek 17_

### Oluşturulan route'un sonucunu görebilmek için ayrı bir terminalde *msfdb init* komutu çalıştırıldıktan sonra msfconsole üzerinden autoroute modülü içinde Ek 18'te görüldüğü şekliyle *hosts* komutu çalıştırılır. Bu komutun ardından aynı ağda oldukları için pivoting uygulayacağımız Metasploitable Kopya makinesinin IP'si gözükmektedir (192.168.81.129).  
![This is an alt text.](https://github.com/yunusemredincell/Pivoting-using-Metasploit/blob/main/pivotingimage/hosts.png "HOSTS")

#### _Ek 18_

### Aynı ağda iletişim halinde oldukları için hosts komutuyla IP'leri görünen makinelere ping_sweep modülünü kullanarak aynı ağda olan cihazlara ping atılır. *search ping_sweep* msfconsole'da çalıştırılır. Ek 19'daki gibi ayarlamalar yapılır. set rhosts ayarlanırken IP aralığı için CIDR adresleme yöntemi kullanılır. Bu işlemlerin ardından 192.168.81.128 ve 192.168.81.129 numaralı IP adresleri bulunduktan sonra ping atılmış olur. 

![This is an alt text.](https://github.com/yunusemredincell/Pivoting-using-Metasploit/blob/main/pivotingimage/ping.png "PING")

#### _Ek 19_

### Bu adımdan sonra portscan modülü kullanılarak makinelere ait açık port analizi yapılır. Ek 20'de görüldüğü gibi modüle girilerek TCP port analizi seçilir. TCP portunun seçilme sebebi kolay erişebilmesidir. options komutuyla rhosts'u asıl hedef makinenin IP'si olarak girilir (Ek 21). Exploit (Sömürmek) edildikten sonra 192.168.81.129'a ait açık portlar tespit edilir.  

![This is an alt text.](https://github.com/yunusemredincell/Pivoting-using-Metasploit/blob/main/pivotingimage/port.png "PORT")

#### _Ek 20_

![This is an alt text.](https://github.com/yunusemredincell/Pivoting-using-Metasploit/blob/main/pivotingimage/portexploit.png "PORTEXPLOIT")

#### _Ek 21_



### Port analizinden sonra *ssh_login* modulüne geri dönerek (Ek 22) 192.168.81.128 IP'li makine için sızma testleri yapılır. Metasploitable makine için ayarlar Ek 23'teki gibi olduğundan emin olduktan sonra exploit komutu ile brute-force saldırısı başlar. Ardından yeni açılan session 3 seçilir ve etkileşime geçilir (Ek 24). 
![This is an alt text.](https://github.com/yunusemredincell/Pivoting-using-Metasploit/blob/main/pivotingimage/backtossh.png "BACKTOSSH")

#### _Ek 22_

![This is an alt text.](https://github.com/yunusemredincell/Pivoting-using-Metasploit/blob/main/pivotingimage/exploitvm1.png "EXPLOITVM1")

#### _Ek 23_
![This is an alt text.](https://github.com/yunusemredincell/Pivoting-using-Metasploit/blob/main/pivotingimage/vm1interact.png "VM1INTERACT")

#### _Ek 24_

### Bu adımdan sonra background komutu ile session 3 arka planda çalıştırılır ve asıl hedef makinenin IP'si (192.168.81.129) rhost'a ayarlanarak exploit komutu çalıştırılır.

![This is an alt text.](https://github.com/yunusemredincell/Pivoting-using-Metasploit/blob/main/pivotingimage/exploitvm2.png "EXPLOITVM2")

#### _Ek 25_

### Ek 26'da gözüktüğü şekilde 4. bir session açılır. Bu oturumun bağlantı yolunu incelediğimiz zaman 192.168.18.130 IP adresli Kali Linux saldırgan makinemizden 192.168.81.129 IP adresli kurban makineye erişebildiğimizi görüyoruz. *sessions -i 4*  (interact) komutuyla hedef makinemize sızma işlemimiz tamamlanmış olur.
### Test etmek için kurban makinenin içinde *ifconfig* komutu çalıştırdığımızda Ek 27'deki şekilde IP adresi görülmektedir.

![This is an alt text.](https://github.com/yunusemredincell/Pivoting-using-Metasploit/blob/main/pivotingimage/vm2interact.png "VM2INTERACT")

#### _Ek 26_

![This is an alt text.](https://github.com/yunusemredincell/Pivoting-using-Metasploit/blob/main/pivotingimage/result.png "RESULT")

#### _Ek 27_

## Sonuç
### 3 ayrı Linux sanal makinesi kullanarak kendi laboratuvar ortamımı oluşturduğum bu projede siber dünyada sıklıkla karşılaşılan pivoting yönteminin uygulanmasını anlatmaya çalıştım. Yapacağınız yöntemlerin yalnızca sanal makineler için kullanılması gerekmektedir. Gerçek kullanıcılar üzerinde bu işlemleri uygulamanız suçtur ve yasalar uyarınca cezası bulunmaktadır. Pivoting ortak bir ağa sahip kullanıcıların başka ağlardaki diğer cihazlar için büyük bir tehdit oluşturmaktadır. Kötü niyetli kişilerin sıklıkla kullandığı bu yöntemin önüne geçmek için ortak ağları kullanmaktan kaçınmak gerekir.  
