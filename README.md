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

Üstte de görüldüğü gibi otomatik sıra oluşturmanı sağlar. Bununla ne yaparım diye düşünenler için yaptığınız ceza sisteminde çok güzel ceza numarası olarak kullanabilirsiniz.

İkinci satırda yer alan isim verisi için de dedik ki sen yirmi haneli `string` taşıyabilirsin. Bu ne demek oluyor? Yirmi karakter ve yirmi karakterin altındaki `string` verileri taşıyabilirsiniz. 

```
Aslı (dört hane.)
Melih (beş hane.)
Abdülrezzak Kıllıbacak (yirmi bir hane.)
```

Üstte verilen verilerden sadece `Aslı` ve `Melih` verisi girilebilir demek oluyor. Yirmi bir haneli veri girişi yapmaya çalışırsanız hata alırsınız. Yirmi bir verilik yer oluşturmak için `VARCHAR(21)` yapmanız yeterli. 

**Character set diye bir şey var bak bana yan yan bakıyor?:** Normal `VARCHAR` birimleri ve diğer bütün `string` verileri sadece İngilizce Latin harfleri ve birimleri desteklediğinden dolayı içerisinde kullanılabilecek karakterleri arttırmak için `CHARACTER SET utf8` diyoruz. `VARCHAR`'ın özel bir durumu var. Dilerseniz böyle uzatabilirsiniz `VARCHAR(18) CHARACTER SET utf8` ya da `NVARCHAR(18)` kullanırsınız. İkisi de aynı sonuca çıkar.

**Not Null diye bir şey var bana el sallıyor?:** `not null` seçili sıranın asla boş bırakılamayacağını söyler. Yani boş bırakmaya çalıştığınızda (`null/nil`) size hata vererek tabloya girişinize izin vermez. Boş bırakılabilecek (keyfekeder veri ekliyorsanız) `not null` kullanmamanız gerekir.

Dördüncü sırada yer alan yaş tablosuna gelecek olursak o tablonun sadece sayı taşıyabileceğini söylemiştik. Diğerlerinden farklı olarak o sıra boş bırakılabilir. 

Açıklama verisine gelecek olursak yeni bir ağabey ile sizi tanıştırmak istiyorum. Kendisine `TEXT` diyoruz. Aşağıda bir sıralama yapacak olursak:

```
TINYTEXT = VARCHAR(255) [En fazla 255 Byte yer kaplar.]
TEXT = VARCHAR(65535) [En fazla 64 KB yer kaplar.]
MEDIUMTEXT = VARCHAR(16777215) [En fazla 16 MB yer kaplar.]
LONGTEXT = VARCHAR(4294967295) [En fazla 4 GB yer kaplar.]
```

Kısacası `TEXT` 65 bin karaktere kadar destekler. `VARCHAR` ile uğraşmanız yerine sizi kurtarır. Fakat neden yine de `VARCHAR` kullanılıyor diye sorarsanız kesin olan birimlerde `VARCHAR` kullanımı sağlıklıdır. Örnek vermek gerekirse T.C. Numaranızın alabileceği maksimum ve minimum hane değeri bellidir. Yanlış bir değer girildiğinde sizi tablo uyarır. `TEXT` biriminin desteği için `CHARACTER SET utf8`'i burada da kullandık.

Zaman kavramı MySQL'in desteklediği harika bazı içerikleri sizlere sunar. `DATETIME` size zaman verisi tutma olacağı verir. Bu verilerin `TIMESTAMP` olmadığını unutmayın. Bu tabloda biz dedik ki, sen bir `DATETIME` türüsün. Bu türün alabileceği `DEFAULT` yani hiçbir veri girilmezse `NULL`'u tercih etmek yerine yerleştireceğin veri şu anın zamanı olsun dedik. Evet, her veri girişinde otomatik olarak zamanı seçecek. 

### Feridun abi, yine seks hikayesi mi yazıyosun?
> MySQL `DEFAULT` birimlere selam verin! Neredeyse her veride `DEFAULT` belirleyebilirsiniz. Örnek vermek gerekirse:

