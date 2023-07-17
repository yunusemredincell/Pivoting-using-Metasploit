# **Sanal Makineler ve Metasploit Framework kullanarak Pivoting**
### Pivoting, ortak bir ağda olduğumuz makineyi kullanarak, o makinenin bağlı olduğu bir diğer ağ varsa o ağı da ortak kullanan makineyi hackleme işlemidir. Şekilde görüldüğü üzere sol tarafta saldırgan makine, ortada iki ayrı ağa bağlı olan diğer makine, sağda ise hedefteki makine bulunmaktadır. (görsel1.png)
## Kendi Laboratuvar ortamınızı oluşturma
### Pivoting işlemini uygulayabilmek için 3 ayrı sanal makine kullanmak gerekmektedir. Bu ortamı hazırlarken ben VMware Workstation Pro 17 kullandım ve anlatımlarım VMware üzerinden olacak, ancak tüm işlemler Oracle VirtualBox ve diğer sanal makine ortamları için de geçerlidir. 
### ** Kurulumlar**
### vmware.com üzerinden kurumları tamamladıktan sonra Help -> Enter a Licence Key adımlarını takip ederek internetten bulduğunuz güncel lisans anahtarını kullanarak kullanıma başlayabilirsiniz. Ardından Kali Linux ve Metasploitable2 sanal makinelerinin kurulumları tamamlanır. 
