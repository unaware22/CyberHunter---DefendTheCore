# 🛡️ Cyber Hunter: The Anti-Cheat Simulation (Console & Standart Scripting)

![Cyber Hunter Gameplay](link-ke-gif-gameplay-kamu.gif)
![gemplaygif](https://github.com/user-attachments/assets/1de349d3-c963-4cd8-bb60-58852976e856)

**Cyber Hunter** bukan sekadar game HTML5 *Tower Defense* biasa. Proyek ini dibangun sebagai **Laboratorium Simulasi Keamanan Klien (Client-Side Security)**. 

Tujuan utama repositori ini adalah mendemonstrasikan bagaimana celah keamanan pada *browser games* dieksploitasi oleh peretas dari berbagai tingkatan, dan bagaimana sistem pertahanan (Shielding) dibangun untuk menangkalnya.

Mainkan Live Demo: https://defendthecore.netlify.app/

---

## 💻 Simulasi Level 1: The Junior Hacker (Script Kiddies)

Peretas pemula biasanya menyerang dari "Pintu Depan" menggunakan fitur bawaan browser, yaitu Developer Tools (F12). Di sini, sistem pertahanan kita (Vanguard) diuji.

### ⚔️ Exploit 1: Console Injection
* **Serangan:** Hacker membuka tab Console dan mencoba mengubah variabel *game* secara langsung, misalnya mengetik `player.coins = 999999` atau `player.hp = 9999` untuk menjadi kebal.
  <img width="1904" height="892" alt="image" src="https://github.com/user-attachments/assets/b8eba90b-742d-41e8-87d6-ad161c4fcb38" />

* **🛡️ Vanguard Shield (The Vault & Honeypot):** Game ini mengisolasi seluruh logika utamanya di dalam **IIFE (Immediately Invoked Function Expression)** sehingga variabel aslinya tidak bisa diakses dari global objek. Sebagai jebakan, Vanguard memasang sebuah **Proxy Honeypot**.
  
  ```javascript
  // Jebakan mematikan untuk hacker yang mencari objek 'player'
  window.player = new Proxy({ coins: 0, hp: 100 }, {
      set: function(obj, prop, value) { 
          window.triggerBan(`VANGUARD: Illegal Write on '${prop}'`); 
          return false; 
      }
  });
* **Hasil:** Hacker terkena Auto-Ban seketika!
  <img width="1916" height="903" alt="image" src="https://github.com/user-attachments/assets/abf7d34d-e971-48c9-a238-0a5ea742d146" />

### ⚔️ Exploit 2: DOM Auto-Clicker
* **Serangan:** Hacker malas mengklik mouse dan membuat skrip otomatis di Console: setInterval(() => { document.dispatchEvent(new MouseEvent('mousedown')) }, 10); agar senjata menembak seperti machine gun.
<img width="1906" height="909" alt="image" src="https://github.com/user-attachments/assets/ca11e320-a492-4e2c-9a92-02be2b8dd1f1" />

* **🛡️ Vanguard Shield (Event Validation):** Setiap klik divalidasi menggunakan property bawaan browser e.isTrusted.
  
  ```javascript
  function handleShoot(x, y, isTrusted) {
    if (!isTrusted) return window.triggerBan("Auto-Clicker Blocked!");
    // ... logika tembakan ...
  }
* **Hasil:** Tembakan dari skrip injeksi ditolak karena isTrusted bernilai false (bukan dari hardware fisik manusia). Hacker kembali di-Banned.
<img width="1915" height="911" alt="image" src="https://github.com/user-attachments/assets/1d88ea1e-8fd5-479e-a531-465b1834e47b" />

## ☠️ Simulasi Level 2: The Senior Hacker (Advanced Threats)
Di level ini, kita harus jujur pada realitas Cybersecurity. Tidak ada game murni HTML/JS (Client-Side) yang 100% aman. Jika sistem Vanguard kita diserang oleh Hacker Senior, pertahanan HTML kita akan hancur. Mengapa? Berikut cara mereka menjebolnya:

### 1. Memory Manipulation (Cheat Engine)
Walaupun kode JS kita sembunyikan dalam IIFE, angka coins dan hp pada akhirnya tetap bersarang di kepingan memori RAM laptop/HP pemain. Hacker Senior menggunakan software seperti Cheat Engine untuk memindai RAM secara langsung dan mengubah angkanya (misal dari 50 -> 9999). Vanguard tidak bisa melihat aktivitas di luar Browser.

### 2. Network Interception (Man-in-the-Middle)
Hacker menggunakan aplikasi Proxy seperti Burp Suite. Saat browser mencoba mengunduh file index.html dari server (sebelum game dimulai), mereka mencegat file tersebut di tengah jalan, MENGHAPUS seluruh kode Vanguard Anti-Cheat kita, lalu membiarkan browser memuat versi game yang sudah "telanjang".

### 3. Hardware / OS Level Macros
Hacker menggunakan mouse gaming canggih (Razer/Logitech) yang mengirimkan sinyal Auto-Clicker langsung dari level Sistem Operasi (Windows/Mac). Browser Chrome mengira klik tersebut berasal dari jari manusia asli, sehingga e.isTrusted tetap bernilai true dan sistem kita tertipu.

## 🚀 Cara Menjalankan Simulasi Ini
### 1. Clone repositori ini.
### 2. Buka index.html di browser apa pun.
### 3. Buka tab F12 (Developer Tools) -> Console.
### 4. Cobalah menjadi Junior Hacker dengan mengetik: player.coins = 9999999 dan lihat apa yang terjadi!