```
item_name VARCHAR(50) NOT NULL,
item_count INT DEFAULT 0
item_durability DEFAULT 100
item_lore DEFAULT "Şamda Kayısı"
```

Üstte üç adet `DEFAULT` birim verdik. Hadi yorumlayalım fakat ilk başta `DEFAULT` tam olarak ne yapar ondan söz edelim. `DEFAULT` sayesinde seçili sıraya veri girişi yapmazsanız otomatik olarak `DEFAULT` olarak seçili veriyi alır. Yani siz bu üstteki örnekte şu yerleri doldurdunuz diyelim: `item_name`'yi "Anka Kuş'u", `item_count`'u da bir olarak belirlediniz. İki yeri boş bıraktınız ama o birimlerin `DEFAULT` değerleri mevcut. Bundan dolayı veriler sırasıyla şöyle olacak:

```
item_name | item_count | item_durability | item_lore

Anka Kuşu | 1          | 100             | Şamda Kayısı
```

Oluşturduğumuz tablolara göz atmak için `SHOW TABLES` `query`'ini girebilirsiniz.

## Müzmin yedek burada! Verileri havuç olarak kayıt etmek.
> Nasıl veri tablosu oluşturacağımızı yazdık. Sırada verileri tutmada. MySQL'da `INSERT INTO` adı verilen yeni giriş suretiyle veri ekleme biçimi mevcut. (Yani `INSERT INTO` durması belirtilmezse sonsuza dek veri ekleyebilir. Mevcut verinin üzerine yazmaz.) İlk başta nasıl veri tutacağız kısa bir şekilde ona bakalım. Tablomuz aşağıdaki gibi olsun.

```
CREATE TABLE aptallar (
  isim VARCHAR(20) NOT NULL,
  ikinci_isim VARCHAR(20),
  soyisim VARCHAR(20) NOT NULL,
  aptallik_seviyesi INT DEFAULT 0
  bu_kisi_vurduruyor_mu BOOLEAN DEFAULT true
);
```

Bu tabloya veri kayıdını şöyle yapabiliriz: 

```
INSERT INTO aptallar (isim, soyisim, aptallik_seviyesi) VALUES ('Talha','KAVAKLI',100);
```

Sonuç:

```
isim  | ikinci_isim  | soyisim  | aptallik_seviyesi  | bu_kisi_vurduruyor_mu

Talha | (NULL)       | KAVAKLI  | 100                | true
```

Hadi olan bitene göz atalım sevgili Müzmin arkadaşım. `INSERT INTO` neydi? `INSERT INTO` sevgiydi, kardeşikti. İlk `query`'imiz `INSERT INTO` oldu. Daha sonrasında `aptallar` olarak yazdığımız tabloya verdiğimiz genel isimi belirtiyor. Daha sonrasında hangi verileri eklemek istiyorsak o verileri yazıyoruz. Tabloda toplam beş adet veri olabilir. Biz kayıt yaparken sadece üç veriyi eklemek istedik. Bunları sırasıyla virgül ile yazdık ve `VALUES` `query`'ini koyduk. Daha sonra da verileri nasıl istiyorsak öyle girdik. Burada `ikinci_isim` olmadığı için ve o veri tipine `NOT NULL` yazmadığımız için `(NULL)` olarak giriş yaptı. `BOOLEAN` veri tipi ise dört tip veriyi taşıyabilir bunlar: `true`, `false`, `0`, `1`. Burada `DEFAULT` olarak `true (1)` ayarlaması yaptık. 

### Baldız baldan tatlıdır: TRUNCATE.
> Araya girmek gerekirse size yeni baldızımız olan `TRUNCATE`'yi tanıtmak istiyorum. `TRUNCATE` kimsenin sevmediği ama yerini kimsenin tutamadığı baldızımız gibi. Ne demişler baldız baldan tatlıdır. 

`TRUNCATE` en kısa anlatımla bir tablonun içindeki bütün verileri boşaltmanıza yarar. Hadi test edelim!

Şu anki verilerimiz: 

```
isim  | ikinci_isim  | soyisim  | aptallik_seviyesi  | bu_kisi_vurduruyor_mu

Talha | (NULL)       | KAVAKLI  | 100                | true
Mert  | (NULL)       | YILMAZ   | -100               | false
```

