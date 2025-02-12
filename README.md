# Dikkat Ömemli !!
İndirilecek çalışma ekleri ve jar dosyası releases altındadır..

# Borsa-verileri
Merhaba arkadaşlar, geliştirmiş olduğum jar dosyası ile browser üzerinden tüm borsa verilerini alablirsiniz. coin, nasdaq, borsaistanbul vs.. 
çok ayrıntılı olmasada temel olarak işe yarayacak şekilde. 
Veriler borsa istanbul için 15 dakika gecikmelidir.
Hadi başlayalım, öncelikle jar dosyamızı çalıştırmk için bazı kütüphaneleri yüklememiz lazım. 
Buradaki can alıcı nokta tüm işletimlerde java yüklü olduğu sürece çalışır. 
## Install java (jdk)

Linux tabanlı işletim sistemini baz alarak anlatıyorum.
Android işletimde de olur. ternux yüklü olması yeterli.

``` {.sourceCode .bash}
pkg update && pkg upgrade -y

```
Dosyaları güncelledik. Sıra jdk yüklemede.

Önce yüklü olup olmadığını kontrol edelim..
``` {.sourceCode .bash}
pkg search openjdk
```
Sonra jdk yoksa yükleyelim..
``` {.sourceCode .bash}
pkg install openjdk-21 -y

```
Java'yı kontrol edelim..

``` {.sourceCode .bash}
java -version
```
Buraya kadar java dosyalarını yani jdk yükledik. Şimdi sıra jar dosyamızı çalıştırmaya.
## UnInstall java (jdk)
Android cihazınıza kurduğunuz jdk bazen performans sorunu yapabilir kaldırmak isterseniz terminalde bu kodu kulanınız.

Yüklü olup olmadığını kontrol edelim..
``` {.sourceCode .bash}
pkg search openjdk
```
Aradıktan sonra kaldırmak için.

``` {.sourceCode .bash}
pkg uninstall openjdk-21 -y
```
Silindiğine emin olmak için Şu kodu kullanıyoruz. Hata almış isek kaldırılmıştır.

``` {.sourceCode .bash}
java -version
```


## Jar klasörüne gitme ve çalıştırma
anlatacaklarım root dizininde olduğunuzu varsayarak yapıyorum.
Android'de de sdcard dizini.
Android'de
``` {.sourceCode .bash}
cd /sdcard
cd jar
```
linux da

``` {.sourceCode .bash}
cd /jar
```
Dilerseniz sayfa sonunda start stop yöntemi ile istersenizde bir satır altta bulanan kodla çalıştırabilirsiniz.

``` {.sourceCode .bash}
#!/bin/bash
java -cp "App-1.0.jar:libs/*" com.api.htfinance.Main 8484
```

Dikkat ettiy seniz 8484 gibi rakamlar var bu çalışacağı port numarasıdır dilediğimiz gibi kullanabilirsiniz.
standart port 8090'dır

Gelelim Browserde çalıştırmaya.
## Browserde Aç


``` {.sourceCode .bash}
http://localhost:8484/price?ticker=THYAO.IS
```
Türk borsalarında tinker yani senbol isme .IS eki koyunuz, diğer türlü hata çıktısı alırsınız.
Abd ve cripto borsalarında senbol ismi yeterli. 

Herşey düzgün gittiğinde ekrana şu şekilde bir çıktı alacaksınız.
``` {.sourceCode .bash}
{
  "symbol": "THYAO.IS",
  "sirketismi": "Türk Hava Yollari Anonim Ortakligi",
  "sirketkisaismi": "TURK HAVA YOLLARI",
  "ilkislemtarihi": "10.05.2000",
  "sonislemtarihi": "12.02.2025 / 15:17:47",
  "yillikendusukfiyat": 257.5,
  "yillikenyuksekfiyat": 332,
  "guncelfiyat": 320,
  "gunicienyuksekfiyat": 321.25,
  "guniciendusukfiyat": 316,
  "dunkapanisfiyati": 319.5,
  "islemgorenlotsayisi": "24.515.132"
}
```

buraya kadar geldiğinize göre herşey yolundadır Esenlikle kalın.

## Çalışan Jar dosyasını durdurma

``` {.sourceCode .bash}
#!/bin/bash

JAR_NAME="com.api.htfinance.Main"  # Buraya durdurmak istediğin JAR dosyasının adını yaz

# JAR dosyasını çalıştıran işlemin PID'sini bul
PID=$(jps -l | grep "$JAR_NAME" | awk '{print $1}')

if [ -z "$PID" ]; then
    echo "Çalışan $JAR_NAME bulunamadı."
else
    echo "$JAR_NAME çalışıyor (PID: $PID). Durduruluyor..."
    kill "$PID"
    echo "Durduruldu."
fi

```
## Start ve stop 
Daha pratik olması için .sh uzantılı dosyalarınıda ekledim.
start.sh 
``` {.sourceCode .bash}
bash start.sh
```
durdurmak içinse stop.sh kullanıyoruz..
``` {.sourceCode .bash}
bash stop.sh
```
