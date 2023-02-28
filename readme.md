# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

# Proje Kurulumu
Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

## Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#Tablolar 
`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]



# Görevler
Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın. 



	1) ÖRNEK SORU: Yazar tablosunu KEMAL UYUMAZ isimli yazarı ekleyin.
	
		insert into yazar (yazarad, yazarsoyad) values ("Kemal", "Uyumaz");
		select * from yazar where yazarad="Kemal" and yazarsoyad="Uyumaz";
	
	2) Biyografi türünü tür tablosuna ekleyiniz.
	
		insert into tur (turadi) values ("Biyografi");
		select * from tur where turadi="Biyografi";

	3) 10A sınıfı olan ÇAĞLAR ÜZÜMCÜ isimli erkek, sınıfı 9B olan LEYLA ALAGÖZ isimli kız ve sınıfı 11C olan Ayşe Bektaş isimli kız öğrencileri tek sorguda ekleyin.

		insert into ogrenci (sinif, ograd, ogrsoyad, cinsiyet) values
		("10A", "Çağlar", "Üzümcü", "E"),
		("9B", "Leyla", "Alagöz", "K"),
		("11C", "Ayşe", "Bektaş", "K");
		select * from ogrenci;
	
	
	4) Öğrenci tablosundaki rastgele bir öğrenciyi yazarlar tablosuna yazar olarak ekleyiniz.

		insert into yazar (yazarad, yazarsoyad) select ograd, ogrsoyad from ogrenci order by random() limit 1;
		select * from yazar;
	
	
	5) Öğrenci numarası 10 ile 30 arasındaki öğrencileri yazar olarak ekleyiniz.

		insert into yazar (yazarad, yazarsoyad) select ograd, ogrsoyad from ogrenci where ogrno between 10 and 30;
		select * from yazar;

	
	6) Nurettin Belek isimli yazarı ekleyip yazar numarasını yazdırınız.
	(Not: Otomatik arttırmada son arttırılan değer @@IDENTITY değişkeni içinde tutulur.)

		insert into yazar (yazarad, yazarsoyad) values ("Nurettin", "Bellek");
		select scope_identity();
	
	
	7) 10B sınıfındaki öğrenci numarası 3 olan öğrenciyi 10C sınıfına geçirin.

		update ogrenci set sinif="10C" where ogrno="3" and sinif="10B";
		select * from ogrenci where ogrno=3;
	
	8) 9A sınıfındaki tüm öğrencileri 10A sınıfına aktarın

		update ogrenci set sinif="10A" where sinif="9A";
		select * from ogrenci where sinif="9A";
	
	
	9) Tüm öğrencilerin puanını 5 puan arttırın.

		update ogrenci set puan=puan+5 where puan is not null;
		select * from ogrenci;
	
	10) 25 numaralı yazarı silin.

		select yazarad as "Yazar Adı" from yazar where yazarno=25;
		delete from yazar where yazarno=25;
		select * from yazar where yazarno=25;
		//[22:01:47] Error while executing SQL query on database 'ogrenci': FOREIGN KEY constraint failed


	11) Doğum tarihi null olan öğrencileri listeleyin. (insert sorgusu ile girilen 3 öğrenci listelenecektir)

		select * from ogrenci where dtarih is null;
	
	
	12) Doğum tarihi null olan öğrencileri silin.

		delete from ogrenci where dtarih is null;
		select * from ogrenci;
	
	
	13) Kitap tablosunda adı a ile başlayan kitapların puanlarını 2 artırın.

		update kitap set puan=puan+2 where kitapadi like "A%";
		select * from kitap;
	
	
	14) Kişisel Gelişim isimli bir tür oluşturun.

		insert into tur (turadi) values ("Kişisel Gelişim");
		select * from tur where turadi="Kişisel Gelişim";
	
	
	15) Kitap tablosundaki Başarı Rehberi kitabının türünü bu tür ile değiştirin.

		update kitap set turno=(select turno from tur where turadi="Kişisel Gelişim") where kitapadi="Başarı Rehberi";
		select * from kitap where turno=9;
	
	
	16) Öğrenci tablosunu kontrol etmek amaçlı tüm öğrencileri görüntüleyen "ogrencilistesi" adında bir prosedür oluşturun.

		create procedure ogrencilistesi as select * from ogrenci;
		exec ogrencilistesi;

		/https://www.sqlekibi.com/sql-server/stored-procedure-olusturmak-2.html/
		/http://www.ibrahimbayraktar.net/2014/10/sql-serverda-stored-procedure-temel.html/
		//[22:24:17] Error while executing SQL query on database 'ogrenci': near "procedure": syntax error
	
	
	17) Öğrenci tablosuna yeni öğrenci eklemek için "ekle" adında bir prosedür oluşturun.

		create procedure ekle @isim nvarchar(50), @soyisim nvarchar(50), @sinif nvarchar(5), @cinsiyet nvarchar(1)
		as insert into ogrenci (sinif, ograd, ogrsoyad, cinsiyet) values (@sinif, @isim, @soyisim, @cinsiyet);

		exec ekle "Kenan", "Aksoy", "18A", "E";

		/http://www.ibrahimbayraktar.net/2014/10/sql-serverda-stored-procedure-temel.html/
	
	18) Öğrenci noya göre öğrenci silebilmeyi sağlayan "sil" adında bir prosedür oluşturun.

		create procedure sil @id numeric(10) as delete from ogrenci where ogrno=@id;

		exec sil 32
	
	
	19) Öğrenci numarasını kullanarak kolay bir biçimde öğrencinin sınıfını değiştirebileceğimiz bir prosedür oluşturun.

		create procedure sinifdegis @no numeric(10, @sinif nvarchar(25) as update ogrenci set sinif=@sinif where ogrno=@no;

		exec sinifdegis 32, "10A";
	
	
	20) Öğrenci adı ve soyadını "Ad Soyad" olarak birleştirip, ad soyada göre kolayca arama yapmayı sağlayan bir prosedür yazın.

		create procedure adsoyadbirlesiklistele as select concat(ograd, ogrsoyad) as adsoyad from ogrenci;
		 /*select adsoyad from (select concat(ograd, ogrsoyad) as adsoyad from ogrenci) as birogrenci where adsoyad="Hülya Yiğit"*/

		exec adsoyadbirlesiklistele
	
	21) Daha önceden oluşturduğunu tüm prosedürleri silin.
		
		drop procedure [procedurler]; //https://learn.microsoft.com/en-us/sql/relational-databases/stored-procedures/delete-a-stored-procedure?view=sql-server-ver16
	
	
	#Esnek görevler (Esnek görevlerin hepsini Select in Select ile gerçekleştirmeniz beklenmektedir.)
	22) Select in select yöntemiyle dram türündeki kitapları listeleyiniz.
		
		select * from tur;
		select * from kitap;
		select * from kitap where turno=(select turno from tur where turadi="Dram");
		//Dram sorgusu tekilde gelemedi sqllitestudioda test engineerde geldi
	
	23) Adı e harfi ile başlayan yazarların kitaplarını listeleyin.

		select yazarno from yazar where yazarad like "E%")
		select * from kitap where yazarno in (select yazarno from yazar where yazarad like "E%");

	
	24) Kitap okumayan öğrencileri listeleyiniz.
		 
		 select * from ogrenci where ogrno not in (select ogrno from islem);

	25) Okunmayan kitapları listeleyiniz

		select * from kitap where kitapno not in (select kitapno from islem);
		//başarı kitapı tek geldi son eklenen

	
	26) Mayıs ayında okunmayan kitapları listeleyiniz.

		select * from kitap;
		select * from islem;
		select * from islem where vtarih not like "%-05-%";
		select * from kitap where kitapno in (select kitapno from islem where vtarih not like "%-05-%");
		select * from kitap where kitapno in (select kitapno from islem where vtarih not like "____-05-%" and atarih not like "____-05-%");


# Görevler
Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın. 



	1) ÖRNEK SORU: Yazar tablosunu KEMAL UYUMAZ isimli yazarı ekleyin.
	

	
	2) Biyografi türünü tür tablosuna ekleyiniz.
	
	
	3) 10A sınıfı olan ÇAĞLAR ÜZÜMCÜ isimli erkek, sınıfı 9B olan LEYLA ALAGÖZ isimli kız ve sınıfı 11C olan Ayşe Bektaş isimli kız öğrencileri tek sorguda ekleyin. 
	
	
	4) Öğrenci tablosundaki rastgele bir öğrenciyi yazarlar tablosuna yazar olarak ekleyiniz.
	
	
	5) Öğrenci numarası 10 ile 30 arasındaki öğrencileri yazar olarak ekleyiniz.
	
	
	6) Nurettin Belek isimli yazarı ekleyip yazar numarasını yazdırınız.
	(Not: Otomatik arttırmada son arttırılan değer @@IDENTITY değişkeni içinde tutulur.)
	
	
	7) 10B sınıfındaki öğrenci numarası 3 olan öğrenciyi 10C sınıfına geçirin.
	
	
	8) 9A sınıfındaki tüm öğrencileri 10A sınıfına aktarın
	
	
	9) Tüm öğrencilerin puanını 5 puan arttırın.
	
	
	10) 25 numaralı yazarı silin.


	11) Doğum tarihi null olan öğrencileri listeleyin. (insert sorgusu ile girilen 3 öğrenci listelenecektir)
	
	
	12) Doğum tarihi null olan öğrencileri silin. 
	
	
	13) Kitap tablosunda adı a ile başlayan kitapların puanlarını 2 artırın.
	
	
	14) Kişisel Gelişim isimli bir tür oluşturun.
	
	
	15) Kitap tablosundaki Başarı Rehberi kitabının türünü bu tür ile değiştirin.
	
	
	16) Öğrenci tablosunu kontrol etmek amaçlı tüm öğrencileri görüntüleyen "ogrencilistesi" adında bir prosedür oluşturun.
	
	
	17) Öğrenci tablosuna yeni öğrenci eklemek için "ekle" adında bir prosedür oluşturun.
	
	
	18) Öğrenci noya göre öğrenci silebilmeyi sağlayan "sil" adında bir prosedür oluşturun.
	
	
	19) Öğrenci numarasını kullanarak kolay bir biçimde öğrencinin sınıfını değiştirebileceğimiz bir prosedür oluşturun.
	
	
	20) Öğrenci adı ve soyadını "Ad Soyad" olarak birleştirip, ad soyada göre kolayca arama yapmayı sağlayan bir prosedür yazın.
	
	
	21) Daha önceden oluşturduğunu tüm prosedürleri silin.
	
	
	#Esnek görevler (Esnek görevlerin hepsini Select in Select ile gerçekleştirmeniz beklenmektedir.)
	22) Select in select yöntemiyle dram türündeki kitapları listeleyiniz.
	
	
	23) Adı e harfi ile başlayan yazarların kitaplarını listeleyin.
	
	
	24) Kitap okumayan öğrencileri listeleyiniz.
	
	
	25) Okunmayan kitapları listeleyiniz

	
	26)Mayıs ayında okunmayan kitapları listeleyiniz.