Bu tablonun ismini hatırlayalım `aptallar`. Gireceğimiz `query` ise `TRUNCATE TABLE aptallar`. Bunun sonucu aşağıda yer alıyor:

```
isim  | ikinci_isim  | soyisim  | aptallik_seviyesi  | bu_kisi_vurduruyor_mu
```

Tablo tamamen boşaldı fakat tablodaki veri türleri hala duruyor. Bu test yaptıktan sonra eski haline getirmek için muhteşem bir çözümdür. 

#### Uzman Sude yanıtlıyor: `TRUNCATE` işlemi tablodaki her şeyi sıfırladığı gibi `AUTO_INCREMENT` verisini de sıfırlar. Bu da ne ola ki?

```
/* Buradaki bilgiler ekstra bilgidir. Öğrenmek zorunda değilsin. */
/* AUTO_INCREMENT neydi? Her veri girişinde kendisini arttıran bir sayı tablosuydu değil mi? 
Aşağıdaki gibi bir örnek verebiliyorduk hatırladıysan: */

pid |  isim  | ikinci_isim  | soyisim  | aptallik_seviyesi  | bu_kisi_vurduruyor_mu

1   |  Talha | (NULL)       | KAVAKLI  | 100                | true
2   |  Mert  | (NULL)       | YILMAZ   | -100               | false

/* Burada biz 2. PID değerini sildik diyelim: */

pid |  isim  | ikinci_isim  | soyisim  | aptallik_seviyesi  | bu_kisi_vurduruyor_mu

1   |  Talha | (NULL)       | KAVAKLI  | 100                | true

/* Sonuç ne oldu? Üstteki gibi tek bir veri kaldı değil mi? Hadi şimdi bir veri ekleyelim: */

pid |  isim  | ikinci_isim  | soyisim  | aptallik_seviyesi  | bu_kisi_vurduruyor_mu

1   |  Talha | (NULL)       | KAVAKLI  | 100                | true
3   |  Miaf  | (NULl)       | (NULL)   | -100               | false

/* O da ne? Biz ikinci veriyi silmiştik? Ne yani AUTO_INCREMENT eski veriyi hatırlıyor ve 
üzerine kayıt yapmıyor mu? Evet öyle. Bu hafızayı sıfırlamak için de TRUNCATE kullanılabilir. */
```

## Harici Beddah gibi mi olalım? İki veri eklemek de neyin nesi?
> `INSERT INTO`'nun veri değiştirme yapamayacağını söylemiştik. Hadi bunu örneklerle pekiştirelim. Aşağıda bir tablo örneği var:

```
memurun_ismi_soyismi | memurun_bodrosu | memurun_pozisyonu | aylık_kesinti

Melih Şam            | 1000          | İnsan Kaynakları  | 50
Damla                | 4500          | CEO               | 130
```

Bu tabloya birinci sıradaki aynı girişi yaparsak ne olur? 

```
INSERT INTO memurlar (memurun_ismi_soyismi, memurun_bodrosu, memurun_pozisyonu, aylık_kesinti) VALUES ('Melih Şam', 1000, 'İnsan Kaynakları', 50)
```

Sonuç:

```
memurun_ismi_soyismi | memurun_bodrosu | memurun_pozisyonu | aylık_kesinti

Melih Şam            | 1000            | İnsan Kaynakları  | 50
Damla                | 4500            | CEO               | 130
Melih Şam            | 1000            | İnsan Kaynakları  | 50
```

Ağlak çocuğumuz Melih'in aylık aldığı maaşı düşürmemiz lazım. Tek başına bütün İnsan Kaynakları'nı sömürüyor köpek. Ne yapmalıyız?

```
UPDATE memurlar SET memurun_bodrosu = 500;
```

Sonucumuz ne olacak?

```
memurun_ismi_soyismi | memurun_bodrosu | memurun_pozisyonu | aylık_kesinti

Melih Şam            | 500             | İnsan Kaynakları  | 50
Damla                | 500             | CEO               | 130
```

