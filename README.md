# STM32 FreeRTOS Practices

Bu proje, STM32F446RE (Nucleo-F446RE) geliştirme kartı üzerinde SAF FreeRTOS C API'leri kullanarak geliştirdiğim pratik çalışmaları ve RTOS mimari uygulamalarını içerir.

---

## Donanım ve Yazılım Gereksinimleri

Geliştirme Kartım: STM32F446RE (Nucleo-64)
IDE: STM32CubeIDE
Kütüphane: STM32Cube HAL & FreeRTOS
Kullandığım Terminaller: PuTTY / Tera Term (Serial Baud Rate: 115200)

---

##  Uygulanan Görevler (Tasks)

 projenin gelişim aşamaları ve içerdiği Task'lar:

### 1. Task_LED (Tekli LED Yanıp Sönme)
Açıklama: Saf C FreeRTOS API'si (vTaskDelay) kullanılarak oluşturulan ilk görev. 
Çalışma Mantığı: On-board LED'i (PA5) 500 ms aralıklarla toggle eder.

### 2. Task_UART (Periyodik Serial Loglama)
Açıklama: Seri port üzerinden bilgisayara periyodik olarak canlı durum logu gönderir.
Çalışma Mantığı: sprintf ve HAL_UART_Transmit kullanılarak her 1 saniyede bir sayaç değerini PuTTY terminaline basar.

### 3. Dinamik Öncelik Yönetimi (Task Priorities & vTaskPrioritySet)
Açıklama: RTOS üzerinde çalışan görevlerin öncelik (Priority) seviyelerinin incelenmesi ve çalışma zamanında değiştirilmesi.
Çalışma Mantığı: TaskUARTHigh (High Priority) ve TaskLEDLow (Low Priority) görevleri tanımladım. vTaskPrioritySet kullanıaarak sayaç modülüne göre LED görevine dinamik olarak HIGH ve LOW öncelik atadım işlemci kaynaklarının öncelikli göreve devredilmesini gözlemledim.

### 4. Görevler Arası Haberleşme Queue (Kuyruk) Yapısı
Açıklama: İplik güvenli veri aktarımı amacıyla Saf FreeRTOS C APIleri ile 5 elemanlı bir Queue yapısı oluşturdum.
Üretici (Producer - TaskLEDLow): Sayacı her 500 ms'de 1 artırır LED durumunu değiştirir ve veriyi xQueueSend ile kuyruğa kopyalar.
Tüketici (Consumer - TaskUARTHigh): xQueueReceive fonksiyonunda portMAX_DELAY parametresi ile kuyruğa yeni veri düşene kadar işlemciyi meşgul etmeden BLOCKED durumunda bekler. Veri geldiği an uyanarak seri port üzerinden bilgisayara aktarır.

---

##  Proje Yapısı

Core/Src/main.c: RTOS Task tanımlamaları ve ana döngü kodları.
Core/Inc/: Başlık dosyaları.
Middlewares/Third_Party/FreeRTOS/: FreeRTOS çekirdek dosyaları.

---

## Nasıl Çalıştırılır?

1. Bu depoyu klonlayın veya indirin:
   ```bash
   git clone [https://github.com/batucodeng/STM32_FreeRTOS_Practices.git](https://github.com/batucodeng/STM32_FreeRTOS_Practices.git)
