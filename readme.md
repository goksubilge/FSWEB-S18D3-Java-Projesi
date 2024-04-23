# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

## Proje Kurulumu
Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

### Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#### Tablolar 
`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]


##### Görevler
Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın. 


MIN-MAX, COUNT-AVG-SUM, GROUP BY, JOINS (INNER, OUTER, LEFT, RIGHT
	#ilk 3 soruyu join kullanmadan yazın.

1) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.	

	SELECT ograd, ogrsoyad, i.atarih 
	FROM ogrenci AS o, islem AS i 
	WHERE o.ogrno = i.ogrno

2) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.

   SELECT k.kitapadi, t.turadi 
   FROM kitap AS k, tur AS t	
   WHERE k.turno = t.turno AND t.turadi IN ('Fıkra', 'Hikaye')

3) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.
   
   SELECT  o.ogrno, o.ograd, o.ogrsoyad, k.kitapadi, o.sinif 
   FROM ogrenci AS o, kitap AS k, islem AS i 
   WHERE o.ogrno = i.ogrno AND i.kitapno = i.kitapno AND o.sinif IN ('10B','10C') 
   ORDER BY o.ograd, o.ogrsoyad
       

4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin. #join ile yazın
   
   SELECT o.ograd, o.ogrsoyad, i.atarih 
   FROM ogrenci as o 
   INNER JOIN islem as i ON o.ogrno = i.ogrno 
   ORDER BY i.atarih DESC
	
5) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.

   SELECT k.kitapadi, t.turadi FROM kitap as k 
   INNER JOIN tur as t ON k.turno =  t.turno 
   WHERE t.turadi IN('Fıkra', 'Hikaye')

6) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.

   SELECT  o.ogrno, o.ograd, o.ogrsoyad, k.kitapadi 
   FROM ogrenci as o 
   INNER JOIN islem as i ON o.ogrno = i.ogrno 
   INNER JOIN kitap as k ON k.kitapno =  i.kitapno 
   WHERE o.sinif IN('10C', '10B') 
   ORDER BY o.ograd, o.ogrsoyad
	
7) Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.
   
   SELECT o.ograd, o.ogrsoyad, i.atarih 
   FROM ogrenci as o 
   LEFT JOIN islem as i ON o.ogrno = i.ogrno 
   ORDER BY o.ograd, o.ogrsoyad

8) Kitap almayan öğrencileri listeleyin.

   SELECT o.ograd, o.ogrsoyad, i.atarih 
   FROM ogrenci as o 
   LEFT JOIN islem as i ON o.ogrno = i.ogrno 
   WHERE i.atarih is null

	----

   SELECT * FROM ogrenci 
   WHERE ogrno NOT IN (select ogrno FROM islem)

9) Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.

   SELECT k.kitapno, k.kitapadi , 
   COUNT(i.kitapno) 
   FROM kitap as k 
   INNER JOIN islem as i ON i.kitapno = k.kitapno 
   GROUP BY k.kitapno, k.kitapadi 
   ORDER BY k.kitapno

10) Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.

    SELECT k.kitapno, k.kitapadi , 
    COUNT(i.kitapno) 
    FROM kitap as k 
    LEFT JOIN islem as i ON i.kitapno = k.kitapno 
    GROUP BY k.kitapno, k.kitapadi 
    ORDER BY k.kitapno

11) Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.
	
    SELECT o.ograd, o.ogrsoyad, k.kitapadi 
    FROM ogrenci as o 
    INNER JOIN islem as i ON o.ogrno = i.ogrno 
    INNER JOIN kitap as k ON i.kitapno = k.kitapno 
    ORDER BY o.ograd, o.ogrsoyad

12) Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.
    
    SELECT o.ogrno, o.ograd, o.ogrsoyad, k.kitapadi, y.yazarad, y.yazarsoyad, t.turadi, i.atarih 
    FROM ogrenci as o 
    LEFT JOIN islem as i ON o.ogrno = i.ogrno 
    LEFT JOIN kitap as k ON i.kitapno = k.kitapno 
    LEFT JOIN yazar as y ON k.yazarno = y.yazarno 
    LEFT JOIN tur as t ON k.turno = t.turno 
    ORDER BY o.ograd, o.ogrsoyad

	----

    SELECT o.ogrno, o.ograd, o.ogrsoyad, k.kitapadi, y.yazarad, y.yazarsoyad, t.turadi, i.atarih 
    FROM islem as i 
    INNER JOIN kitap as k ON i.kitapno = k.kitapno  
    INNER JOIN yazar as y ON k.yazarno = y.yazarno  
    INNER JOIN tur as t ON k.turno = t.turno  
    RIGHT JOIN ogrenci as o ON o.ogrno = i.ogrno 
    ORDER BY o.ograd, o.ogrsoyad
    
13) 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.
    
    SELECT o.ograd, o.ogrsoyad, 
    COUNT(i.ogrno) as kitap_sayisi 
    FROM ogrenci as o 
    INNER JOIN islem as i ON i.ogrno = o.ogrno 
    WHERE o.sinif IN ('10A', '10B') 
    GROUP BY o.ograd, o.ogrsoyad 
    ORDER BY kitap_sayisi DESC
    
14) Tüm kitapların ortalama sayfa sayısını bulunuz. #AVG

    SELECT AVG(sayfasayisi) FROM kitap

15) Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.	
    
    SELECT * FROM kitap 
    WHERE sayfasayisi > (SELECT avg(sayfasayisi) FROM kitap)

16) Öğrenci tablosundaki öğrenci sayısını gösterin

    SELECT count(*) FROM ogrenci

	-----

	SELECT count(ogrno) FROM ogrenci

17) Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.	

    SELECT COUNT(ogrno) AS 'toplam sayı' FROM ogrenci

18) Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.	

    SELECT COUNT(DISTINCT ograd) FROM ogrenci

19) En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.	

    SELECT MAX(sayfasayisi) FROM kitap
    
    ---- 
    
    SELECT * FROM kitap ORDER BY sayfasayisi LIMIT 1

20) En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.	

    SELECT k.kitapadi, k.sayfasayisi 
    FROM kitap AS k WHERE k.sayfasayisi = (SELECT MAX(sayfasayisi) FROM kitap)

21) En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.	

    SELECT MAX(sayfasayisi) FROM kitap

22) En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.

    SELECT k.kitapadi, k.sayfasayisi 
    FROM kitap AS k 
    WHERE k.sayfasayisi = (SELECT MIN(sayfasayisi) FROM kitap)

23) Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.	
        
    SELECT MAX(k.sayfasayisi) 
    FROM kitap AS k 
    INNER JOIN tur AS t ON k.turno=t.turno 
    WHERE trim(t.turadi)=trim('Dram ')

24) numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.	
    
    SELECT SUM(k.sayfasayisi) 
    FROM kitap as k 
    INNER JOIN islem AS i ON k.kitapno=i.kitapno 
    WHERE i.ogrno=15
        
    ----
    
    SELECT i.ogrno, SUM(k.sayfasayisi) 
    FROM kitap as k 
    INNER JOIN islem AS i ON k.kitapno=i.kitapno 
    WHERE i.ogrno>15 
    GROUP BY i.ogrno 
    HAVING SUM(k.sayfasayisi) > 5000

25) İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane 
    
    SELECT ograd,
    COUNT(ogrno) AS toplam 
    FROM ogrenci 
    GROUP BY ograd 
    ORDER BY toplam DESC

26) Her sınıftaki öğrenci sayısını bulunuz.	
    
    SELECT sinif, 
    COUNT(ogrno) 
    FROM ogrenci 
    WHERE sinif IS NOT NULL 
    GROUP BY sinif

27) Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.
    
    SELECT sinif,cinsiyet, 
    COUNT(ogrno) AS 'Ogrenci Sayisi' 
    FROM ogrenci 
    WHERE sinif IS NOT NULL AND cinsiyet IS NOT NULL 
    GROUP BY sinif,cinsiyet 
    
    ---- 
    
    SELECT COUNT(*) FROM ogrenci 
    WHERE sinif = '10A' AND cinsiyet IS NOT NULL

28) Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.	

    SELECT o.ograd,o.ogrsoyad,SUM(k.sayfasayisi) AS 'okunansayfa' 
    FROM ogrenci AS o 
    INNER JOIN islem as i ON o.ogrno =i.ogrno 
    INNER JOIN kitap as k ON i.kitapno =k.kitapno 
    GROUP BY o.ograd,o.ogrsoyad 
    ORDER BY okunansayfa  DESC

29) Her öğrencinin okuduğu kitap sayısını getiriniz.
    
    SELECT o.ograd,o.ogrsoyad, 
    COUNT(i.islemno) AS 'okuduguKitapSayisi' 
    FROM ogrenci AS o 
    INNER JOIN islem as i ON o.ogrno =i.ogrno 
    GROUP BY o.ograd,o.ogrsoyad 
    ORDER BY okuduguKitapSayisi DESC