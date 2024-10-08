---
description: Berisi catatan singkat terkati istilah dan masalah pada OLT
---

# OLT

## Indikator OLT

Alarms terkait ODN (Optical Distribution Network) mengacu pada masalah yang terjadi dalam jaringan distribusi optik, terutama antara OLT (Optical Line Terminal) dan ONT (Optical Network Terminal). Alarms ini mengindikasikan adanya gangguan atau anomali dalam komunikasi optik, yang dapat disebabkan oleh masalah perangkat keras, sinyal, atau bahkan kabel serat optik yang terputus. Mari kita uraikan maksud dari setiap alarm beserta contoh kasusnya:

#### 1. **0x2e112002: Loss of GEM Channel Delineation (LCDGi) Occurs**

**Deskripsi:** GEM (GPON Encapsulation Method) channel delineation hilang. Hal ini berarti ada masalah dalam penguraian saluran GEM yang digunakan untuk mengirimkan data antara OLT dan ONT.

**Contoh kasus:** Misalkan dalam sebuah jaringan GPON, beberapa pelanggan melaporkan gangguan internet secara tiba-tiba. Setelah dicek, terdeteksi alarm **0x2e112002** pada ONT pelanggan. Ini menunjukkan bahwa ada masalah pada jalur data GEM yang menyebabkan data tidak dapat diterima dengan benar.

***

#### 2. **0x2e112003: Signal Degrade of ONTi (SDi) Occurs**

**Deskripsi:** Terjadi degradasi sinyal di ONT. Degradasi ini menunjukkan bahwa kualitas sinyal optik yang diterima ONT menurun di bawah ambang batas tertentu, meskipun sinyal masih ada.

**Contoh kasus:** Seorang teknisi menemukan bahwa salah satu ONT pelanggan mengalami penurunan kecepatan internet. Alarm **0x2e112003** muncul, mengindikasikan bahwa meskipun sinyal masih ada, kualitasnya sudah menurun, mungkin disebabkan oleh redaman yang tinggi pada kabel optik atau kotoran di konektor optik.

***

#### 3. **0x2e112004: Signal Fail of ONTi (SFi) Occurs**

**Deskripsi:** Sinyal di ONT hilang sama sekali. Ini merupakan alarm yang lebih serius dibandingkan degradasi sinyal, karena menunjukkan bahwa ONT tidak dapat menerima sinyal optik sama sekali.

**Contoh kasus:** Jika sebuah ONT di rumah pelanggan tiba-tiba kehilangan semua koneksi internet dan alarm **0x2e112004** muncul, ini bisa menunjukkan bahwa ada gangguan fisik pada kabel serat optik, seperti kabel yang putus atau koneksi optik yang terlepas.

***

#### 4. **0x2e112006: Loss of Frame of ONTi (LOFi) Occurs**

**Deskripsi:** Terjadi kehilangan frame di ONT. Frame di sini mengacu pada unit data optik yang dikirim antara OLT dan ONT. Kehilangan frame dapat menyebabkan data tidak dapat diproses dengan benar.

**Contoh kasus:** Pelanggan mengeluh bahwa koneksi internet mereka tidak stabil. Setelah dilakukan pengecekan, alarm **0x2e112006** terdeteksi di OLT. Ini menunjukkan bahwa ONT kehilangan frame data, mungkin karena gangguan optik yang sporadis, seperti interferensi atau kabel yang tidak terhubung dengan baik.

***

#### 5. **0x2e11a001: The Feed Fiber is Broken or OLT Cannot Receive Any Expected Optical Signals (LOS)**

**Deskripsi:** Fiber optik utama (feed fiber) yang menghubungkan OLT ke jaringan distribusi terputus, atau OLT tidak dapat menerima sinyal optik yang diharapkan (Loss of Signal - LOS).

**Contoh kasus:** Jaringan di seluruh lingkungan tiba-tiba mengalami gangguan besar. Alarm **0x2e11a001** muncul, menunjukkan bahwa kabel serat optik utama yang menghubungkan OLT ke jaringan backbone terputus, mungkin karena penggalian jalan yang tidak disengaja.

***

#### 6. **0x2e112007: The Distribute Fiber is Broken or the OLT Cannot Receive Expected Optical Signals from the ONT (LOSi/LOBi)**

**Deskripsi:** Serat optik distribusi (distribute fiber) yang menghubungkan OLT ke ONT terputus, atau OLT tidak dapat menerima sinyal optik yang diharapkan dari ONT (Loss of Signal Indicator - LOSi).

**Contoh kasus:** Seorang pelanggan di lokasi tertentu kehilangan koneksi internet. Alarm **0x2e112007** menunjukkan bahwa kabel serat optik distribusi yang menghubungkan OLT ke rumah pelanggan mungkin terputus karena kabel tersebut tertarik atau rusak secara fisik.

***

#### 7. **0x2e314021: There are Illegal Incursionary Rogue ONTs Under the Port**

**Deskripsi:** OLT mendeteksi adanya ONT "rogue" (ONT liar) yang mencoba terhubung ke port jaringan. ONT rogue adalah perangkat ONT yang tidak sah atau berperilaku buruk, yang dapat menyebabkan gangguan pada jaringan GPON.

**Contoh kasus:** Seorang administrator jaringan mendeteksi alarm **0x2e314021** yang menunjukkan adanya ONT rogue di jaringan. ONT rogue ini mungkin merupakan perangkat ONT yang tidak diotorisasi yang dipasang oleh pihak ketiga tanpa izin.

***

#### 8. **0x2e314022: The ONT is a Rogue ONT**

**Deskripsi:** ONT yang bersangkutan dikategorikan sebagai ONT rogue. Ini bisa terjadi jika ONT berperilaku tidak normal atau menyebabkan interferensi di jaringan.

**Contoh kasus:** Setelah dilakukan pemantauan, alarm **0x2e314022** muncul pada salah satu ONT. Ini bisa berarti bahwa perangkat ONT tersebut terinfeksi malware atau berusaha mengirimkan lalu lintas yang tidak sah, sehingga dianggap sebagai ONT rogue oleh OLT.

***

#### Kesimpulan:

Alarms ini membantu dalam memonitor dan menjaga kualitas serta stabilitas jaringan GPON. Dengan memahami setiap alarm, teknisi dapat mendiagnosis masalah dengan lebih efektif dan cepat mengambil tindakan yang tepat.
