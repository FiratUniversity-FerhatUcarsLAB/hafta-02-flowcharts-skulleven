BAŞLA
AL öğrenci_no
AL şifre
EĞER dogrula(öğrenci_no, şifre) == false YAP
    GÖSTER "öğrenci no veya şifre yanlış"
    GİT satir 2
GÖSTER ders_listele(öğrenci_no) // öğrencinin hangi derslere girebileceğini sunucudan alıp listele
EĞER ders seçerse YAP
    EĞER ders_kontenjan > 0 YAP
        EĞER ön koşul dersi alınmışsa YAP
            EĞER ders diğer derslerin zamanıyla çakışmıyorsa YAP
                EĞER ders zaten seçilmemişse
                    ders_listesi += ders
                    GÖSTER "Başka bir işlem yapmak istermisiniz?"
                    EĞER başka işlem yapılacaksa YAP
                        GİT satir 7
                DEĞİLSE YAP
                    GÖSTER "Seçtiğiniz dersi daha önceden seçmiştiniz"
            DEĞİLSE YAP
                GÖSTER "Seçtiğiniz dersin zamanı diğer seçtiğiniz derslerle çakışıyor"
        DEĞİLSE YAP
            GÖSTER "Seçtiğiniz dersin ön koşul dersini almamışsınız
    DEĞİLSE YAP
        GÖSTER "Seçtiğiniz dersin kontenjanı dolu"
EĞER ders çıkarırsa YAP
    ders_listesi -= seçilen_ders
    GÖSTER "Ders listeden çıkarılmıştır."

EĞER kayıt tamamla tuşuna basılmışsa YAP
    GÖSTER kayıt özeti
    EĞER tamam tuşuna basılırsa YAP
        EĞER ortalama_al(öğrenci_no) <= 2.5 YAP
            GÖSTER "Ders seçimini tamamlayabilmeniz için danışman onayından geçmeniz gerek."
        DEĞİLSE YAP
            HER ders_listesi İÇİNDEKİ ders İÇİN
                EĞER ders.kontenjan <= 0 YAP
                    GÖSTER "Gösterilen dersin kontenjanı dolmuştur."
                    GİT satır 7
                DEĞİLSE YAP
                    ders.kontenjan -= 1
            kayıt_tamamla(ders_listesi)