Cafer bez getir! Bizim CEO'nun da maaşı 500'e düştü. Bizi öpmeden önce bu durumu düzeltmeliyiz ama ilk başta neden oldu bu?

### Karşınızda kanatsız melek: `WHERE`.
> Üstteki durumla karşılaşmanızın nedeni çok basit. Çünkü verinin değişmesini istedik ama kimin değişmesi gerektiğini istemedik. `WHERE` kullanmadığımız için herkesin verisi değişti. Hadi bunu düzeltelim:

```
UPDATE memurlar SET memurun_bodrosu = 500 WHERE memurun_ismi_soyismi = 'Melih Şam';
```

Sonuç:

```
memurun_ismi_soyismi | memurun_bodrosu | memurun_pozisyonu | aylık_kesinti

Melih Şam            | 500             | İnsan Kaynakları  | 50
Damla                | 4500            | CEO               | 130
```

## Baltaya sap olamayanlar için değişim şansı: `DELETE`.
> Tabloyu tümden silme yolunu göstermiştik. Bu sefer tek bir sütunu nasıl silebileceğinizi göstereceğim. 

```
DELETE FROM memurlar WHERE memurun_ismi_soyismi = 'Melih Şam'
```

Üstteki `query`'i çalıştırdığımız zaman sonuç aşağıdaki gibi olacak:

```
memurun_ismi_soyismi | memurun_bodrosu | memurun_pozisyonu | aylık_kesinti

Damla                | 4500            | CEO               | 130
```

## Ne demek sevdiklerimizi seçmek? Biz yanlı mıyız!
> Hep birlikte oğlumuz `SELECT`'i kızımız `FROM`'a istiyoruz. Şampanya, patla! Bir veriyi nasıl çekeceğimizi öğrenelim. Ben `callback`'ları çok sevdiğim için MySQL2'de callback kullanıyorum. Kullandığımız tablo yukardaki memurlarla ilgili olan tablo olacak.

```
SELECT * FROM memurlar;
```

Basit bir soru olan `SELECT *` bütün her şeyi seçmek için kullanılır. Memur tablosunda bulunan bütün sonuçları bize verir. Sonucumuz şu olur:

```
memurun_ismi_soyismi | memurun_bodrosu | memurun_pozisyonu | aylık_kesinti

Damla                | 4500            | CEO               | 130
```

Peki spesifik verileri çekmek için ne yapabiliriz? Ya da sadece istenileni nasıl çekeriz? Bu da oldukça basit. Her şeyi seçmek için `*` kullanmamanız ve isteğinizi yazmanız yeterli.

```
SELECT aylık_kesinti FROM memurlar;
```

Sonucumuz:

```
aylık_kesinti

130
```

Daha da özel veri çekmek için ne yapıyoruz altan? `WHERE` kullanıyoruz müzmin! Hadi başka bir veritabanına geçelim. 


```
suclu_ismi | suc_orani | en_sonuncu_sucu

Aether     | 100       | Sanal mafya.
Ozzy       | 78        | Bot yapmak.
Shinoa     | 100       | Gay olmak.
Damla      | 35        | Harika biyografi yazmak.
Ada        | (NULL)    | (NULL)
```

Şimdi `SELECT * FROM suclular;` yaptığımızda çıkan sonuç üstteki sonuç. Bu sonuçları daraltmaya geçmeden önce size üstteki örneği pekiştirmek amacıyla `SELECT suclu_ismi FROM suclular` yapmak istiyorum.

```
suclu_ismi 

Aether
Ozzy
Shinoa
Damla
Ada
```

Bu sonuçları bir kişiye ait olmasını isterseniz? `WHERE` buradaydı değil mi? 

```
SELECT * FROM suclular WHERE suclu_ismi = 'Ozzy';
```

Çıkan sonuç: 

```
suclu_ismi | suc_orani | en_sonuncu_sucu

Ozzy       | 78        | Bot yapmak.
```

## Hoş gelmişsiniz efendim: MySQL kullanarak Discord Bot'u yapmak.
> İlk başta bir bağlantı sağlamalıyız. Bu bağlantı için MySQL2 modülünü kullanacağız. Aşağıda bağlantı için örnek verdim. (Chalk mevcut.)

