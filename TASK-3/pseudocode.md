BAŞLA
AL hasta_tc_kimlik_no
EĞER tc_kimlik_no harf içeriyorsa YADA uzunluk(tc_kimlik_no) < 11 YADA uzunluk(tc_kimlik_no) > 11 YAP
    GÖSTER "TC Kimlik No. hatalı"
    GİT satır 2
GÖSTER "Hangi işlemi yapmak istiyorsunuz?"
EĞER seçilen_işlem == randevu YAP
    GÖSTER "Poliklinik seçin"
    AL poliklinik doktor
    EĞER uygun doktor yoksa YAP
        GÖSTER "Uygun doktor bulunamadı"
        GİT satır 5
    EĞER doktor seçtiyse YAP
        AL doktor saatler
        EĞER saat seçtiyse YAP
            GÖNDER sms
            DOĞRULA randevu
            GÖSTER "Başka işlem yapmak istermisiniz?"
            EĞER baska_islem == true YAP
                GİT satır 3
            BİTİR
        DEĞİLSE
            GİT satır 10
    DEĞİLSE YAP
        GİT satır 5

EĞER seçilen_işlem == tahlil YAP
    EĞER sonuç hazırsa YAP
        GÖSTER "PDF'i indirmek istermisiniz?"
        EĞER indirmek istiyorsa YAP
            İNDİR PDF
            GÖSTER "Başka işlem yapmak istermisiniz?"
            EĞER baska_islem == true YAP
                GİT satır 3
            BİTİR
    DEĞİLSE YAP
        GÖSTER "Tahlil sonuçlarınız henüz hazır değil. Başka bir işlem yapmak istermisiniz?"
        EĞER baska_islem == true YAP
                GİT satır 3
        BİTİR