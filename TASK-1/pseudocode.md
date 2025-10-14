BAŞLA
TAK ATM'ye kart
EĞER islem == para_cekme İSE
    deneme = 1
    HER deneme İÇİN kart_sifresi İSE
        GİR kart_şifresi
        EĞER kart_şifresi != dogru_kart_sifresi İSE
            EĞER deneme == 3 İSE
                BLOKE kart
                BİTİR
            DEĞİLSE
                deneme += 1
        DEĞİLSE
            ÇIK
    AL bakiye
    GİR tutar
    EĞER tutar >= bakiye İSE
        yeterlibakiye = true
        EĞER tutar / 20 == 0 İSE
            yirmininkati = true
            EĞER tutar <= limit İSE
                EĞER tutar <= atm_deki_para İSE
                    EĞER atm_deki_fis > 0 İSE
                        VER FİŞ
                    VER tutar
    EĞER yeterlibakiye YADA yirmininkati == false iSE
        ÇIK
DEĞİLSE
    // işlemi yap.
    ÇIK
YAZ "başka bir işlem yapmak istermisiniz"
EĞER cevap == "Evet" İSE
    DÖN 3. adım
İADE Kart
BİTİR