```
const  con  = (global.con  =  mysql.createConnection({
host: localhost,
user: "root",
password: "1234",
database: "test_database",
}));

con.connect(err  => {
if (err) throw  err;
console.log(chalk.bgWhite.black(`[${moment().format('YYYY-MM-DD HH:mm:ss')}]`) +  chalk.cyan(` Successfully connected to MySQL database.`));
});
```

Başarılı bir şekilde bağlantı gerçekleştirilirse konsola pozitif mesaj atacak. Global hale getirdiğimiz bu komut sayesinde her yerde tanımlama yapmadan kullanabiliriz. (Methodları göstermez.)

## İyi güzel Faruk Ağabey, burada nasıl `query` gireceğiz?
> Bu sorunun cevabı oldukça basit. `con.query()` direk olarak `query` komutları girmenize yardım eder. Örnek vermek gerekirse, `con.query("CREATE DATABASE OlumNedirKi")` gibi. 

### Özel zamanlar için not alma vakti. Yaz kızım...
> Çok basit bir not alma komutu yaratmaya ne dersin? Hadi yapalım! Ama yapmadan önce bir tablo oluşturalım.

```
CREATE TABLE Notes (
  note_author VARCHAR(18) NOT NULL,
  note_description TEXT CHARACTER SET utf8 NOT NULL,
  note_time DATETIME DEFAULT (CURRENT_TIME)
); 
```

```
(1-) if (!args[0]) return message.channel.send("Bir içerik girmek zorundasın!")

(2-) con.query(`SELECT * FROM Notes WHERE note_author = '${message.author}'`, async (err, rows) => {
(3-)  if (err) throw err;
(4-)  if (rows.length < 1) {
(5-)  con.query(`INSERT INTO Notes (note_author, note_description) VALUES ('${message.author.id}','${args.join(" ")}')`)
(6-)  } else {
(7-)    con.query(`UPDATE Notes SET note_description = '${args.join(" ")} WHERE note_author = '${message.author.id}''`)
    }
})
```

Gözünüz korktu mu? Korktuysa hiç korkmasına gerek yok tek tek bütün numaralı yerleri anlatacağım. 

**1-** Burada hiçbir not içeriği girmediysen komutu çalıştırma dedik.
**2-** Burada o üyenin bütün notlarını çekmeye çalışıyoruz. 
**3-** Herhangi bir hata varsa onu konsola at diyoruz.
**4-** Eğer o `SELECT` aramasından çıkan sonuçların toplamı birden düşükse **5-** satırı çalıştır diyoruz.
**5-** Üyenin hiçbir notu olmadığı için yeni bir not oluşturmak zorundayız değil mi? O halde `INSERT INTO` ile oluşturuyoruz.
**6-** Şimdi de eğer üyenin notu varsa **7-** satırı çalıştır diyoruz.
**7-** Adamın eski notunun olduğunu bildiğimiz için o notu düzenlemesini istiyoruz. 

## Ağlayarak tekrar MySQL'a dönmek: `ALTER TABLE`.
> Bir tablonuz var ama içindeki verilerinizi kaybetmeden tablonuza yeni kolonlar eklemek, değiştirmek veya silmek istiyor olabilirsiniz. Bunun için `ALTER TABLE` yardıma koşuyor. 

### Kolon Eklemek

Aşağıda örnek bir kolon verdik. Hemen göz atalım.

```
kitap_numarasi | kitap_sahibi | kutuphane_karti 

0065           | Melis SEZEN  | TEK_GIRIS
0031           | Yasin SEVGİM | COKLU_GIRIS
```

Üstte sadece üç adet kolonun olduğunu görüyorsunuz. Bu tablonun adı: `KUTUPHANE`. Yeni eklenen kolonlar her zaman en sona eklenir. 

```
ALTER TABLE KUTUPHANE ADD kutuphane_gecerliligi TEXT CHARACTER SET utf8 NOT NULL;
```

Sonucumuz: 

```
kitap_numarasi | kitap_sahibi | kutuphane_karti | kutuphane_gecerliligi

0065           | Melis SEZEN  | TEK_GIRIS       | (NULL?)
0031           | Yasin SEVGİM | COKLU_GIRIS     | (NULL?)
```

