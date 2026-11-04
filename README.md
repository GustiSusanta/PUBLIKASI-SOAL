import React, { useState, useEffect } from 'react';
import {
  Clock, CheckCircle, HelpCircle, AlertCircle, User, Award, 
  ChevronLeft, ChevronRight, LogOut, Download, Eye, FileText,
  BarChart2, RefreshCw, Layers, Check, X, ShieldAlert, BookOpen
} from 'lucide-react';

// --- DATA SOAL COMPLETE 50 QUESTIONS & EXPLANATIONS ---
const QUESTIONS = [
  {
    id: 1,
    text: "Besaran berikut yang nilainya dipengaruhi oleh arah perpindahan objek (besaran vektor) dalam gerak lurus adalah ...",
    options: ["Jarak dan kelajuan", "Kelajuan dan waktu", "Perpindahan dan kecepatan", "Jarak dan kecepatan", "Kelajuan dan percepatan"],
    answer: 2,
    hots: false,
    explanation: "Perpindahan dan kecepatan memiliki arah sehingga dikategorikan sebagai besaran vektor. Sedangkan jarak dan kelajuan hanya memiliki nilai (skalar)."
  },
  {
    id: 2,
    text: "Seorang siswa berjalan 80 meter ke arah timur di lapangan SMAN 1 Gianyar, lalu berbalik arah ke barat sejauh 30 meter. Jarak total dan perpindahan siswa tersebut adalah ...",
    options: ["110 m dan 50 m", "50 m dan 110 m", "110 m dan 110 m", "50 m dan 50 m", "80 m dan 30 m"],
    answer: 0,
    hots: false,
    explanation: "Jarak = 80 m + 30 m = 110 m. Perpindahan = 80 m - 30 m = 50 m (ke timur)."
  },
  {
    id: 3,
    text: "Sebuah mobil bergerak lurus beraturan (GLB) dengan kecepatan konstan 18 m/s. Jarak yang ditempuh mobil selama selang waktu 20 sekon adalah ...",
    options: ["180 m", "240 m", "360 m", "420 m", "540 m"],
    answer: 2,
    hots: false,
    explanation: "Pada GLB: s = v × t = 18 m/s × 20 s = 360 meter."
  },
  {
    id: 4,
    text: "Dua kendaraan A dan B bergerak dari titik awal yang sama pada lintasan lurus. Kecepatan A konstan 25 m/s, sedangkan B konstan 15 m/s. Jarak antara kendaraan A dan B setelah bergerak selama 45 sekon adalah ...",
    options: ["300 m", "450 m", "600 m", "675 m", "1.125 m"],
    answer: 1,
    hots: true,
    explanation: "Jarak pisah Δs = (vA - vB) × t = (25 - 15) m/s × 45 s = 10 × 45 = 450 meter."
  },
  {
    id: 5,
    text: "Sepeda motor melaju dari keadaan diam dan mengalami percepatan konstan 3 m/s². Kecepatan sepeda motor setelah 6 sekon bergerak adalah ...",
    options: ["9 m/s", "12 m/s", "18 m/s", "24 m/s", "36 m/s"],
    answer: 2,
    hots: false,
    explanation: "vt = v0 + a × t = 0 + (3 × 6) = 18 m/s."
  },
  {
    id: 6,
    text: "Jarak yang ditempuh sepeda motor pada soal nomor 5 selama selang waktu 6 sekon tersebut adalah ...",
    options: ["18 m", "36 m", "54 m", "72 m", "108 m"],
    answer: 2,
    hots: false,
    explanation: "s = v0·t + ½a·t² = 0 + ½(3)(6²) = ½ × 3 × 36 = 54 meter."
  },
  {
    id: 7,
    text: "Perhatikan grafik v - t berikut yang menggambarkan gerak sebuah partikel pada lintasan lurus:",
    options: ["80 m", "120 m", "160 m", "200 m", "320 m"],
    answer: 2,
    hots: false,
    svgType: "graph_vt_rect",
    explanation: "Jarak = Luas daerah di bawah kurva v-t. s = v × t = 16 m/s × 10 s = 160 meter."
  },
  {
    id: 8,
    text: "Sebuah batu dilepaskan dari jembatan tanpa kecepatan awal (gerak jatuh bebas) dan menyentuh dasar sungai dalam waktu 3 sekon. Jika g = 10 m/s², tinggi jembatan dari permukaan sungai adalah ...",
    options: ["15 m", "30 m", "45 m", "60 m", "90 m"],
    answer: 2,
    hots: false,
    explanation: "h = ½·g·t² = ½(10)(3²) = 5 × 9 = 45 meter."
  },
  {
    id: 9,
    text: "Kecepatan batu pada soal nomor 8 sesaat sebelum menyentuh permukaan air sungai adalah ...",
    options: ["15 m/s", "30 m/s", "45 m/s", "60 m/s", "90 m/s"],
    answer: 1,
    hots: false,
    explanation: "vt = g × t = 10 m/s² × 3 s = 30 m/s."
  },
  {
    id: 10,
    text: "Mobil A bergerak dari Gianyar menuju Denpasar dengan kecepatan konstan 20 m/s. Pada waktu yang sama, mobil B bergerak dari Denpasar menuju Gianyar di jalur lurus yang sama dengan kecepatan konstan 30 m/s. Jika jarak Gianyar-Denpasar adalah 25 km, kedua mobil berpapasan setelah bergerak selama ...",
    options: ["250 s", "400 s", "500 s", "625 s", "1.000 s"],
    answer: 2,
    hots: true,
    explanation: "sA + sB = S_total → (vA + vB)t = 25.000 → (20 + 30)t = 25.000 → 50t = 25.000 → t = 500 sekon."
  },
  {
    id: 11,
    text: "Sebuah bola dilempar vertikal ke atas dengan kecepatan awal 40 m/s dari tanah. Jika percepatan gravitasi g = 10 m/s², waktu yang dibutuhkan bola untuk mencapai titik tertinggi adalah ...",
    options: ["2 s", "4 s", "6 s", "8 s", "10 s"],
    answer: 1,
    hots: false,
    explanation: "Di titik puncak vt = 0. t_puncak = v0 / g = 40 / 10 = 4 sekon."
  },
  {
    id: 12,
    text: "Tinggi maksimum yang dapat dicapai oleh bola pada soal nomor 11 adalah ...",
    options: ["40 m", "80 m", "120 m", "160 m", "200 m"],
    answer: 1,
    hots: false,
    explanation: "h_max = v0² / (2g) = 40² / (2 × 10) = 1600 / 20 = 80 meter."
  },
  {
    id: 13,
    text: "Sebuah sedan melaju di jalan raya dengan kecepatan 30 m/s. Pengemudi melihat rintangan di depan dan menginjak rem secara mendadak sehingga mengalami perlambatan konstan 6 m/s². Jarak minimum yang dibutuhkan sedan hingga berhenti total adalah ...",
    options: ["45 m", "60 m", "75 m", "90 m", "150 m"],
    answer: 2,
    hots: true,
    explanation: "vt² = v0² - 2as → 0 = 30² - 2(6)s → 12s = 900 → s = 75 meter."
  },
  {
    id: 14,
    text: "Perhatikan pita ketukan ticker timer hasil percobaan troli bergerak ke kanan berikut:",
    options: ["Gerak Lurus Beraturan (GLB)", "GLBB dipercepat", "GLBB diperlambat", "Gerak Vertikal ke Atas", "Gerak Melingkar Beraturan"],
    answer: 1,
    hots: false,
    svgType: "ticker_accelerated",
    explanation: "Karena troli bergerak ke KANAN, titik yang ditinggalkan pertama ada di kiri. Titik makin ke kanan makin renggang, artinya kecepatan bertambah (GLBB dipercepat)."
  },
  {
    id: 15,
    text: "Pernyataan di bawah ini yang benar mengenai karakteristik Gerak Lurus Beraturan (GLB) adalah ...",
    options: ["Kecepatan benda berubah secara konstan", "Percepatan benda bernilai positif tetap", "Percepatan benda bernilai nol", "Jarak tempuh berbanding terbalik dengan waktu", "Lintasan berupa garis melengkung"],
    answer: 2,
    hots: false,
    explanation: "Pada GLB, kecepatan konstan/tetap sehingga percepatan a = 0."
  },
  {
    id: 16,
    text: "Benda P dijatuhkan bebas dari ketinggian H, sedangkan benda Q dijatuhkan bebas dari ketinggian 4H di atas tanah. Perbandingan selang waktu jatuh tP : tQ hingga menyentuh tanah adalah ...",
    options: ["1 : 16", "1 : 4", "1 : 2", "2 : 1", "4 : 1"],
    answer: 2,
    hots: true,
    explanation: "t = √(2h/g) → t berbanding lurus dengan √h. Maka tP : tQ = √H : √(4H) = 1 : 2."
  },
  {
    id: 17,
    text: "Sebuah bola pejal dilempar ke atas dengan kecepatan awal v0. Pada titik tertinggi lintasannya, kondisi fisik bola tersebut adalah ...",
    options: ["Kecepatan = 0 dan percepatan = 0", "Kecepatan = v0 dan percepatan = g (ke bawah)", "Kecepatan = 0 dan percepatan = g (ke bawah)", "Kecepatan = maksimum dan percepatan = 0", "Kecepatan = 0 dan percepatan = g (ke atas)"],
    answer: 2,
    hots: false,
    explanation: "Di puncak, kecepatan benda sesaat bernilai nol (v = 0), namun gaya gravitasi bumi tetap bekerja sehingga percepatan konstan sebesar g bernilai ke bawah."
  },
  {
    id: 18,
    text: "Gambar berikut memperlihatkan grafik hubungan posisi (s) terhadap waktu (t) dari gerak suatu benda:",
    options: ["4 m/s", "5 m/s", "6.5 m/s", "8 m/s", "10 m/s"],
    answer: 1,
    hots: false,
    svgType: "graph_st",
    explanation: "v = Δs / Δt = (52 - 12) / (8 - 0) = 40 / 8 = 5 m/s."
  },
  {
    id: 19,
    text: "Persamaan gerak partikel sepanjang sumbu x dinyatakan oleh x(t) = 4t² - 12t + 5 (x dalam meter, t dalam sekon). Partikel akan berhenti bergerak sesaat pada saat t bernilai ...",
    options: ["1,0 s", "1,5 s", "2,0 s", "2,5 s", "3,0 s"],
    answer: 1,
    hots: true,
    explanation: "v(t) = dx/dt = 8t - 12. Berhenti sesaat saat v = 0 → 8t - 12 = 0 → t = 1,5 sekon."
  },
  {
    id: 20,
    text: "Berdasarkan persamaan pada soal nomor 19, percepatan konstan yang dialami oleh partikel tersebut sebesar ...",
    options: ["2 m/s²", "4 m/s²", "6 m/s²", "8 m/s²", "12 m/s²"],
    answer: 3,
    hots: false,
    explanation: "a(t) = dv/dt = d(8t - 12)/dt = 8 m/s²."
  },
  {
    id: 21,
    text: "Sebuah apel jatuh dari pohon setinggi 5 meter di atas permukaan tanah. Waktu yang dibutuhkan apel hingga jatuh menyentuh tanah adalah ... (g = 10 m/s²)",
    options: ["0,5 s", "1,0 s", "1,5 s", "2,0 s", "2,5 s"],
    answer: 1,
    hots: false,
    explanation: "t = √(2h/g) = √(2 × 5 / 10) = √1 = 1,0 sekon."
  },
  {
    id: 22,
    text: "Dari puncak menara setinggi 75 m, batu dilemparkan vertikal ke atas dengan kecepatan awal 10 m/s. Selang waktu hingga batu tiba di tanah dasar menara adalah ... (g = 10 m/s²)",
    options: ["3 s", "4 s", "5 s", "6 s", "8 s"],
    answer: 2,
    hots: true,
    explanation: "y = y0 + v0·t - ½g·t² → 0 = 75 + 10t - 5t² → t² - 2t - 15 = 0 → (t - 5)(t + 3) = 0 → t = 5 sekon."
  },
  {
    id: 23,
    text: "Grafik v - t berikut menunjukkan tahapan gerak sebuah bus kota:",
    options: ["160 m", "200 m", "240 m", "280 m", "320 m"],
    answer: 1,
    hots: false,
    svgType: "graph_vt_trapezoid",
    explanation: "Jarak = Luas Trapesium = ½ × (Jumlah Sisi Sejajar) × Tinggi = ½ × (14 + 6) × 20 = ½ × 20 × 20 = 200 meter."
  },
  {
    id: 24,
    text: "Berdasarkan grafik nomor 23, percepatan bus pada selang t = 0 sampai t = 4 sekon adalah ...",
    options: ["2 m/s²", "4 m/s²", "5 m/s²", "10 m/s²", "20 m/s²"],
    answer: 2,
    hots: false,
    explanation: "a = (v2 - v1) / t = (20 - 0) / 4 = 5 m/s²."
  },
  {
    id: 25,
    text: "Benda A dilempar vertikal ke atas dengan kecepatan awal 50 m/s dari tanah. Pada saat bersamaan, benda B dijatuhkan dari ketinggian 150 m tepat di atas A. Kedua benda akan bertemu di udara pada ketinggian ... (g = 10 m/s²)",
    options: ["45 m", "60 m", "75 m", "105 m", "120 m"],
    answer: 3,
    hots: true,
    explanation: "t_temu = H / (v0A + v0B) = 150 / (50 + 0) = 3 sekon. Ketinggian temu hA = v0A·t - ½gt² = 50(3) - 5(9) = 150 - 45 = 105 meter."
  },
  {
    id: 26,
    text: "Kecepatan awal sebuah benda GLBB adalah 10 m/s. Setelah bergerak selama 4 s, kecepatannya menjadi 26 m/s. Percepatan benda tersebut adalah ...",
    options: ["3 m/s²", "4 m/s²", "5 m/s²", "6 m/s²", "8 m/s²"],
    answer: 1,
    hots: false,
    explanation: "a = (vt - v0) / t = (26 - 10) / 4 = 16 / 4 = 4 m/s²."
  },
  {
    id: 27,
    text: "Jarak yang ditempuh oleh benda pada soal nomor 26 selama 4 sekon tersebut sebesar ...",
    options: ["52 m", "64 m", "72 m", "80 m", "104 m"],
    answer: 2,
    hots: false,
    explanation: "s = ½(v0 + vt)t = ½(10 + 26) × 4 = 36 × 2 = 72 meter."
  },
  {
    id: 28,
    text: "Sebuah benda jatuh bebas dari ketinggian H di atas permukaan tanah. Jarak yang ditempuh benda khusus pada detik ke-3 pergerakannya saja adalah ... (g = 10 m/s²)",
    options: ["15 m", "20 m", "25 m", "30 m", "45 m"],
    answer: 2,
    hots: true,
    explanation: "s(t=3) = ½(10)(3²) = 45m. s(t=2) = ½(10)(2²) = 20m. Jarak detik ke-3 = 45m - 20m = 25 meter."
  },
  {
    id: 29,
    text: "Sepeda bergerak dengan kelajuan awal 15 m/s direm dengan perlambatan konstan 3 m/s². Waktu yang diperlukan hingga sepeda berhenti adalah ...",
    options: ["3 s", "4 s", "5 s", "6 s", "10 s"],
    answer: 2,
    hots: false,
    explanation: "t = v0 / a = 15 / 3 = 5 sekon."
  },
  {
    id: 30,
    text: "Perhatikan grafik v - t untuk peristiwa pengereman mobil di bawah ini:",
    options: ["2 m/s²", "4 m/s²", "5 m/s²", "10 m/s²", "25 m/s²"],
    answer: 2,
    hots: false,
    svgType: "graph_brake",
    explanation: "Perlambatan a = |Δv / Δt| = |(0 - 25) / 5| = 5 m/s²."
  },
  {
    id: 31,
    text: "Dua batu A dan B jatuh bebas dari posisi diam masing-masing pada waktu tA = 2 sekon dan tB = 6 sekon. Perbandingan kecepatan vA : vB kedua batu saat itu adalah ...",
    options: ["1 : 9", "1 : 3", "1 : 2", "3 : 1", "9 : 1"],
    answer: 1,
    hots: true,
    explanation: "v = g × t → vA : vB = tA : tB = 2 : 6 = 1 : 3."
  },
  {
    id: 32,
    text: "Benda dilempar vertikal ke atas dengan kecepatan awal 20 m/s. Tinggi benda saat kecepatannya tinggal 10 m/s di atas tanah adalah ... (g = 10 m/s²)",
    options: ["5 m", "10 m", "15 m", "20 m", "25 m"],
    answer: 2,
    hots: false,
    explanation: "vt² = v0² - 2gh → 10² = 20² - 2(10)h → 100 = 400 - 20h → 20h = 300 → h = 15 meter."
  },
  {
    id: 33,
    text: "Peluru ditembakkan ke papan kayu dengan kecepatan 40 m/s dan mampu menembus kedalaman 8 cm hingga berhenti. Jika peluru ditembakkan ke kayu sejenis dengan kecepatan 60 m/s, kedalaman penembusan peluru tersebut adalah ...",
    options: ["12 cm", "16 cm", "18 cm", "24 cm", "36 cm"],
    answer: 2,
    hots: true,
    explanation: "v² = 2as → s berbanding lurus v² → s2/s1 = (v2/v1)² = (60/40)² = (3/2)² = 9/4. s2 = 9/4 × 8 cm = 18 cm."
  },
  {
    id: 34,
    text: "Gambar berikut merupakan pita ticker timer dari troli yang bergerak ke arah kanan:",
    options: ["GLB konstan", "GLBB dipercepat", "GLBB diperlambat", "Gerak Vertikal ke Atas", "Gerak Jatuh Bebas"],
    answer: 2,
    hots: false,
    svgType: "ticker_decelerated",
    explanation: "Arah gerak ke KANAN. Jarak antar ketukan semakin ke kanan semakin rapat, menunjukkan kelajuan berkurang (GLBB diperlambat)."
  },
  {
    id: 35,
    text: "Sebuah partikel bergerak lurus dengan lintasan s = 5t² + 2t (s dalam meter, t dalam sekon). Kecepatan rata-rata partikel antara t = 1 s dan t = 3 s adalah ...",
    options: ["12 m/s", "18 m/s", "22 m/s", "26 m/s", "30 m/s"],
    answer: 2,
    hots: false,
    explanation: "s(1) = 5(1) + 2 = 7m. s(3) = 5(9) + 6 = 51m. v_rata = Δs/Δt = (51 - 7)/(3 - 1) = 44/2 = 22 m/s."
  },
  {
    id: 36,
    text: "Perhatikan grafik v - t untuk dua mobil A dan B yang berangkat bersamaan dari tempat sama:",
    options: ["5 sekon", "8 sekon", "10 sekon", "12 sekon", "15 sekon"],
    answer: 2,
    hots: true,
    svgType: "graph_two_cars",
    explanation: "Mobil A menyusul B saat sA = sB → Luas segitiga A = Luas persegi B → ½ × t × vA = vB × t → ½·t(40) = 20·t → 20t = 20t (berpapasan saat luas sama pada t = 10 sekon)."
  },
  {
    id: 37,
    text: "Gerak Jatuh Bebas (GJB) merupakan bentuk khusus dari GLBB yang memiliki ciri khas ...",
    options: ["Kecepatan awal bernilai maksimum", "Kecepatan awal bernilai nol dan percepatannya a = g", "Percepatan geraknya bernilai nol", "Kecepatannya konstan selama bergerak", "Lintasan berupa parabola"],
    answer: 1,
    hots: false,
    explanation: "GJB dialami benda jatuh tanpa dorongan awal (v0 = 0) dan percepatannya melulu disebabkan oleh percepatan gravitasi bumi (a = g)."
  },
  {
    id: 38,
    text: "Sebuah bola kasti dilempar vertikal ke atas hingga mencapai ketinggian maksimum 45 meter. Total waktu bola melayang di udara hingga kembali ke tempat semula adalah ... (g = 10 m/s²)",
    options: ["3 s", "4.5 s", "6 s", "8 s", "9 s"],
    answer: 2,
    hots: true,
    explanation: "h_max = ½gt² → 45 = 5t_naik² → t_naik = 3 sekon. Waktu total udara = 2 × t_naik = 2 × 3 = 6 sekon."
  },
  {
    id: 39,
    text: "Kendaraan bergerak dari A ke B sejauh 60 km dengan kelajuan 30 km/jam, lalu kembali dari B ke A dengan kelajuan 60 km/jam. Kelajuan rata-rata kendaraan untuk seluruh perjalanan tersebut adalah ...",
    options: ["40 km/jam", "45 km/jam", "50 km/jam", "55 km/jam", "60 km/jam"],
    answer: 0,
    hots: true,
    explanation: "t1 = 60/30 = 2 jam. t2 = 60/60 = 1 jam. Total jarak = 120 km, Total waktu = 3 jam. v_rata = 120 / 3 = 40 km/jam."
  },
  {
    id: 40,
    text: "Bola dilempar vertikal ke atas dengan v0 = 35 m/s. Kecepatan bola saat berada pada ketinggian 50 meter di atas tanah adalah ... (g = 10 m/s²)",
    options: ["5 m/s", "10 m/s", "15 m/s", "20 m/s", "25 m/s"],
    answer: 2,
    hots: true,
    explanation: "vt² = v0² - 2gh = 35² - 2(10)(50) = 1225 - 1000 = 225 → vt = √225 = 15 m/s."
  },
  {
    id: 41,
    text: "Sebuah mobil melaju dengan v0 = 15 m/s lalu dipercepat konstan 2 m/s². Kecepatan mobil setelah menempuh jarak 54 m adalah ...",
    options: ["21 m/s", "24 m/s", "25 m/s", "27 m/s", "30 m/s"],
    answer: 0,
    hots: false,
    explanation: "vt² = v0² + 2as = 15² + 2(2)(54) = 225 + 216 = 441 → vt = √441 = 21 m/s."
  },
  {
    id: 42,
    text: "Peluru 1 dan 2 dilempar vertikal ke atas dari permukaan tanah dengan kecepatan awal masing-masing v1 dan v2 = 3 v1. Perbandingan ketinggian maksimum h1 : h2 adalah ...",
    options: ["1 : 3", "1 : 6", "1 : 9", "3 : 1", "9 : 1"],
    answer: 2,
    hots: true,
    explanation: "h_max berbanding lurus v0² → h1 : h2 = v1² : (3v1)² = 1 : 9."
  },
  {
    id: 43,
    text: "Besaran berikut yang nilainya tetap nol sepanjang gerakan berlangsung pada GLB adalah ...",
    options: ["Kecepatan", "Jarak", "Waktu", "Percepatan", "Perpindahan"],
    answer: 3,
    hots: false,
    explanation: "Gerak Lurus Beraturan (GLB) terjadi saat kelajuan konstan sehingga percepatannya a = 0 m/s²."
  },
  {
    id: 44,
    text: "Benda A dijatuhkan bebas dari titik H. Satu sekon kemudian benda B dijatuhkan bebas dari titik yang sama. Selisih jarak antara kedua benda saat benda A bergerak selama 3 sekon adalah ... (g = 10 m/s²)",
    options: ["10 m", "15 m", "20 m", "25 m", "30 m"],
    answer: 3,
    hots: true,
    explanation: "tA = 3s → sA = ½(10)(3²) = 45m. tB = 2s → sB = ½(10)(2²) = 20m. Selisih jarak Δs = 45 - 20 = 25 meter."
  },
  {
    id: 45,
    text: "Grafik percepatan terhadap waktu (a - t) untuk benda yang mengalami Gerak Lurus Beraturan (GLB) berbentuk ...",
    options: ["Garis lurus miring naik ke atas", "Garis lurus horizontal pada sumbu t (a = 0)", "Garis lurus miring turun ke bawah", "Kurva parabola terbuka ke atas", "Garis vertikal sejajar sumbu a"],
    answer: 1,
    hots: false,
    explanation: "Pada GLB, percepatan selalu bernilai 0 sepanjang waktu t, sehingga grafiknya berhimpit sejajar dengan sumbu horizontal t (a = 0)."
  },
  {
    id: 46,
    text: "Sebuah kereta bergerak dari diam dengan a = 2 m/s² selama 8 s, kemudian bergerak GLB selama 12 s, dan direm hingga berhenti dalam 4 s. Jarak total yang ditempuh kereta tersebut adalah ...",
    options: ["192 m", "256 m", "288 m", "320 m", "384 m"],
    answer: 2,
    hots: true,
    explanation: "Phase 1: s1 = ½(2)(8²) = 64m (vt = 16m/s). Phase 2: s2 = 16 × 12 = 192m. Phase 3: s3 = ½(16)(4) = 32m. Total s = 64 + 192 + 32 = 288 meter."
  },
  {
    id: 47,
    text: "Suatu benda dilempar vertikal ke atas dengan v0 = 25 m/s. Waktu yang diperlukan benda untuk mencapai tinggi 20 m saat sedang bergerak naik adalah ... (g = 10 m/s²)",
    options: ["1 s", "2 s", "3 s", "4 s", "5 s"],
    answer: 0,
    hots: false,
    explanation: "h = v0·t - ½gt² → 20 = 25t - 5t² → t² - 5t + 4 = 0 → (t - 1)(t - 4) = 0. Saat naik t = 1 sekon."
  },
  {
    id: 48,
    text: "Pada grafik v - t, perpindahan total benda yang bergerak lurus dapat ditentukan secara matematis melalui ...",
    options: ["Kemiringan (gradien) garis grafik", "Luas daerah di bawah kurva grafik v - t", "Nilai puncak tertinggi pada grafik", "Titik potong garis grafik pada sumbu v", "Hasil kali percepatan dengan kuadrat waktu"],
    answer: 1,
    hots: true,
    explanation: "Luas wilayah di bawah kurva grafik kecepatan terhadap waktu (v-t) mewakili total s (jarak/perpindahan) yang ditempuh."
  },
  {
    id: 49,
    text: "Benda yang dilempar vertikal ke atas akan mengalami gerak ...",
    options: ["GLB saat naik dan GLBB saat turun", "GLBB diperlambat saat naik dan GLBB dipercepat saat turun", "GLBB dipercepat saat naik dan GLBB diperlambat saat turun", "GLB sepanjang seluruh lintasannya", "Gerak melingkar beraturan"],
    answer: 1,
    hots: false,
    explanation: "Saat bergerak naik kecepatan berkurang karena gravitasi (GLBB diperlambat). Saat bergerak turun kecepatan bertambah searah gravitasi (GLBB dipercepat)."
  },
  {
    id: 50,
    text: "Kecepatan bola yang dilempar vertikal ke atas saat kembali lagi ke posisi awal pelemparan nilainya sama dengan ...",
    options: ["Nol", "Setengah kecepatan awal", "Sama besar dengan kecepatan awal tetapi arah berlawanan", "Dua kali kecepatan awal", "Tak terhingga"],
    answer: 2,
    hots: false,
    explanation: "Prinsip konservasi energi kinematika: Besarnya kelajuan saat kembali sama dengan kelajuan awal (|vt| = |v0|), namun arah geraknya berbalik ke bawah (-v0)."
  }
];

