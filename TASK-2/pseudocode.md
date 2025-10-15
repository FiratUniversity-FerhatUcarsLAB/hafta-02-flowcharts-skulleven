BAŞLA
HER kullanıcı ürün sayfasına tıkladığında İSE
    GÖSTER ürün_sayfası
    EĞER sepete ekle tuşuna basılmış İSE
        EĞER istenilen_miktar >= stoktaki_miktar İSE
            EKLE sepet eşya istenilen_miktar
        DEĞİLSE
            GÖSTER "yeterli stok yok."
EĞER kullanıcı sepet sayfasına tıklar İSE
    GÖSTER giriş_ekranı
    AL kullanıcı_adı
    AL şifre
    EĞER kullanıcı_adı != dogru_kullanıcı_adı İSE
        GÖSTER "lütfen giriş bilgilerinizi doğru girin."
    DEĞİLSE
        GÖSTER sepet
        kargo_ucreti = 40
        EĞER fiyat >= 200 YAP
            kargo_ucreti = 0
        EĞER sepetteki eşya silinirse YAP
            ÇIKAR sepet eşya istenilen_miktar
        EĞER fiyat >= 50 YAP
            satin_alabilir = true
        EĞER indirim kodu uygulanırsa YAP
            EĞER indirim_kodu_kontrol_et(indirim_kodu) == true YAP
                fiyat = fiyat * indirim_yüzdesi
            DEĞİLSE
                GÖSTER "indirim kodu geçerli değil."
        EĞER satin al tuşuna basılmışsa YAP
            EĞER satin_alabilir == false YAP
                GÖSTER "Sipariş oluşturmak için sepetinizin tutarının en az 50 TL olması gerek"
                ÇIK
            AL adres
            AL ödeme_yöntemi
            EĞER ödeme_yöntemi == "Kredi Kartı" YAP
                AL kart_numarasi
                AL son_kullanma_tarihi
                AL guvenlik_kodu
                HER sepetteki eşya İÇİN
                    EĞER sepet[eşya] != depo[eşya]
                        GÖSTER "sepetteki eşyanız artık stokta bulunmamaktadır."
                        ÇIK.
                ödeme_firmasına_gönder(kart_numarasi, son_kullanma_tarihi, guvenlik_kodu)
                EĞER ödeme tamamlanırsa YAP
                    HER sepetteki eşya İÇİN
                        çıkar(depo(sepet[eşya]))
                GÖNDER doğrulama_epostası
                GÖSTER "Siparişiniz Başarılıyla alınmıştır. Tekrar siteye dönmek istermisiniz"
                EĞER siteye dön tuşuna basılmışsa YAP
                    En başa dön
BİTİR
                
        


    