Üstte görüldüğü gibi yeni bir kolon ekleyebildik. Fakat burada bir problem mevcut, `kutuphane_gecerliligi` kolonunu `NOT NULL` olarak belirlediğimiz için burada `NULL` ifadelerine kesin olarak veri atanmalı. 

**Uyarı:** `DEFAULT` değerler için `NULL` almadan otomatik olarak yerleştirme yapar. 

### Kolon Silmek 

Üstte eklediğimiz `kutuphane_gecerliligi` kolonunu silmek için aşağıdaki ifadeyi kullanmak yeterli.

```
ALTER TABLE KUTUPHANE DROP COLUMN kutuphane_gecerliligi
```

### Kolon Düzenlemek

Halihazırda olan bir kolonun yeni veri tipine geçmesi için aşağıdaki `query` çalıştırılmalı:

```
ALTER TABLE KUTUPHANE MODIFY COLUMN kutuphane_gecerliligi BOOLEAN DEFAULT true
```

## Biz kural adamı değiliz moruk, kurallara uymayan havalıdır.
> Veri tipini belirlerken en sonda kullandığınız içerikler genelde kural belirleyicilerdir. Toplam yedi adet kural belirleyici vardır.

- **NOT NULL**: Bu verinin `NULL` olmadığının kuralını belirler.
- **UNIQUE**: Herkes "*yünik*" kardeşim. Unique olarak belirlenen bir veriden bir tane daha olamaz. Bu karışıklıkların önüne geçmek için iyi bir yol.
- **PRIMARY KEY**: Bu ibne, en iyisi benim diyenler için var. Bir veri tipinde kullanılabilen ilk anahtar kuralı `UNIQUE` ve `NOT NULL`'un birleşimidir. 
- **FOREIGN KEY**: İleri düzeye başladığınız zaman öğreneceğiniz yabancı anahar kuralı iki tabloyu birbirine bağlamanızı sağlar. Mesela tek bir silme işlemi yaparken iki yerden otomatik olarak silmek gibi. (Örnek: T.C. ile ilgili kişilerin bilgilerinin bulunduğu bir veritabanı ve kişilerin işledikleri suçlarla ilgili veritabanı yarattınız diyelim. T.C.'den birisini atarken tek bir T.C.'yi silmeniz iki veritabanından bu bilgileri kaldırmasına sebep olacak.)
- **CHECK**: Koşul yaratmanızı sağlar. Mesela bir veri tipini yaş olarak kaydettiniz ve kaydedilecek yaşların muhakkak 18'den büyük olmasını istiyorsanız.
- **DEFAULT**: Üstte bir çok defa anlatılan varsayılan değer kuralı.
-  **CREATE INDEX**: Bu ibneyi ben bile çözemedim. İndeks yaratmanıza sebep oluyor. Daha hızlı veri çekmek ve akış sağlamak için gerekli olduğunu bilmeniz yeterli. İndeksler insanlar tarafından görülemez. 

## İbneye bak saat tutuyor, sen benim yediğimi mi sayıyorsun Talat?
> MySQL saat biçimi tutmak için harika yollardan birisi. Bir sürü saat tutabileceğiniz tip var ve oldukça sağlam çalışıyorlar. Hadi onları inceleyelim.

```
DATE formatı: YYYY-MM-DD
DATETIME formatı: YYYY-MM-DD HH:MI:SS
TIMESTAMP formatı: YYYY-MM-DD HH:MI:SS
YEAR formatı: YYYY or YY
```

### `DATETIME` benim saçımı çekiyor, `TIMESTAMP` neredesin lanet olsun.
> Bir şeyi söylemeyi unutmuş olabilirim. `TIMESTAMP` bildiğiniz `TIMESTAMP`'lardan değil. Evet o bakire deği- ay pardon yanlış yere girdik. `TIMESTAMP` o karışık `3131313131313131` biçimindeki saatlerden değil. MySQL'da `TIMESTAMP` olarak saat saklamanız mümkün değil. Bundan dolayı kayıt ederken muhakkak tarih kayıt etmelisiniz. 

