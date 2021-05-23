# Mala anlatır gibi: MySQL
> *Selam dostum bu arşiv içinde NodeJS kullanırken MySQL veritabanını güçlendirmeyi ve temel yapıları öğreneceğiz. Metin bazlı olduğu için dilersen ekran görüntüsü alıp boş olduğun zamanlarda okuyabilirsin. Şimdiden bol şans dilerim. (Arşiv içerisinde bazı bölümler NSFW içeriklidir.)*

![MySQL](https://cdn.discordapp.com/attachments/788489815213211698/845772385176256522/MySQL.png)

### Neden MongoDB varken MySQL veya SQL?
- Bu soruya verilebilecek fazla cevap yok. NodeJS projeleri için genel olarak en iyi veritabanı MongoDB'dir. Bunun bir kaç sebebi mevcut. Birincisi MySQL lokal veritabanıdır. MongoDB'ye istenilen her yerden ulaşılabilir. (Buna Cloud diyoruz. MySQL Veritabanlarına uzaktan bağlantı kurabilirsiniz fakat bunun maliyeti olur.) MongoDB'de daha fazla anlaşılır olan `schema` yani şema sistemi mevcut. MySQL'da şemalar yerine `table` yani tablo dediğimiz veri tutma birimleri bulunur. MySQL, MongoDB'deki gibi dizi verisi tutamaz (`Array`) bundan dolayı MongoDB'ye göre en azından **başlangıçta** daha zor gelir. 

> Bu arşivde NodeJS üzerinden [MySQL2](https://npmjs.com/package/mysql2) kullanan Discord Bot'u yapacağız. Kafa karışıklılığını önlemek için iki adet modülün mevcut olduğunu söylemeliyim. Bunlardan birincisi Legacy yani eski olan MySQL modülü diğeri yenilenmiş MySQL2 modülü. 

## Site üzerinden hesap açmaya çalışan yufka yürekli Rişad.
> MySQL kullanabilmek için herhangi bir internet hesabı açmak zorunda değilsiniz. `MySQL Community Server` adı verilen sunucuyu kurmanız yeterli olacak. 

- https://dev.mysql.com/downloads/file/?id=505213

Kurulumda dikkat etmeniz gereken fazla bir şey yok. Kurulumda sadece Community Server'i kurmanız gerekiyor. Bilgisayarınızı `developer` olarak gösterin ve sadece `root` şifresi ayarlayın. Başka bir kullanıcı açmayın.

## MySQL konsolunda gezerken eski kaybolan sevgimle karşılaşmak.
MySQL'ın kurulumu bittiğinde kendisine özel olan konsolu bilgisayarınıza kurar. Bu konsolu açmak için arama yerine `MySQL Command Prompt` kelimelerini girmeniz yeterli olacak. Konsol önünüze geldiğinde sizden giriş bilgilerinizi soracak. (Kurulum yaparken yazdığınız şifre neyse o.) Normal konsoldan giriş yapmak istersen de `mysql -u root -p` yazman yeterli. 
 
![konsolda gezerken olur gibi.](https://cdn.discordapp.com/attachments/722897282965700643/845967120307191808/Screen_Shot_2021-05-23_at_12.10.44.png)

Şifrenizi girdikten sonra başarılı giriş yaparsanız aşağıdaki ekran gelecektir.

![konsol](https://cdn.discordapp.com/attachments/722897282965700643/845967517406199828/Screen_Shot_2021-05-23_at_12.12.41.png)

Heyecanlanmanıza hiç gerek yok. Ağlayarak komut yazacağız. Başlamadan önce unutmamanız gereken bir husus var, MySQL Konsol'unda komut girerken yazdığınız `querry`'lerin sonunda `;` kullanmak zorundasınız. Kullanmazsak ne olur diye soranlar için MySQL Komut penceresi bir alta kayar ve ikinci satırda komutunuzu girmeniz için size olanak sağlar. Yani sanki VSC'de kod düzenliyor edasıyla alt alta `query` girişi yapabilirsiniz. 

![mal](https://cdn.discordapp.com/attachments/722897282965700643/845968172163006474/Screen_Shot_2021-05-23_at_12.15.17.png)

Eğer ilk satırda `;` kullanılsaydı aşağıdaki satırlara geçmeden işlemi çalıştırırdı. Eğer herhangi bir yanlışlık yaptıysanız `ALT + C` kombinasyonu ile mevcut girişi sıfırlayabilirsiniz.

NodeJS'de bağlantı gerçekleştirmeden önce `database` oluşturmak zorundasınız. Bunun için şu `query` kullanılır:

```
CREATE DATABASE SanalMafya;

/* Veya */

create database SanalMafya;

/* Veya */

create database Sanal_Mafya;

/* Veya */

CREATE DATABASE SönölMöfyö;

/* Bu şekilde yorum satırı girebileceğinizi de unutumayın he. */
```
Aman Allah'ım! İlk MySQL `query`'ini girdiniz. Hadi çıkan sonuçlara bakalım.

![oman tonrem](https://cdn.discordapp.com/attachments/722897282965700643/845969572775591936/Screen_Shot_2021-05-23_at_12.19.59.png)

İlk satırda `query`'i yazdık. Altta da bir adet yerin değiştirildiğini ve bunun `0.59 saniye`'de yapıldığını görüyorsunuz. Bu yolunu kaybeden Maymun'u mutlu etmek için bütün `database` isimlerini sıralatalım.

![şov debe](https://cdn.discordapp.com/attachments/722897282965700643/845970015316213820/Screen_Shot_2021-05-23_at_12.22.40.png)

Üstteki `query` sayesinde bulunan bütün `database`'leri görebilirsiniz. Sizde çıkan sonuçlar daha düşük olabilir. 

### Yanlışlar doğruları ne zamandır götürür? O'nu tekrar görebilecek miyim?
Yufka yürekli Rişad'ım sen üzülme. Herkes hata yapar yahu, asıl sorunumuz bu hatayı nasıl düzelteceğimiz olmalı. MySQL bir yapboz gibidir. Bütün `query`'ler neredeyse birbiriyle kullanılabilir. Hatalar bizi güçlendirir diyerek size uzak akrabam olan `DROP` ile tanıştırmalıyım. `DROP` neredeyse her yerde kullanılabilir. Oluşturduğunuz `database`'yi komple ortadan kaldırır veya `table`'yi siler. 

![silerken Çek knk](https://cdn.discordapp.com/attachments/722897282965700643/845972246316449822/Screen_Shot_2021-05-23_at_12.31.29.png)

Artık Maymun sonsuza dek kayıp.

## Oluşturdum da bedeli ne oldu?
> Herhangi bir `database` oluşturduktan sonra içinde işlem yapabilmek için `database` seçimi yapmak zorundasınız. Bundan dolayı `USE`'yi kullanırız. 

![db Seçerken olur gibi.](https://cdn.discordapp.com/attachments/722897282965700643/845972678594920468/Screen_Shot_2021-05-23_at_12.33.16.png)

Hadi ilk tablomuzu oluşturalım. Tablo oluşturmadan önce aşağıdaki örneğe göz atın.

```
CREATE TABLE (
  numara INT PRIMARY KEY AUTO_INCREMENT NOT NULL,
  isim VARCHAR(20) CHARACTER SET utf8 NOT NULL,
  soyisim VARCHAR(20) CHARACTER SET utf8 NOT NULL,
  yaş INT,
  açıklama TEXT CHARACTER SET utf8,
  zaman DATETIME DEFAULT (CURRENT_TIME) 
);
```

Hadi en baştan başlayalım. Dedik ki `CREATE` yani oluştur daha sonrasında ne oluşturmak istediğimizi yazdık. İki adet parantez açıp sonuna da `;` ekledik. Sıra içini doldurmaya geldi hehe. İlk sırada mevcut olan `numara` sırası için dedik ki: sen o tablonun öncelikli anahtarısın (bu şimdilik öğrenmeye değer değil.) ve senin taşıyacağın verinin türü sayı türü olacak (sayı türleri üzerinde ekleme/çıkarma yapılabilir.) özel bir belirteç olan `AUTO_INCREMENT` ise her veri girişi olduğu zaman senin üzerine `+1` sayı ekle. 

**Anlamadım moruk ne verisi ne arttırması?:** Burayı iyi dinle, Auto Increment yani otomatik artış belirteci her veri girildiği zaman otomatik olarak sıra numarası belirler gibi düşün. Bunu varsayılan olarak bırakırsan her veri arttığı zaman +1 ekler. Bu da sana neyi sağlar?

```
1- Melisa ARDUÇ
2- Feyza TEN
3- Ali VARSUVAR
```

Üstte de görüldüğü gibi otomatik sıra oluşturmanı sağlar. 
