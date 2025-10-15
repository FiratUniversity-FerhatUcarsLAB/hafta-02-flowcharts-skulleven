BAŞLAT

  // --- SİSTEMİN İLK AYARLARI (INITIALIZATION) ---
  İŞLEV SistemAyarla():
    SensörleriEtkinleştir()      // Kapı/pencere sensörleri, hareket dedektörleri, kameralar vb.
    AlarmSisteminiKur()         // Siren, ışıklar vb.
    BildirimServisiniBaşlat()   // E-posta, SMS veya mobil uygulama bildirimi için bağlantı kur
    YAZ "Sistem başlatıldı ve izleme modunda."
  BİTİR İŞLEV


  // --- ANA ÇALIŞMA DÖNGÜSÜ (7/24 ÇALIŞIR) ---
  ANA_DÖNGÜ (Sonsuz Döngü):

    // 1. ADIM: SENSÖR OKUMA
    // Tüm bağlı sensörlerden anlık verileri topla.
    sensor_verileri = SensorleriOku() 
    // Örnek veri: {kapi_sensoru: "kapali", hareket_sensoru: "hareket_yok", kamera: "normal_goruntu"}

    // 2. ADIM: TEHDİT ALGILAMA
    // Toplanan sensör verilerini analiz ederek bir anormallik olup olmadığını kontrol et.
    EĞER veriler.kapi_sensoru == "açık" VEYA veriler.hareket_sensoru == "hareket_var" VEYA veriler.kamera == "tespit_edildi":
    EĞER veriler.kamera_yuz_tespidi() == "ev sahibi" YAP
        ÇIK
    EĞER veriler.kapi_sensoru == "açık" YAP
        alarm_durumu += 1
    EĞER verlier.hareket_sensoru == "hareket_var" YAP
        alarm_durumu += 1
    EĞER veriler.kamera == "tespit_edildi" YAP
        alarm_durumu += 1
    tehdit_durumu = "TEHDİT_ALGILANDI"
    DEĞİLSE:
        tehdit_durumu "GÜVENLİ"
    BİTİR EĞER
    // Örnek tehdit: Kapı sensörü "açık" veya hareket sensörü "hareket_var" olarak raporlarsa.

    // 3. ADIM: ALARM VERME VE BİLDİRİM GÖNDERME
    // Eğer bir tehdit algılandıysa ilgili eylemleri gerçekleştir.
    EĞER tehdit_durumu == "TEHDİT_ALGILANDI":
    BildirimGonder(kullanici, mesaj)
    EĞER kullanici tanıyorum olarak cevap verirse YAP
        EKLE kişi
        bildirim.durdur(1) // tanıyorum olara işaretlerse gün boyu kişi hakkında bildirim gönderilmeyecek
      // 3b. BİLDİRİM GÖNDERME
      // Kullanıcıya daha önce bildirim gönderilmediyse gönder.
      EĞER BildirimGonderildiMi() == "HAYIR":
        mesaj = "UYARI: Evinizde bir tehdit algılandı. Algılanan sensör: " + tehdit_durumu.kaynak
        BildirimGonder(kullanici, mesaj)
        SMSGonder(kullanici, mesaj)
        MailGonder(kullanici, mesaj)
        YAZ "Kullanıcıya bildirim gönderildi."
      BİTİR EĞER
    
      EĞER alarm_durumu == 1 YAP
        // 3a. ALARM VERME
        // Sistem zaten alarm durumunda değilse alarmı tetikle.
        EĞER AlarmDurumu() == "PASİF":
            AlarmCal() // Sireni ve ışıkları etkinleştir.
            YAZ "ALARM! Tehdit algılandı, sirenler ve ışıklar aktive edildi."
        BİTİR EĞER
      EĞER alarm_durumu == 2 YAP
        kapi_pencere_kilitle()
      EĞER alarm_durumu == 3 YAP
        polisi_ara()

    DEĞİLSE:
      // Tehdit yoksa her şeyin normal olduğundan emin ol.
      alarm.dur()
      alarm_durumu = 0
      ResetleAlarmVeBildirimDurumu()
      YAZ "Durum normal, tehdit yok."
    BİTİR EĞER

    // Sistemin kaynakları aşırı tüketmemesi için kısa bir süre bekle.
    Bekle(1 saniye) 

  BİTİR ANA_DÖNGÜ

SON