Peki `TIMESTAMP` ile `DATETIME` arasındaki fark ne? MySQL zamanları tutmayı şundan dolayı hedefler: her veri değiştiğinde bu verinin ne zaman değiştiğini tespit etmek için. Sizin amacınız farklı olabilir siz zaman verilerini direk depolamak isteyebilirsiniz. Bu gibi durumlarda `DATETIME` kullanmanız gerekiyor. 

`TIMESTAMP` ise dönüştürülme için gereklidir. UTC Tipi kayıt yapıldığı için `TIMESTAMP` kayıtları kullanıcıdan kullanıcıya değişiklilik gösterebilir. Bu ne demek oluyor? 

```
Örnek bir TIMESTAMP: 15 Mayıs 1990 GMT+1 Orta Avrupa Pasifik Saati
```

Biz Türkiye'den görüntülemek isteseydik bu `TIMESTAMP` bize göre değişecek. 

## Yine başladık mı? Yahu biz bunları bilmiyor muyuz? Veri tiplerini tekrar araştıralım...
> Bizim öğrendiğimiz kısıtlı veri tiplerini biliyorsunuz. (`BOOLEAN, TEXT, VARCHAR, INT` gibi.) Hadi derinlere dalalım.

### String Türleri
```
CHAR(karakter?) => VARCHAR(karakter?)
- Bildiğiniz üzere VARCHAR(18) kullanmıştık. Bu neydi? İçine en fazla 18 hanelik verilerin
girebileceğini anlattık. CHAR aslında VARCHAR ile aynı ama bazı farklılıkları var. 
CHAR kullanabilmek için sabit uzunluğa ihtiyacınız vardır. Yani siz T.C. ile ilgili bir 
veritabanı oluşturdunuz diyelim, T.C.'lerin uzunluğu değişebilir mi? Hayır değişemez. Bundan
dolayı CHAR(11) yapmanız daha mantıklı. Ama isim verilerini tutarken isimler değişkenlik
gösterebilir değil mi? Bundan dolayı VARCHAR kullanmanız gerekir.

CHAR, VARCHAR'a göre %50 daha hızlıdır.
CHAR SMA (static memory allocation) kullanır. VARCHAR DMA (dynamic memory allocation) kullanır.
CHAR'ın tutabileceği en büyük karakter 225'tir. VARCHAR ise 65,535 karakter tutabilir. (En çok)

BINARY(karakter?) = VARBINARY(karakter?)
- CHAR ve VARCHAR ile aynı gibi düşünebilirsiniz. Tek farkları BINARY taşıyabilmeleri. 

TINYBLOB => MEDIUMBLOB => BLOB(karakter?) => LONGBLOB
- BLOB'ları taşımak için kullanılır. TINYBLOB 225 Byte yere kadar taşıma yapabilir.
MEDIUMBLOB'lar 16,777,215 Byte'ya kadar taşıma yapabilir. Bu sayı LONGBLOB'da 4,294,967,295
Byte'ya kadar çıkar.  (BLOB'lar Binary şeklindeki Objelerdir.)

ENUM(değer1, değer2, değer3 ...)
- Önceden girilen değerlerin seçilebileceği ENUM seçeneklerde kullanılır. Değer olarak belirt-
ilmeyen değer girişinde boş değer girilir. 65535'e kadar değer destekler.

SET(değer1, değer2, değer3 ...)
En fazla 64 değer alabilen SET veri tipinde hiçbir değer seçilmeyebilir ya da birden çok değer
seçilebilir.
```

### Sayı Türleri (INT)

