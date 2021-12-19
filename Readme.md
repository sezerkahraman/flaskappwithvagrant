# Kullanılan Yazılımlar Ve Teknolojiler
- Flask
- Vagrant
- Ansible
- Nginx
- Gunicorn


### Vagrant Nedir?
Vagrant sanal makina ayarlarını oldukça kolaylaştıran bir yazılımdır.Oluşturacağımız Vagrantfile isimli tek bir dosya ile tüm takımın aynı ortamda aynı sürüm işletim sistemi ve aynı versiyon araçlarla çalışmasını sağlayabiliriz.Ben kısaca kendi Vagrantfile dosyam hakkında bilgi veriyim.

## Vagrantfile Dosya İçeriği:
1.Vagrant_Ip --> Oluşrulucak vm'e verilmek istenen ip adresini tanımladım.
```
 VAGRANT_IP="10.0.0.9"
 ```
 2.Vagrant.Configure --> Burada vagrantın hangi versiyonunun kullanılacağını belirttim ve her seferinde vagrant.configure yazıp fonksiyonu çağırmak yerine bir kısaltma tanımladım.
 ```
 Vagrant.configure("2") do |config|
 ```
 3.config.vm.box --> Vagrant hızlı bir şekilde sanal makina oluşturmak için [base box](https://app.vagrantup.com/boxes/search) dediğimiz base imagelar kullanılır.Hızlı bir şekilde ihtiyacım doğrultusunda Ubuntu Server 14.04 LTS işletim sistemine sahip bir sanal makina ayağa kaldırmak için ubuntu/trusty64 base box'ını kullandım.
 ```
 config.vm.box= "ubuntu/trusty64"
 ```
 4.config.vm.network --> Burada network tipini tanımladım.Daha öncesinde makinaya vermek istediğim IP'yi değişken olarak tanımlamıştım burda tanımladığım değişkeni çağırdım.
 ```
 config.vm.network :private_network, ip: VAGRANT_IP
 ```
5.config.vm.provision --> Provisioners(yapılandırma araçları) yüklenmiş olan sanal makinada tanımladığımız taskları [chef](https://www.tutorialspoint.com/chef/chef_overview.htm),[puppet](https://puppet.com/docs/),[saltstack](https://docs.saltproject.io/en/latest/),[fabric](https://fabricmc.net/wiki/doku.php),[ansible](https://docs.ansible.com/) çalıştıran araçlardır.Ben bu projede provisioners olarak [ansible](https://docs.ansible.com/) kullandım ve bunuda Vagrantfile içinde belirtmemiz gerekiyor.
```
 config.vm.provision "ansible_local" do |ansible|
 ```
 6.ansible.playbook --> Playbook dosyasının path bilgisini belirttim.
```
 ansible.playbook = "provisioners/deployment.yml"
 ```
7.config.vm.provision --> Ansible host dosyanın içine localhost'u ekledim ve bunuda dev/null içine yönlendirdim.
```
 config.vm.provision "shell", inline: "printf 'localhost\n' | sudo tee /etc/ansible/hosts > /dev/null"
 ```
 8.config.vm.provider --> Vagrant bizlere herhangi bir sanallaştırma işlemi sağlamıyor bunun yerine sanallaştırma hizmeti sunan teknolojilerden yararlanıyor.Bu işlemi sunan teknolojiler [Providers](https://www.vagrantup.com/docs/providers) olarak isimlendirilir.Vagrant için default provider ise [Virtual Box](https://www.virtualbox.org/).Eğer farklı bir sanallaştırma teknolojisi kullanmak istersenizde burada belirtebilirsiniz.
 ```
 config.vm.provider "virtaulbox" do |vb|
 ```

9.vb.memory --> Bu kısımda ihtiyaca göre düzenlenebilir benim 2GB ram'e ihtiyacım vardı ve bunu belirttim.
```
vb.memory = "2048"
 ```
 # Kur Ve Çalıştır

 1.Bu repoyu localine çek.
 ```
git clone https://github.com/sezerkahraman/flaskappwithvagrant.git
 ```
2.Daha sonra cd komutu ile dosyanın bulunduğu dizine git ardından vagrant ortamını ayağa kaldır.
```
vagrant up
 ```
3.Sonrasında oluşan sanal makinanın içerisine vagrant ssh komutu ile bağlan.Ardından şöyle bir karşılama mesajı göreceksiniz.
```
vagrant ssh
 ```
 ```
Welcome to Ubuntu 14.04.5 LTS (GNU/Linux 3.13.0-139-generic x86_64)
.
.
.
.
```
4.Son olarak uygulamaya erişmek için tarayıcınızı açıp [http://10.0.0.9](http://10.0.0.9) adresine gittiğinde karşınızda Hello World yazısını göreceksiniz.

