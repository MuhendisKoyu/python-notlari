- [Python'da Sürüm Yönetimi](#pythonda-sürüm-yönetimi)
  - [`pyenv` Kurulumu](#pyenv-kurulumu)
  - [pyenv Bağımlılıkların Kurulumu](#pyenv-bağımlılıkların-kurulumu)
  - [Yeni Sürümlerin Kurulumu](#yeni-sürümlerin-kurulumu)
  - [Yüklenen Sürümün Kullanımı](#yüklenen-sürümün-kullanımı)
  - [Farklı Sürümlerin Bir Arada Kullanımı](#farklı-sürümlerin-bir-arada-kullanımı)

<span style="font-size:0.8em;">**Not**: Windows üzerinde `pyenv` aracını deneyimlemediğim için anlattığım kurulum aşamaları Windows sistemleri kapsamamaktadır. Eğer deneyimlemek isterseniz [pyenv-win](https://github.com/pyenv-win/pyenv-win) linkini takip edebilirsiniz.</span>

# Python'da Sürüm Yönetimi

## `pyenv` Kurulumu

Projeleriniz için bir veya birden fazla Python sürümü kullanmak istiyorsunuz fakat yeni sürüm kurulumlarında paket deposunda kurulum, root hakları, kaynaktan sürüm derlemeleri, sürümler arası geçiş gibi işlerle uğraşmak istemiyorsanız `pyenv` ile hızlı, pratik ve güvenli bir şekilde gerçekleştirebilirsiniz. `pyenv` sisteminizde birden fazla Python sürümünü kullanabilmenizi ve birçok kolaylığı da beraberinde getiren bir araçtır. Bunun için öncelikle `pyenv-installer` ile sistemimize `pyenv` kurmamız gerekmektedir. Kurulum için aşağıdaki komutu çalıştırmanız yeterlidir

```
$ curl https://pyenv.run | bash 
```

Kurulum tamamlandıktan sonra aşağıdaki şekilde bir mesaj çıkacaktır

```
    # Load pyenv automatically by adding
    # the following to ~/.bashrc:

    export PATH="$HOME/.pyenv/bin:$PATH"
    eval "$(pyenv init -)"
    eval "$(pyenv virtualenv-init -)"
``` 

Buradaki son 3 satırı ana dizininizdeki `.bashrc` dosyanızın *(veya farklı bir shell kullanıyorsanız ilgili dosyaya)* eklemeniz gerekmektedir. Eklemeyi yapıp kaydettikten sonra `pyenv`'i kullanmaya başlamak için terminali yeniden başlatmanız veya terminale `source ~/.bashrc` yazmanız yeterlidir.

`pyenv` ile yükleyebileceğiniz Python sürümlerini listeleyebilmek için tek yapmanız gereken şu komutu çalıştırmaktır:

```
$ pyenv install --list
```
Yalnızca 3.8.x sürümlerini listelemek için aşağıdaki komutu çalıştırabilirsiniz:

```
$ pyenv install --list |grep " 3.8.*"
```

Eğer bütün sürümleri listelediyseniz görebileceğiniz üzere, `pyenv` ile kurabileceğiniz Python sürümü sayısı oldukça fazladır. Bu sürümlerden herhangi bir tanesini kurmadan önce `pyenv`'in kurulumları başarıyla tamamlayabilmesi için ihtiyaç duyduğu bağımlılıkları kurmanız gerekmektedir. Aksi takdirde kurulumu hata ile sonuçlanabilir.

## pyenv Bağımlılıkların Kurulumu

Bağımlılıkların kurulumu için kullandığınız sistem aşağıda yoksa linki inceleyebilirsiniz: [pyenv | Sık Karşılaşılan Hatalar](https://github.com/pyenv/pyenv/wiki/Common-build-problems)

- Ubuntu/Debian
```
$ sudo apt-get install -y build-essential libssl-dev zlib1g-dev libbz2-dev \
libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \
xz-utils tk-dev libffi-dev liblzma-dev python-openssl git
```

- Fedora/CentOS/RHEL
```
$ sudo dnf install zlib-devel bzip2 bzip2-devel readline-devel sqlite \
sqlite-devel openssl-devel xz xz-devel libffi-devel findutils
```

- Arch ve türevleri

```
$ pacman -S --needed base-devel openssl zlib bzip2 readline sqlite curl \
llvm ncurses xz tk libffi python-pyopenssl git
```

## Yeni Sürümlerin Kurulumu

`pyenv` için gerekli ayarları yaptıktan sonra yeni bir Python sürümü kurabilmek için tek yapmanız gereken `pyenv install <python-sürümü>` komutunu çalıştırmaktır. `<python-sürümü>` kısmına yazağımız sürüm bilgisi `pyenv install --list` sonuçları ile aynı isimde olmalıdır. Örneğin `Python 3.8.5` sürümünü kurmak için komutu aşağıdaki gibi çalıştırmalısınız:

```
$ pyenv install 3.8.5
```

Bu komut ile `3.8.5` sürümü sisteminize yüklenecektir.

## Yüklenen Sürümün Kullanımı 

Yüklediğiniz sürümün doğrulamasını yapmak için `pyenv versions` komutunu çalıştırabilirsiniz. Eğer yükleme uygun bir şekilde tamamlandı ise komutu çalıştırdığınızda görmeniz gereken çıktı şu şekildedir:

```
    * system
    3.8.5
```

`*` işareti hali hazırda aktif olan sürümü nitelemektedir. Yeni kurduğunuz sürümü sistemin genelinde aktif etmek için çalıştırmanız gereken komut şudur:

```
$ pyenv global 3.8.5
```

## Farklı Sürümlerin Bir Arada Kullanımı

2020 yılı itibariyle python2 desteği bitmiş olsa da eski projeleri denemek istiyor olabilir veya kullanağınız kütüphaneler mevcut python sürümünüzde çalışmıyor olabilir. Bu veya benzeri sebeplerle sisteminizde birden fazla sürümü aynı anda bulundurmayı tercih edebilirsiniz.

Kurulum adımlarını birebir takip ettiyseniz sisteminizde 3.8.5 sürümü aktiftir. Buradan devam ederek sisteme 2 yeni Python sürümü daha kuralım::

```
$ pyenv install 2.7.18
$ pyenv install 3.6.9
```
Bu durumda ``pyenv versions`` komutunun çıktısı şu şekilde olacaktır::

```
$ pyenv versions
  system
  2.7.18
  3.6.9
* 3.8.5 (set by /home/<kullanıcı-adınız>/.pyenv/version)
```

`pyenv` ile kurulan sürümlerin kullanılabilmesi için 3 farklı komut bulunmaktadır. 

- `pyenv local`

```
$ pyenv local <python-sürümü>
```

`local` komutu ile sürüm seçtiğinizde, bulunduğunuz dizinde `.python-version` isimli bir dosya oluşur. Bu dosya içerisinde yalnızca seçtiğiniz Python sürümü numarası bulunur ve bu sürüm yalnızca mevcut dizin içerisinde aktif olur. Aşağıdaki şekilde bir klasör yapısında `proje1` dizininde `pyenv local 3.6.9` komutunu çalıştırdığımızı düşünelim:

```
    projeler
    ├── proje1
    │   └── .python-version # 3.6.9
    └── proje2
```

Bu durumda `proje1` dizininde `python` komutu `3.6.9` sürümü, bunun dışındaki bütün dizinlerde `3.8.5` sürümü çalışacaktır.

- `pyenv global`

```
$ pyenv global <python-sürümü> 
```

Kurulum kısmında da bahsettiğimiz gibi aktif kullandığınız Python sürümünü `global` komutu ile sistem genelinde etkinleştirerek kullanabilirsiniz. Normalde python2 yüklü sistemlerde `python` komutu python2 sürümünü çalıştırırken, `global` komutu ile aktifleştirme sonrasında `python` yazdığınızda seçtiğiniz Python sürümü çalışacaktır.

```
$ pyenv global 3.8.5
```

- `pyenv shell`

```
$ pyenv shell <python-sürümü>
```

`shell` komutu ile aktifleştirdiğiniz Python sürümü mevcut terminal oturumunuz süresince aktif olacaktır. `shell` komutu `local` ve `global` sürümleri baskılar. Yani `local` olarak ayarladığınız bir dizine girdiğinizde bile hala `shell` ile aktifleştirdiğiniz sürüm çalışır. Terminali yeniden başlatmadan normal şekilde kullanmaya devam edebilmek için `pyenv shell --unset` komutunu çalıştırmalısınız.

Yeni Python sürümleri çıktıkça `pyenv`'e eklenmektedir. Bunun için öncelikle `pyenv update` komutu ile `pyenv` aracını güncellemelisiniz.