// --- SVG RENDERER COMPONENTS ---
const RenderSVGDiagram = ({ type }) => {
  switch (type) {
    case "graph_vt_rect":
      return (
        <svg className="w-full max-w-sm h-32 mx-auto bg-white border border-slate-300 rounded p-2" viewBox="0 0 300 110">
          <line x1="40" y1="90" x2="270" y2="90" stroke="#000" strokeWidth="1.5" />
          <line x1="40" y1="90" x2="40" y2="15" stroke="#000" strokeWidth="1.5" />
          <text x="275" y="94" fontRight="9" fontSize="9" fontFamily="Arial">t (s)</text>
          <text x="35" y="10" fontSize="9" fontFamily="Arial">v (m/s)</text>
          <line x1="40" y1="35" x2="240" y2="35" stroke="#2563eb" strokeWidth="2.5" />
          <line x1="240" y1="35" x2="240" y2="90" stroke="#64748b" strokeDasharray="3,3" />
          <text x="28" y="38" fontSize="9" textAnchor="end" fontFamily="Arial">16</text>
          <text x="35" y="93" fontSize="9" textAnchor="end" fontFamily="Arial">0</text>
          <text x="240" y="102" fontSize="9" textAnchor="middle" fontFamily="Arial">10</text>
        </svg>
      );
    case "ticker_accelerated":
      return (
        <svg className="w-full max-w-sm h-14 mx-auto bg-amber-50 border border-slate-300 rounded p-1" viewBox="0 0 340 45">
          <rect x="10" y="10" width="320" height="25" fill="#fffbeb" stroke="#334155" strokeWidth="1.2" />
          <text x="320" y="26" fontSize="8" fontFamily="Arial" textAnchor="end">Arah gerak ➔</text>
          <circle cx="25" cy="22" r="2.2" fill="#000" />
          <circle cx="40" cy="22" r="2.2" fill="#000" />
          <circle cx="62" cy="22" r="2.2" fill="#000" />
          <circle cx="95" cy="22" r="2.2" fill="#000" />
          <circle cx="140" cy="22" r="2.2" fill="#000" />
          <circle cx="198" cy="22" r="2.2" fill="#000" />
          <circle cx="265" cy="22" r="2.2" fill="#000" />
        </svg>
      );
    case "graph_st":
      return (
        <svg className="w-full max-w-sm h-32 mx-auto bg-white border border-slate-300 rounded p-2" viewBox="0 0 280 100">
          <line x1="35" y1="80" x2="250" y2="80" stroke="#000" strokeWidth="1.5" />
          <line x1="35" y1="80" x2="35" y2="15" stroke="#000" strokeWidth="1.5" />
          <text x="255" y="84" fontSize="9" fontFamily="Arial">t (s)</text>
          <text x="30" y="10" fontSize="9" fontFamily="Arial">s (m)</text>
          <line x1="35" y1="60" x2="210" y2="25" stroke="#16a34a" strokeWidth="2.5" />
          <line x1="35" y1="60" x2="210" y2="60" stroke="#94a3b8" strokeDasharray="3,3" />
          <line x1="210" y1="25" x2="210" y2="80" stroke="#94a3b8" strokeDasharray="3,3" />
          <text x="25" y="63" fontSize="9" textAnchor="end" fontFamily="Arial">12</text>
          <text x="25" y="28" fontSize="9" textAnchor="end" fontFamily="Arial">52</text>
          <text x="210" y="92" fontSize="9" textAnchor="middle" fontFamily="Arial">8</text>
        </svg>
      );
    case "graph_vt_trapezoid":
      return (
        <svg className="w-full max-w-sm h-32 mx-auto bg-white border border-slate-300 rounded p-2" viewBox="0 0 320 110">
          <line x1="35" y1="90" x2="295" y2="90" stroke="#000" strokeWidth="1.5" />
          <line x1="35" y1="90" x2="35" y2="15" stroke="#000" strokeWidth="1.5" />
          <text x="300" y="94" fontSize="9" fontFamily="Arial">t (s)</text>
          <text x="30" y="10" fontSize="9" fontFamily="Arial">v (m/s)</text>
          <polyline points="35,90 115,30 205,30 275,90" fill="none" stroke="#2563eb" strokeWidth="2.5" />
          <line x1="35" y1="30" x2="205" y2="30" stroke="#94a3b8" strokeDasharray="3,3" />
          <line x1="115" y1="30" x2="115" y2="90" stroke="#94a3b8" strokeDasharray="3,3" />
          <line x1="205" y1="30" x2="205" y2="90" stroke="#94a3b8" strokeDasharray="3,3" />
          <line x1="275" y1="30" x2="275" y2="90" stroke="#94a3b8" strokeDasharray="3,3" />
          <text x="25" y="33" fontSize="9" textAnchor="end" fontFamily="Arial">20</text>
          <text x="35" y="101" fontSize="9" textAnchor="middle" fontFamily="Arial">0</text>
          <text x="115" y="101" fontSize="9" textAnchor="middle" fontFamily="Arial">4</text>
          <text x="205" y="101" fontSize="9" textAnchor="middle" fontFamily="Arial">10</text>
          <text x="275" y="101" fontSize="9" textAnchor="middle" fontFamily="Arial">14</text>
        </svg>
      );
    case "graph_brake":
      return (
        <svg className="w-full max-w-sm h-32 mx-auto bg-white border border-slate-300 rounded p-2" viewBox="0 0 300 110">
          <line x1="35" y1="90" x2="270" y2="90" stroke="#000" strokeWidth="1.5" />
          <line x1="35" y1="90" x2="35" y2="15" stroke="#000" strokeWidth="1.5" />
          <text x="275" y="94" fontSize="9" fontFamily="Arial">t (s)</text>
          <text x="30" y="10" fontSize="9" fontFamily="Arial">v (m/s)</text>
          <line x1="35" y1="30" x2="230" y2="90" stroke="#dc2626" strokeWidth="2.5" />
          <line x1="35" y1="30" x2="230" y2="30" stroke="#94a3b8" strokeDasharray="3,3" />
          <line x1="230" y1="30" x2="230" y2="90" stroke="#94a3b8" strokeDasharray="3,3" />
          <text x="25" y="33" fontSize="9" textAnchor="end" fontFamily="Arial">25</text>
          <text x="35" y="101" fontSize="9" textAnchor="middle" fontFamily="Arial">0</text>
          <text x="230" y="101" fontSize="9" textAnchor="middle" fontFamily="Arial">5</text>
        </svg>
      );
    case "ticker_decelerated":
      return (
        <svg className="w-full max-w-sm h-14 mx-auto bg-amber-50 border border-slate-300 rounded p-1" viewBox="0 0 340 45">
          <rect x="10" y="10" width="320" height="25" fill="#fffbeb" stroke="#334155" strokeWidth="1.2" />
          <text x="320" y="26" fontSize="8" fontFamily="Arial" textAnchor="end">Arah gerak ➔</text>
          <circle cx="25" cy="22" r="2.2" fill="#000" />
          <circle cx="85" cy="22" r="2.2" fill="#000" />
          <circle cx="140" cy="22" r="2.2" fill="#000" />
          <circle cx="185" cy="22" r="2.2" fill="#000" />
          <circle cx="220" cy="22" r="2.2" fill="#000" />
          <circle cx="248" cy="22" r="2.2" fill="#000" />
          <circle cx="270" cy="22" r="2.2" fill="#000" />
        </svg>
      );
    case "graph_two_cars":
      return (
        <svg className="w-full max-w-sm h-32 mx-auto bg-white border border-slate-300 rounded p-2" viewBox="0 0 300 110">
          <line x1="35" y1="90" x2="270" y2="90" stroke="#000" strokeWidth="1.5" />
          <line x1="35" y1="90" x2="35" y2="15" stroke="#000" strokeWidth="1.5" />
          <text x="275" y="94" fontSize="9" fontFamily="Arial">t (s)</text>
          <text x="30" y="10" fontSize="9" fontFamily="Arial">v (m/s)</text>
          <line x1="35" y1="90" x2="230" y2="20" stroke="#0284c7" strokeWidth="2" />
          <text x="220" y="18" fontSize="8" fill="#0284c7" fontFamily="Arial">Mobil A</text>
          <line x1="35" y1="55" x2="240" y2="55" stroke="#e11d48" strokeWidth="2" strokeDasharray="4,2" />
          <text x="242" y="58" fontSize="8" fill="#e11d48" fontFamily="Arial">Mobil B</text>
          <line x1="230" y1="20" x2="230" y2="90" stroke="#94a3b8" strokeDasharray="3,3" />
          <text x="25" y="23" fontSize="9" textAnchor="end" fontFamily="Arial">40</text>
          <text x="25" y="58" fontSize="9" textAnchor="end" fontFamily="Arial">20</text>
          <text x="230" y="101" fontSize="9" textAnchor="middle" fontFamily="Arial">10</text>
        </svg>
      );
    default:
      return null;
  }
};

