
*** Crypto Trading Strategy Backtest (Time Series Analysis)

Bu projede, kripto para piyasasına ait zaman serisi verileri kullanılarak teknik göstergelere dayalı bir alım-satım stratejisinin backtest edilmesi amaçlanmıştır.


1. Proje Genel Bakış

    Finansal piyasalarda geliştirilen stratejilerin gerçek performansını değerlendirebilmek için doğru bir backtest yaklaşımı kritik öneme sahiptir.

    Bu projede, Aralık 2025 – Şubat 2026 dönemine ait, 5 dakikalık frekansta oluşturulmuş 10 farklı kripto para birimine ait fiyat ve hacim verileri kullanılmıştır.

    Veri seti aşağıdaki bileşenleri içermektedir:

        - Gerçek piyasa fiyatları (normal_open, normal_close vb.)
        - Heiken Ashi dönüşümü ile elde edilen fiyatlar
        - Teknik indikatörler (RSI, SMA, EMA, MACD, trend vb.)

    Amaç, geçmiş veriler üzerinden işlem sinyalleri üretmek ve bu sinyallere göre oluşturulan işlemlerin performansını analiz etmektir.


2. Veri Hazırlama ve Feature Engineering

    - Zaman serisi verisi "symbol" ve "timestamp" bazında sıralanmıştır
    - Teknik göstergelerin geçmiş değerleri ("lag") oluşturulmuştur
    - Bu sayede veri sızıntısı (data leakage) engellenmiştir

    Örnek kullanılan değişkenler:

        - RSI (Relative Strength Index)
        - SMA200 (200 periyotluk hareketli ortalama)
        - Trend göstergeleri
        - Fiyat bazlı türetilmiş değişkenler


3. Strateji ve Sinyal Üretimi

    Strateji, teknik göstergelerin geçmiş değerlerine göre oluşturulmuştur:

    - LONG (1):  
    RSI > 50, trend pozitif ve fiyat SMA200 üzerinde

    - SHORT (-1):  
    RSI < 50, trend negatif ve fiyat SMA200 altında

    - Diğer durumlar:  
    Pozisyon alınmaz (0)

    Sinyal üretiminde yalnızca geçmiş veriler kullanılmıştır.


4. Backtest Mantığı

    Backtest süreci aşağıdaki adımlarla gerçekleştirilmiştir:

        1. Sinyal değişimlerine göre entry (giriş) ve exit (çıkış) noktaları belirlenmiştir  
        2. Her entry, kendisinden sonra gelen ilk exit ile eşleştirilmiştir  
        3. İşlemler coin bazlı (symbol) ayrı ayrı değerlendirilmiştir  
        4. Trade bazlı bir veri seti oluşturulmuştur  

    Önemli bir nokta:

        - Sinyaller Heiken Ashi verilerinden üretilmiştir  
        - Ancak işlemler gerçek piyasa fiyatları ("normal_open") ile simüle edilmiştir  

    Bu ayrım, backtest sonuçlarının daha gerçekçi olmasını sağlamaktadır.


5. Getiri Hesaplama

    Her işlem için getiri hesaplaması:

    - LONG: (exit / entry - 1)

    - SHORT: (1 - exit / entry)

    Her işlem için %0.1 işlem maliyeti (fee) düşülerek net getiri elde edilmiştir.


6. Performans Metrikleri

    Strateji performansı aşağıdaki metrikler ile değerlendirilmiştir:

        - Ortalama getiri (Average Return)
        - Kazanma oranı (Win Rate)
        - Minimum ve maksimum getiri
        - Long vs Short performans karşılaştırması
        - Kümülatif getiri (simple sum)
        - Equity curve (compounded)


7. Görselleştirme

    Strateji performansı iki farklı şekilde görselleştirilmiştir:

        - Cumulative Return (Simple Sum): Trade sonuçlarının toplamsal etkisini gösterir

        - Equity Curve (Compounded): Sermayenin zaman içindeki gerçek büyümesini simüle eder


8. Sonuç

    Elde edilen sonuçlar incelendiğinde, stratejinin ortalama getirisinin negatif olduğu ve kazanma oranının düşük seviyelerde kaldığı görülmektedir.

    Kümülatif getiri ve equity eğrileri, stratejinin zaman içerisinde sürdürülebilir bir performans sergilemediğini göstermektedir.

    Bu sonuç, finansal piyasalarda basit teknik kural setleri ile kalıcı bir avantaj (edge) elde etmenin zor olduğunu ortaya koymaktadır.

    Ayrıca, doğru bir backtest sürecinde:

        - Gerçek fiyat kullanımı
        - İşlem maliyetlerinin dahil edilmesi
        - Veri sızıntısının engellenmesi

    gibi unsurların sonuçları ciddi şekilde etkilediği gözlemlenmiştir.

    Bu çalışma, finansal zaman serileri üzerinde güvenilir bir backtest altyapısının nasıl kurulacağını göstermektedir.
