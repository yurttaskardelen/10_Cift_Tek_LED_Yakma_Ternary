# 10_Cift_Tek_LED_Algoritmik_Yontem (Bitwise Logic)

Bu proje, **STM32F407-Discovery** kartı üzerinde 4 adet LED kullanarak **çift/tek flaşör** (alternatif yanıp sönme) animasyonu gerçekleştirir.

Bu proje, aynı görsel efekti gerçekleştiren ancak farklı bir kodlama yaklaşımı kullanan **➡️ **[09_Cift_Tek_LED_Yakma (Flaşör Efekti)](https://github.com/yurttaskardelen/09_Cift_Tek_LED_Yakma)**** optimize edilmiş versiyonudur.

---

### 🎯 Proje Senaryosu ve Mantığı

Kod, `while(1)` döngüsü içinde iki ana `for` döngüsü çalıştırır. Burada kritik nokta **Ternary Operatörü (`? :`)** ve **Bitwise AND (`&`)** operatörünün birleşimidir.

**Mantık: `(i & 1)` Nedir?**
Bu işlem, sayının son bitine bakar.
* Sayı Çift ise (0, 2, 4...) -> Sonuç **0** olur.
* Sayı Tek ise (1, 3, 5...) -> Sonuç **1** olur.

**Akış:**

1.  **Aşama 1 (Çift İndeksler Yansın):**
    * Döngü 0'dan 3'e kadar döner.
    * `(i & 1) == 0` şartı kontrol edilir.
    * Eğer indeks **çift ise** (`0` ve `2` yani `PA1` ve `PA3`) -> **SET** (Yanar).
    * Eğer indeks **tek ise** (`1` ve `3` yani `PA2` ve `PA4`) -> **RESET** (Söner).
    * *Sonuç: PA1 ve PA3 yanar, diğerleri söner.*

2.  **Aşama 2 (Tek İndeksler Yansın):**
    * Döngü tekrar 0'dan 3'e kadar döner.
    * `(i & 1) == 1` şartı kontrol edilir.
    * Eğer indeks **tek ise** (`1` ve `3` yani `PA2` ve `PA4`) -> **SET** (Yanar).
    * Eğer indeks **çift ise** (`0` ve `2` yani `PA1` ve `PA3`) -> **RESET** (Söner).
    * *Sonuç: PA2 ve PA4 yanar, diğerleri söner.*

**Zamanlama:** Her değişim arasında 1000 ms (1 saniye) bekleme vardır.

---

### 🛠️ Gerekli Donanım

* **1x** STM32F407-Discovery Geliştirme Kartı
* **4x** Tercih edilen renklerde LED
* **4x** 220 ya da 330 Ohm Direnç (LED'ler için ön direnç)
* Breadboard ve Jumper kablolar

---

### 🔌 Devre Şeması

LED'lerin anot (uzun) bacakları STM32 pinlerine, katot (kısa) bacakları ise direnç üzerinden GND hattına bağlanmalıdır.

| LED | Direnç | STM32 Pini |
| :--- | :--- | :--- |
| LED 1 | 220 Ohm | `PA1` |
| LED 2 | 220 Ohm | `PA2` |
| LED 3 | 220 Ohm | `PA3` |
| LED 4 | 220 Ohm | `PA4` |
| (Tümü) | - | `GND` |

<img width="473" height="404" alt="Pin_Baglantilari" src="https://github.com/user-attachments/assets/2faf879d-af80-4f97-9495-9c89e4afac5b" />

### Kod Bloğu

<img width="1213" height="481" alt="image" src="https://github.com/user-attachments/assets/bd1d7c76-001e-48ac-bd30-78c295e0bb45" />

---

### 🚀 Nasıl Kullanılır?

1.  Bu depoyu klonlayın (`git clone ...`).
2.  STM32CubeIDE yazılımını açın.
3.  `File > Open Projects from File System...` seçeneği ile proje klasörünü seçin.
4.  Proje içindeki `.ioc` dosyasını açarak pin yapılandırmasını inceleyebilirsiniz.
5.  Derleyin (Build) ve ST-Link V2 üzerinden kartınıza yükleyin (Run).