```
BIT(büyüklük?)
1 ila 64 arasındaki BIT'leri tutabilir.

TINYINT(sayı?) => SMALLINT(sayı?) => MEDIUMINT(sayı?) => INT(sayı?) => BIGINT(sayı?)

TINYINT en fazla -128 ila 127 sayılarını (ve arasındakini) tutabilir.
SMALLINT en fazla -32768 ila 32767 sayılarını (ve arasındakini) tutabilir.
MEDIUMINT en fazla -8388608 ila 8388607 sayılarını (ve arasındakini) tutabilir.
INT en fazla -2147483648 ila 2147483647 sayılarını (ve arasındakini) tutabilir.
BIGINT en fazla -9223372036854775808 ila 9223372036854775807 sayılarını (ve arasındakini)
tutabilir.
INT(sayı?) === INTEGER(sayı?)'dır.

BOOL = BOOLEAN
0 veya 1 olarak veri tutar. (true/false.)
İleri düzey bilgi: BOOLEAN veriler 0 veya 1 olabilir desek de, 1'den büyük değerleri alırsa
1'e eşit yani true olarak görülür. Sadece 0 değeri false değer alabilir.

Allah'ın unuttuğu diğer veri tipleri ve ananızın ağlayacağı diğer türler: FLOAT(), 
DOUBLE(), DOUBLE PRECISION(), DEC() veya DECIMAL().

ALLAHIN BELASI UNSIGNED ve ZEROFILL'in anlatımı aşağıda.
```

### ZEROFILL'i ve UNSIGNED'i üretenin ailevi değerlerine selam olsun.
> Anlaması kıt arkadaşlarımız için kısa özet: `ZEROFILL` varsa sadece `TINYINT` değerler için varolabilir. Bu ne demek? `ZEROFILL` sadece `TINYINT` için çalışır demek. Peki ne iş yapıyor? Sayının sonuna ne kadar sıfır isterseniz herhangi bir veri girişinde o kadar sıfır ekliyor. `UNSIGNED` ise masum görünebilir bu sayının sadece `POSITIVE` değerler alabileceğini gösterir. `UNSIGNED` her türlü `INTEGER` değerde kullanılabilir. Bununla da kalmaz yukardaki öğrendiğiniz bütün sayıların alabileceği değerlerin amına koyar.

`UNSIGNED` Değerleri:
`TINYINT`: 0 ila 255 arası. 
`SMALLINT`: 0 ila 65535 arası.
`MEDIUMINT`: 0 ila 16777215 arası.
`INT`: 0 ila 4294967295 arası.
`BIGINT`: 0 ila 18446744073709551615 arası. 

```
Diğer dillerde 18446744073709551615 sayısının okunuşu. NEDEN BÖYLE BİR SAYI VAR AMINA
KODUKLARIM BU KADAR BÜYÜK SAYIYI NE YAPIYORSUNUZ?

TÜRKÇE: on sekiz kentrilyon dört yüz kırk altı katrilyon yedi yüz kırk dört trilyon yetmiş üç 
milyar yedi yüz dokuz milyon beş yüz elli bir bin altı yüz on beş.

LEHÇE: osiemnaście trylionów czterysta czterdzieści sześć biliardów siedemset czterdzieści
cztery biliony siedemdziesiąt trzy miliardy siedemset dziewięć milionów pięćset 
pięćdziesiąt dwa tysiące osiemset siedem. (Bu ne amına koyayım.)

İNGİLİZCE: eighteen quintillion four hundred forty-six quadrillion seven hundred forty-four 
trillion seventy-three billion seven hundred nine million five hundred fifty-two thousand 
eight hundred seven.
```

## Veritabanınızı kemiren fareler için fare tuzağı. Backup DB!
> Veritabanlarınızı güvenli çözümlerden birisi olan `BACKUP` ile koruyun. Kısa anlatımla `BACKUP` yedek alma işlemidir. Sonundaki uzantısı `.bak` olarak bitmek zorunda olduğunu unutmayın.

```
BACKUP DATABASE testDB TO DISK = 'D:\backups\testDB.bak';
```

Seçtiğiniz yere anında yedek alacaktır. (Uzun veritabanlarında işlem sürer.) Bu tüm `BACKUP` alınması anlamına gelir. `DIFFERENTIAL` biçimde almak isteyenleri aşağıya beklerim. (Bu ne lan? Diyenleri görüyorum. `DIFFERENTIAL Backup` türü diğer aldığınız yedekleme ile şu anki veritabanını karşılaştırır ve sadece değişikliklerin yedeklerini alır.)

```
BACKUP DATABASE testDB TO DISK = 'D:\backups\testDB.bak' WITH DIFFERENTIAL;
```