export default function App() {
  // State management
  const [view, setView] = useState('login'); // 'login', 'exam', 'result', 'teacher'
  const [studentInfo, setStudentInfo] = useState({ name: '', classNum: 'X.1', nisn: '', token: '' });
  const [answers, setAnswers] = useState(Array(50).fill(null));
  const [doubts, setDoubts] = useState(Array(50).fill(false));
  const [currentIdx, setCurrentIdx] = useState(0);
  const [timeLeft, setTimeLeft] = useState(120 * 60); // 120 minutes in seconds
  const [isTimerRunning, setIsTimerRunning] = useState(false);
  const [showConfirmSubmit, setShowConfirmSubmit] = useState(false);
  const [scoreData, setScoreData] = useState(null);
  const [savedSubmissions, setSavedSubmissions] = useState([]);

  // Load stored submissions for teacher dashboard
  useEffect(() => {
    const stored = localStorage.getItem('sman1_gianyar_cbt_results');
    if (stored) {
      try {
        setSavedSubmissions(JSON.parse(stored));
      } catch (e) {
        console.error("Failed to parse stored results");
      }
    }
  }, []);

  // Timer Effect
  useEffect(() => {
    let timer;
    if (isTimerRunning && timeLeft > 0) {
      timer = setInterval(() => {
        setTimeLeft((prev) => {
          if (prev <= 1) {
            clearInterval(timer);
            handleAutoSubmit();
            return 0;
          }
          return prev - 1;
        });
      }, 1000);
    }
    return () => clearInterval(timer);
  }, [isTimerRunning, timeLeft]);

  // Format Time
  const formatTime = (seconds) => {
    const hrs = Math.floor(seconds / 3600);
    const mins = Math.floor((seconds % 3600) / 60);
    const secs = seconds % 60;
    return `${hrs.toString().padStart(2, '0')}:${mins.toString().padStart(2, '0')}:${secs.toString().padStart(2, '0')}`;
  };

  // Login handler
  const handleStartExam = (e) => {
    e.preventDefault();
    if (!studentInfo.name || !studentInfo.nisn) {
      alert("Mohon isi Nama Lengkap dan NISN terlebih dahulu!");
      return;
    }
    setIsTimerRunning(true);
    setView('exam');
  };

  // Option select
  const handleSelectOption = (optIdx) => {
    const newAnswers = [...answers];
    newAnswers[currentIdx] = optIdx;
    setAnswers(newAnswers);
  };

  // Toggle doubt
  const handleToggleDoubt = () => {
    const newDoubts = [...doubts];
    newDoubts[currentIdx] = !newDoubts[currentIdx];
    setDoubts(newDoubts);
  };

  // Calculate & Submit
  const handleAutoSubmit = () => {
    setIsTimerRunning(false);
    let correct = 0;
    let wrong = 0;
    let empty = 0;

    QUESTIONS.forEach((q, idx) => {
      if (answers[idx] === null) {
        empty++;
      } else if (answers[idx] === q.answer) {
        correct++;
      } else {
        wrong++;
      }
    });

    const finalScore = Math.round((correct / QUESTIONS.length) * 100);
    const resultObj = {
      ...studentInfo,
      date: new Date().toLocaleString('id-ID'),
      score: finalScore,
      correct,
      wrong,
      empty,
      userAnswers: answers
    };

    setScoreData(resultObj);
    
    // Save to local storage for teacher
    const updated = [resultObj, ...savedSubmissions];
    setSavedSubmissions(updated);
    localStorage.setItem('sman1_gianyar_cbt_results', JSON.stringify(updated));

    setView('result');
    setShowConfirmSubmit(false);
  };

  // Export CSV
  const handleExportCSV = () => {
    if (savedSubmissions.length === 0) {
      alert("Belum ada data nilai tersimpan.");
      return;
    }

    let csvContent = "data:text/csv;charset=utf-8,";
    csvContent += "No,Nama Siswa,Kelas,NISN,Nilai (0-100),Benar,Salah,Kosong,Waktu Pengumpulan\n";

    savedSubmissions.forEach((item, index) => {
      csvContent += `${index + 1},"${item.name}",${item.classNum},${item.nisn},${item.score},${item.correct},${item.wrong},${item.empty},"${item.date}"\n`;
    });

    const encodedUri = encodeURI(csvContent);
    const link = document.createElement("a");
    link.setAttribute("href", encodedUri);
    link.setAttribute("download", `Rekap_Nilai_CBT_Fisika_SMAN1Gianyar_${new Date().toISOString().slice(0,10)}.csv`);
    document.body.appendChild(link);
    link.click();
    document.body.removeChild(link);
  };

  // Clear data
  const handleClearTeacherData = () => {
    if (window.confirm("Apakah Anda yakin ingin menghapus seluruh data rekap nilai ujian dari memori?")) {
      localStorage.removeItem('sman1_gianyar_cbt_results');
      setSavedSubmissions([]);
    }
  };

  // --- RENDER SCREEN: LOGIN ---
  if (view === 'login') {
    return (
      <div className="min-h-screen bg-slate-100 flex flex-col justify-between font-sans">
        {/* Header Branding */}
        <header className="bg-sky-900 text-white shadow-lg p-4">
          <div className="max-w-5xl mx-auto flex items-center justify-between">
            <div className="flex items-center space-x-3">
              <div className="bg-white p-2 rounded-full shadow text-sky-900 font-bold text-lg border-2 border-amber-400">
                SMA
              </div>
              <div>
                <h1 className="text-xl font-bold tracking-wide">CBT SMAN 1 GIANYAR</h1>
                <p className="text-xs text-sky-200">Sistem Ujian Daring Interaktif Terpadu T.A. 2026/2027</p>
              </div>
            </div>
            <button
              onClick={() => setView('teacher')}
              className="bg-sky-800 hover:bg-sky-700 text-sky-100 text-xs px-3 py-2 rounded flex items-center gap-1.5 border border-sky-600 transition"
            >
              <BarChart2 className="w-4 h-4" /> Dashboard Guru
            </button>
          </div>
        </header>

        {/* Form Main */}
        <main className="flex-1 max-w-lg w-full mx-auto p-4 flex flex-col justify-center">
          <div className="bg-white rounded-xl shadow-md border border-slate-200 overflow-hidden">
            <div className="bg-gradient-to-r from-sky-800 to-sky-900 text-white p-5 text-center">
              <BookOpen className="w-10 h-10 mx-auto mb-2 text-amber-300" />
              <h2 className="text-lg font-bold">Sumatif Tengah Semester (STS)</h2>
              <p className="text-xs text-sky-200 mt-1">Fisika Kelas X — Kinematika Gerak Lurus (50 Soal)</p>
            </div>

            <form onSubmit={handleStartExam} className="p-6 space-y-4">
              <div>
                <label className="block text-xs font-semibold text-slate-700 uppercase mb-1">Nama Lengkap Murid</label>
                <div className="relative">
                  <User className="w-4 h-4 absolute left-3 top-3 text-slate-400" />
                  <input
                    type="text"
                    required
                    placeholder="Masukkan nama sesuai presensi"
                    value={studentInfo.name}
                    onChange={(e) => setStudentInfo({ ...studentInfo, name: e.target.value })}
                    className="w-full pl-9 pr-3 py-2 text-sm border border-slate-300 rounded-lg focus:ring-2 focus:ring-sky-600 focus:outline-none"
                  />
                </div>
              </div>

              <div className="grid grid-cols-2 gap-3">
                <div>
                  <label className="block text-xs font-semibold text-slate-700 uppercase mb-1">Kelas</label>
                  <select
                    value={studentInfo.classNum}
                    onChange={(e) => setStudentInfo({ ...studentInfo, classNum: e.target.value })}
                    className="w-full px-3 py-2 text-sm border border-slate-300 rounded-lg focus:ring-2 focus:ring-sky-600 focus:outline-none"
                  >
                    {['X.1', 'X.2', 'X.3', 'X.4', 'X.5', 'X.6', 'X.7', 'X.8', 'X.9', 'x.10'].map(c => (
                      <option key={c} value={c}>Kelas {c}</option>
                    ))}
                  </select>
                </div>

                <div>
                  <label className="block text-xs font-semibold text-slate-700 uppercase mb-1">NISN / No. Absen</label>
                  <input
                    type="text"
                    required
                    placeholder="Contoh: 10245"
                    value={studentInfo.nisn}
                    onChange={(e) => setStudentInfo({ ...studentInfo, nisn: e.target.value })}
                    className="w-full px-3 py-2 text-sm border border-slate-300 rounded-lg focus:ring-2 focus:ring-sky-600 focus:outline-none"
                  />
                </div>
              </div>

              <div className="bg-amber-50 border border-amber-200 rounded-lg p-3 text-xs text-amber-800 space-y-1">
                <p className="font-semibold flex items-center gap-1">
                  <AlertCircle className="w-3.5 h-3.5" /> Petunjuk Pengerjaan:
                </p>
                <ul className="list-disc list-inside space-y-0.5 text-slate-600">
                  <li>Waktu pengerjaan total <strong>120 Menit</strong> (50 Soal).</li>
                  <li>Soal mencakup GLB, GLBB, GJB, &amp; Gerak Vertikal.</li>
                  <li>Jawaban akan tersimpan secara otomatis.</li>
                </ul>
              </div>

              <button
                type="submit"
                className="w-full bg-sky-700 hover:bg-sky-800 text-white font-bold py-2.5 rounded-lg shadow-md transition flex items-center justify-center gap-2 text-sm"
              >
                Mulai Ujian Sekarang <ChevronRight className="w-4 h-4" />
              </button>
            </form>
          </div>
        </main>

        <footer className="bg-slate-200 text-center text-xs text-slate-600 py-3">
          &copy; 2026 SMAN 1 Gianyar — Tim Pengembang Assessment Digital Fisika
        </footer>
      </div>
    );
  }

  // --- RENDER SCREEN: TEACHER DASHBOARD ---
  if (view === 'teacher') {
    return (
      <div className="min-h-screen bg-slate-100 flex flex-col font-sans">
        <header className="bg-slate-900 text-white p-4 shadow">
          <div className="max-w-6xl mx-auto flex justify-between items-center">
            <div className="flex items-center space-x-2">
              <BarChart2 className="w-6 h-6 text-sky-400" />
              <h1 className="font-bold text-lg">Dashboard Rekap Nilai Ujian (Guru)</h1>
            </div>
            <div className="flex gap-2">
              <button
                onClick={() => setView('login')}
                className="bg-slate-700 hover:bg-slate-600 text-xs px-3 py-1.5 rounded flex items-center gap-1"
              >
                <LogOut className="w-3.5 h-3.5" /> Kembali ke Login
              </button>
            </div>
          </div>
        </header>

        <main className="max-w-6xl w-full mx-auto p-4 flex-1">
          <div className="bg-white rounded-xl shadow border border-slate-200 p-5 mb-6">
            <div className="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4 mb-4">
              <div>
                <h2 className="text-base font-bold text-slate-800">Hasil Pengerjaan Ujian Siswa</h2>
                <p className="text-xs text-slate-500">STS Fisika Kelas X Kinematika Gerak (Tersimpan di LocalStorage)</p>
              </div>

              <div className="flex gap-2">
                <button
                  onClick={handleExportCSV}
                  className="bg-emerald-600 hover:bg-emerald-700 text-white text-xs font-semibold px-3 py-2 rounded-lg flex items-center gap-1.5 shadow-sm transition"
                >
                  <Download className="w-4 h-4" /> Ekspor Excel / CSV
                </button>
                <button
                  onClick={handleClearTeacherData}
                  className="bg-rose-50 hover:bg-rose-100 text-rose-700 text-xs font-semibold px-3 py-2 rounded-lg border border-rose-200 flex items-center gap-1 transition"
                >
                  <RefreshCw className="w-3.5 h-3.5" /> Reset Data
                </button>
              </div>
            </div>

            {savedSubmissions.length === 0 ? (
              <div className="text-center py-12 text-slate-400 border-2 border-dashed border-slate-200 rounded-lg">
                <FileText className="w-12 h-12 mx-auto mb-2 text-slate-300" />
                <p className="text-sm">Belum ada murid yang menyelesaikan ujian.</p>
              </div>
            ) : (
              <div className="overflow-x-auto">
                <table className="w-full text-left text-xs border-collapse">
                  <thead>
                    <tr className="bg-slate-100 border-b border-slate-200 text-slate-700 uppercase font-semibold">
                      <th className="p-3">No</th>
                      <th className="p-3">Nama Siswa</th>
                      <th className="p-3">Kelas</th>
                      <th className="p-3">NISN</th>
                      <th className="p-3">Nilai (0-100)</th>
                      <th className="p-3">Benar</th>
                      <th className="p-3">Salah</th>
                      <th className="p-3">Kosong</th>
                      <th className="p-3">Waktu Selesai</th>
                    </tr>
                  </thead>
                  <tbody className="divide-y divide-slate-100">
                    {savedSubmissions.map((sub, idx) => (
                      <tr key={idx} className="hover:bg-slate-50">
                        <td className="p-3 font-medium text-slate-600">{idx + 1}</td>
                        <td className="p-3 font-semibold text-slate-800">{sub.name}</td>
                        <td className="p-3 text-slate-600">{sub.classNum}</td>
                        <td className="p-3 text-slate-600">{sub.nisn}</td>
                        <td className="p-3">
                          <span className={`inline-block px-2.5 py-0.5 rounded-full font-bold text-xs ${
                            sub.score >= 75 ? 'bg-emerald-100 text-emerald-800' : 'bg-amber-100 text-amber-800'
                          }`}>
                            {sub.score}
                          </span>
                        </td>
                        <td className="p-3 text-emerald-600 font-semibold">{sub.correct}</td>
                        <td className="p-3 text-rose-600">{sub.wrong}</td>
                        <td className="p-3 text-slate-400">{sub.empty}</td>
                        <td className="p-3 text-slate-500">{sub.date}</td>
                      </tr>
                    ))}
                  </tbody>
                </table>
              </div>
            )}
          </div>
        </main>
      </div>
    );
  }

  // --- RENDER SCREEN: EXAM INTERFACE ---
  if (view === 'exam') {
    const currentQuestion = QUESTIONS[currentIdx];

    return (
      <div className="min-h-screen bg-slate-100 flex flex-col font-sans select-none">
        {/* Top Navbar */}
        <header className="bg-sky-900 text-white shadow-md sticky top-0 z-20">
          <div className="max-w-7xl mx-auto px-4 py-2.5 flex items-center justify-between">
            <div className="flex items-center space-x-3">
              <span className="bg-amber-400 text-sky-950 font-extrabold text-xs px-2 py-1 rounded">
                CBT v2.6
              </span>
              <div className="hidden sm:block">
                <h1 className="text-sm font-bold leading-tight">STS FISIKA KELAS X — SMAN 1 GIANYAR</h1>
                <p className="text-[10px] text-sky-200">Peserta: <strong>{studentInfo.name}</strong> ({studentInfo.classNum})</p>
              </div>
            </div>

            {/* Timer Display */}
            <div className="flex items-center space-x-3">
              <div className="flex items-center gap-1.5 bg-sky-950 px-3 py-1.5 rounded-lg border border-sky-700">
                <Clock className="w-4 h-4 text-amber-400 animate-pulse" />
                <span className="font-mono font-bold text-sm tracking-wide text-amber-300">
                  {formatTime(timeLeft)}
                </span>
              </div>

              <button
                onClick={() => setShowConfirmSubmit(true)}
                className="bg-emerald-600 hover:bg-emerald-500 text-white text-xs font-bold px-3 py-1.5 rounded-lg shadow transition"
              >
                Selesai Ujian
              </button>
            </div>
          </div>
        </header>

        {/* Main Content Area */}
        <div className="flex-1 max-w-7xl w-full mx-auto p-3 sm:p-4 grid grid-cols-1 lg:grid-cols-4 gap-4">
          
          {/* Question Box (3 cols) */}
          <main className="lg:col-span-3 flex flex-col justify-between bg-white rounded-xl shadow-sm border border-slate-200 p-4 sm:p-6 min-h-[500px]">
            <div>
              {/* Question Header */}
              <div className="flex justify-between items-center border-b border-slate-100 pb-3 mb-4">
                <div className="flex items-center gap-2">
                  <span className="bg-sky-800 text-white font-bold text-sm px-3 py-1 rounded-md">
                    Soal No. {currentIdx + 1}
                  </span>
                  {currentQuestion.hots && (
                    <span className="bg-slate-900 text-amber-300 font-bold text-[10px] px-2 py-0.5 rounded tracking-wider flex items-center gap-1">
                      <ShieldAlert className="w-3 h-3" /> HOTS / TKA
                    </span>
                  )}
                </div>

                <label className="flex items-center gap-2 cursor-pointer bg-amber-50 hover:bg-amber-100 border border-amber-200 px-3 py-1 rounded-md transition text-xs text-amber-800 font-semibold">
                  <input
                    type="checkbox"
                    checked={doubts[currentIdx]}
                    onChange={handleToggleDoubt}
                    className="rounded text-amber-600 focus:ring-amber-500"
                  />
                  Ragu-Ragu
                </label>
              </div>

              {/* Question Text */}
              <div className="text-sm sm:text-base text-slate-800 leading-relaxed mb-4">
                {currentQuestion.text}
              </div>

              {/* Diagram SVG if available */}
              {currentQuestion.svgType && (
                <div className="my-4">
                  <RenderSVGDiagram type={currentQuestion.svgType} />
                </div>
              )}

              {/* Options List */}
              <div className="space-y-2.5 mt-4">
                {currentQuestion.options.map((opt, optIdx) => {
                  const isSelected = answers[currentIdx] === optIdx;
                  const optionLetters = ['A', 'B', 'C', 'D', 'E'];

                  return (
                    <button
                      key={optIdx}
                      onClick={() => handleSelectOption(optIdx)}
                      className={`w-full text-left p-3 rounded-lg border text-sm transition flex items-center space-x-3 ${
                        isSelected
                          ? 'bg-sky-50 border-sky-600 text-sky-900 font-medium shadow-sm'
                          : 'bg-white border-slate-200 hover:bg-slate-50 text-slate-700'
                      }`}
                    >
                      <span className={`w-7 h-7 flex items-center justify-center rounded-full text-xs font-bold flex-shrink-0 ${
                        isSelected ? 'bg-sky-700 text-white' : 'bg-slate-100 text-slate-600 border border-slate-300'
                      }`}>
                        {optionLetters[optIdx]}
                      </span>
                      <span className="flex-1">{opt}</span>
                    </button>
                  );
                })}
              </div>
            </div>

            {/* Bottom Controls */}
            <div className="flex justify-between items-center border-t border-slate-100 pt-4 mt-6">
              <button
                disabled={currentIdx === 0}
                onClick={() => setCurrentIdx((prev) => prev - 1)}
                className="bg-slate-100 hover:bg-slate-200 disabled:opacity-40 text-slate-700 text-xs font-bold px-4 py-2 rounded-lg flex items-center gap-1 transition"
              >
                <ChevronLeft className="w-4 h-4" /> Sebelumnya
              </button>

              <span className="text-xs text-slate-400 font-medium">
                {answers.filter(a => a !== null).length} dari 50 Terjawab
              </span>

              {currentIdx === QUESTIONS.length - 1 ? (
                <button
                  onClick={() => setShowConfirmSubmit(true)}
                  className="bg-emerald-600 hover:bg-emerald-700 text-white text-xs font-bold px-5 py-2 rounded-lg transition"
                >
                  Selesai Ujian
                </button>
              ) : (
                <button
                  onClick={() => setCurrentIdx((prev) => prev + 1)}
                  className="bg-sky-800 hover:bg-sky-900 text-white text-xs font-bold px-4 py-2 rounded-lg flex items-center gap-1 transition shadow-sm"
                >
                  Berikutnya <ChevronRight className="w-4 h-4" />
                </button>
              )}
            </div>
          </main>

          {/* Navigation Drawer (1 col) */}
          <aside className="bg-white rounded-xl shadow-sm border border-slate-200 p-4 flex flex-col justify-between">
            <div>
              <h3 className="text-xs font-bold uppercase text-slate-500 mb-3 flex items-center justify-between">
                <span>Navigasi Soal</span>
                <span className="text-[10px] text-sky-700 font-semibold bg-sky-50 px-2 py-0.5 rounded">1 - 50</span>
              </h3>

              {/* Question Number Grid */}
              <div className="grid grid-cols-5 gap-1.5 max-h-[380px] overflow-y-auto pr-1">
                {QUESTIONS.map((_, idx) => {
                  const isAns = answers[idx] !== null;
                  const isDoubt = doubts[idx];
                  const isCurr = currentIdx === idx;

                  let btnBg = "bg-slate-100 text-slate-600 border-slate-200";
                  if (isDoubt) {
                    btnBg = "bg-amber-400 text-slate-900 border-amber-500 font-bold";
                  } else if (isAns) {
                    btnBg = "bg-sky-700 text-white border-sky-800 font-semibold";
                  }

                  return (
                    <button
                      key={idx}
                      onClick={() => setCurrentIdx(idx)}
                      className={`h-9 w-full rounded text-xs transition flex items-center justify-center relative border ${btnBg} ${
                        isCurr ? 'ring-2 ring-amber-400 ring-offset-1 font-extrabold' : ''
                      }`}
                    >
                      {idx + 1}
                    </button>
                  );
                })}
              </div>
            </div>

            {/* Legend */}
            <div className="border-t border-slate-100 pt-3 mt-4 space-y-1.5 text-[11px] text-slate-600">
              <div className="flex items-center gap-2">
                <span className="w-3.5 h-3.5 rounded bg-sky-700 inline-block"></span>
                <span>Sudah Dijawab</span>
              </div>
              <div className="flex items-center gap-2">
                <span className="w-3.5 h-3.5 rounded bg-amber-400 inline-block"></span>
                <span>Ragu-Ragu</span>
              </div>
              <div className="flex items-center gap-2">
                <span className="w-3.5 h-3.5 rounded bg-slate-100 border border-slate-300 inline-block"></span>
                <span>Belum Dijawab</span>
              </div>
            </div>
          </aside>
        </div>

        {/* Modal Confirm Submit */}
        {showConfirmSubmit && (
          <div className="fixed inset-0 bg-black/50 z-50 flex items-center justify-center p-4">
            <div className="bg-white rounded-xl max-w-md w-full p-6 shadow-xl space-y-4">
              <div className="flex items-center gap-3 text-amber-600">
                <AlertCircle className="w-8 h-8 flex-shrink-0" />
                <h3 className="text-base font-bold text-slate-800">Konfirmasi Kumpulkan Ujian</h3>
              </div>

              <div className="text-xs text-slate-600 space-y-2 bg-slate-50 p-3 rounded-lg border border-slate-200">
                <p>Apakah Anda yakin ingin mengakhiri ujian ini?</p>
                <div className="grid grid-cols-3 gap-2 text-center pt-2 font-semibold">
                  <div className="bg-emerald-50 text-emerald-800 p-2 rounded">
                    <div>{answers.filter(a => a !== null).length}</div>
                    <div className="text-[10px] font-normal">Dijawab</div>
                  </div>
                  <div className="bg-amber-50 text-amber-800 p-2 rounded">
                    <div>{doubts.filter(d => d).length}</div>
                    <div className="text-[10px] font-normal">Ragu-Ragu</div>
                  </div>
                  <div className="bg-rose-50 text-rose-800 p-2 rounded">
                    <div>{answers.filter(a => a === null).length}</div>
                    <div className="text-[10px] font-normal">Kosong</div>
                  </div>
                </div>
              </div>

              <div className="flex justify-end gap-2 pt-2">
                <button
                  onClick={() => setShowConfirmSubmit(false)}
                  className="bg-slate-200 hover:bg-slate-300 text-slate-700 text-xs font-semibold px-4 py-2 rounded-lg transition"
                >
                  Batal
                </button>
                <button
                  onClick={handleAutoSubmit}
                  className="bg-emerald-600 hover:bg-emerald-700 text-white text-xs font-bold px-4 py-2 rounded-lg shadow transition"
                >
                  Ya, Kumpulkan Sekarang
                </button>
              </div>
            </div>
          </div>
        )}
      </div>
    );
  }

  // --- RENDER SCREEN: RESULT & EXPLANATION ---
  if (view === 'result' && scoreData) {
    return (
      <div className="min-h-screen bg-slate-100 flex flex-col font-sans">
        <header className="bg-sky-900 text-white p-4 shadow">
          <div className="max-w-5xl mx-auto flex justify-between items-center">
            <h1 className="font-bold text-base">Hasil &amp; Pembahasan Ujian CBT Fisika</h1>
            <button
              onClick={() => setView('login')}
              className="bg-sky-800 hover:bg-sky-700 text-xs px-3 py-1.5 rounded flex items-center gap-1"
            >
              <LogOut className="w-3.5 h-3.5" /> Keluar
            </button>
          </div>
        </header>

        <main className="max-w-4xl w-full mx-auto p-4 space-y-6 flex-1">
          {/* Score Card Banner */}
          <div className="bg-white rounded-xl shadow-md border border-slate-200 p-6 text-center relative overflow-hidden">
            <div className="absolute top-0 left-0 w-full h-2 bg-gradient-to-r from-sky-600 via-amber-400 to-emerald-500"></div>

            <Award className="w-12 h-12 text-amber-500 mx-auto mb-2" />
            <h2 className="text-xl font-bold text-slate-800">Laporan Hasil Ujian STS Fisika</h2>
            <p className="text-xs text-slate-500 mb-4">{scoreData.name} ({scoreData.classNum}) — NISN: {scoreData.nisn}</p>

            <div className="inline-block bg-sky-50 border border-sky-200 rounded-2xl px-8 py-4 my-2">
              <span className="text-xs uppercase tracking-wider font-semibold text-sky-700 block mb-1">Nilai Akhir Ujian</span>
              <span className="text-4xl font-black text-sky-900">{scoreData.score}</span>
              <span className="text-xs text-slate-400 block font-normal">skala 0 - 100</span>
            </div>

            <div className="grid grid-cols-3 gap-3 max-w-sm mx-auto mt-4 text-xs font-semibold">
              <div className="bg-emerald-50 text-emerald-800 p-2.5 rounded-lg border border-emerald-200">
                <div className="text-lg font-bold">{scoreData.correct}</div>
                <div className="text-[10px]">Benar</div>
              </div>
              <div className="bg-rose-50 text-rose-800 p-2.5 rounded-lg border border-rose-200">
                <div className="text-lg font-bold">{scoreData.wrong}</div>
                <div className="text-[10px]">Salah</div>
              </div>
              <div className="bg-slate-100 text-slate-700 p-2.5 rounded-lg border border-slate-200">
                <div className="text-lg font-bold">{scoreData.empty}</div>
                <div className="text-[10px]">Kosong</div>
              </div>
            </div>
          </div>

          {/* Solutions Section */}
          <div className="bg-white rounded-xl shadow-md border border-slate-200 p-6 space-y-6">
            <h3 className="text-base font-bold text-slate-800 border-b border-slate-200 pb-3 flex items-center gap-2">
              <CheckCircle className="w-5 h-5 text-emerald-600" /> Kunci Jawaban &amp; Pembahasan Lengkap
            </h3>

            <div className="space-y-6">
              {QUESTIONS.map((q, idx) => {
                const userAns = scoreData.userAnswers[idx];
                const isCorrect = userAns === q.answer;
                const isNull = userAns === null;
                const optionLetters = ['A', 'B', 'C', 'D', 'E'];

                return (
                  <div
                    key={q.id}
                    className={`p-4 rounded-xl border text-xs sm:text-sm ${
                      isCorrect
                        ? 'bg-emerald-50/40 border-emerald-200'
                        : isNull
                        ? 'bg-slate-50 border-slate-200'
                        : 'bg-rose-50/40 border-rose-200'
                    }`}
                  >
                    <div className="flex justify-between items-start mb-2">
                      <span className="font-bold text-slate-800">
                        Soal {idx + 1}. {q.hots && <span className="text-[10px] bg-slate-900 text-amber-300 px-1.5 py-0.5 rounded ml-1">HOTS</span>}
                      </span>
                      <span className={`px-2 py-0.5 rounded text-[11px] font-bold ${
                        isCorrect
                          ? 'bg-emerald-100 text-emerald-800'
                          : isNull
                          ? 'bg-slate-200 text-slate-700'
                          : 'bg-rose-100 text-rose-800'
                      }`}>
                        {isCorrect ? 'Benar (+2)' : isNull ? 'Tidak Dijawab' : 'Salah (0)'}
                      </span>
                    </div>

                    <p className="text-slate-700 mb-3">{q.text}</p>

                    {q.svgType && (
                      <div className="my-3">
                        <RenderSVGDiagram type={q.svgType} />
                      </div>
                    )}

                    <div className="grid grid-cols-1 sm:grid-cols-2 gap-1.5 my-3 text-xs">
                      {q.options.map((opt, oIdx) => {
                        const isChosen = userAns === oIdx;
                        const isKey = q.answer === oIdx;

                        let optStyle = "bg-white border-slate-200 text-slate-600";
                        if (isKey) optStyle = "bg-emerald-100 border-emerald-400 text-emerald-900 font-bold";
                        if (isChosen && !isKey) optStyle = "bg-rose-100 border-rose-300 text-rose-900 font-semibold";

                        return (
                          <div key={oIdx} className={`p-2 rounded border flex items-center justify-between ${optStyle}`}>
                            <span>{optionLetters[oIdx]}. {opt}</span>
                            {isKey && <Check className="w-3.5 h-3.5 text-emerald-700" />}
                            {isChosen && !isKey && <X className="w-3.5 h-3.5 text-rose-600" />}
                          </div>
                        );
                      })}
                    </div>

                    <div className="bg-white p-3 rounded-lg border border-slate-200 text-xs text-slate-600 mt-2 space-y-1">
                      <span className="font-bold text-sky-900 block flex items-center gap-1">
                        <HelpCircle className="w-3.5 h-3.5 text-sky-600" /> Pembahasan:
                      </span>
                      <p className="leading-relaxed">{q.explanation}</p>
                    </div>
                  </div>
                );
              })}
            </div>
          </div>
        </main>
      </div>
    );
  }

  return null;
}
