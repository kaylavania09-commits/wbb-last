<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Smart Parking AI</title>
<style>
:root{--asp:#1f2227;--asp2:#2a2e35;--line:#f2c230;--ink:#f3f1ea;--mute:#9da2aa;--ok:#4cc38a;--bad:#ef5f5f;color-scheme:dark}
*{box-sizing:border-box}
body{margin:0;background:var(--asp);color:var(--ink);font:15px/1.5 system-ui,-apple-system,"Segoe UI",sans-serif}
header{display:flex;flex-wrap:wrap;gap:12px;align-items:center;justify-content:space-between;padding:16px 20px;border-bottom:3px dashed var(--line)}
h1{margin:0;font-size:1.3rem}
h2{margin:0 0 10px;font-size:1rem;color:var(--mute);font-weight:600}
.chips{display:flex;gap:8px;flex-wrap:wrap}
.chip{padding:3px 10px;border-radius:99px;background:var(--asp2);color:var(--mute);font-size:.85rem}
.chip::before{content:"";display:inline-block;width:8px;height:8px;border-radius:50%;background:var(--bad);margin-right:6px}
.chip.on{color:var(--ink)}.chip.on::before{background:var(--ok)}
main{display:grid;grid-template-columns:repeat(auto-fit,minmax(320px,1fr));gap:16px;padding:16px 20px;max-width:1100px;margin:auto}
section{background:var(--asp2);border-radius:10px;padding:16px}
.wide{grid-column:1/-1}
.bays{display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-bottom:12px}
.bay{border:2px solid var(--ink);border-top-color:var(--line);border-bottom:0;padding:14px 8px;text-align:center;border-radius:4px 4px 0 0;min-height:96px}
.bay b{display:block;font-size:1.6rem}
.bay.full b{color:var(--bad)}.bay.free b{color:var(--ok)}
.count{font-size:1.1rem;margin:4px 0 12px}
.gates{display:grid;grid-template-columns:1fr 1fr;gap:12px}
.gate{background:var(--asp);border-radius:8px;padding:10px 12px}
.gate b{display:block}
.gate.open b{color:var(--line)}
button{font:inherit;background:var(--line);color:#1a1a1a;border:0;border-radius:6px;padding:7px 12px;cursor:pointer;margin:6px 6px 0 0}
button.alt{background:transparent;color:var(--ink);border:1px solid var(--mute)}
button:focus-visible{outline:2px solid var(--ink);outline-offset:2px}
#cam{width:100%;max-width:320px;aspect-ratio:4/3;background:#000;border-radius:8px;overflow:hidden}
#cam canvas{width:100%;height:100%}
.bar{display:grid;grid-template-columns:90px 1fr 44px;gap:8px;align-items:center;font-size:.85rem;margin-top:6px}
.bar i{display:block;height:8px;border-radius:4px;background:var(--line);width:0}
.bar span:last-child{text-align:right;color:var(--mute)}
label{display:inline-flex;gap:6px;align-items:center;margin-top:10px}
#log{list-style:none;margin:0;padding:0;max-height:260px;overflow:auto}
#log li{padding:5px 0;border-bottom:1px solid var(--asp);display:flex;gap:10px}
#log em{font-style:normal;color:var(--line);min-width:72px}
.hint{color:var(--mute);font-size:.85rem}
nav{display:flex;gap:6px;flex-wrap:wrap}
nav button{margin:0;background:transparent;color:var(--ink);border:1px solid var(--mute)}
nav button.on{background:var(--line);color:#1a1a1a;border-color:var(--line)}
.tab{display:none}
main.tab.show{display:grid}
div.tab.show{display:block}
.page{max-width:1100px;margin:auto;padding:16px 20px}
pre{background:#14161a;border-radius:8px;padding:14px;overflow:auto;max-height:520px;font:13px/1.5 ui-monospace,Consolas,monospace;margin:10px 0 0}
.page ol{padding-left:20px}.page li{margin:6px 0}
.pal{display:block;width:100%;max-width:160px;margin-bottom:6px}
.arm{transform-origin:22px 38px;transition:transform .7s ease}
.gate.open .arm{transform:rotate(-70deg)}
@media (prefers-reduced-motion:reduce){.arm{transition:none}}
</style>
</head>
<body>
<header>
  <h1>Smart Parking AI</h1>
  <nav id="nav">
    <button data-t="dash" class="on">Dashboard</button>
    <button data-t="mb">Kode micro:bit</button>
    <button data-t="esp">Kode ESP32</button>
    <button data-t="guide">Panduan</button>
  </nav>
  <div class="chips">
    <span class="chip" id="chip-fb">Firebase</span>
    <span class="chip" id="chip-ble">micro:bit</span>
    <span class="chip" id="chip-cam">Kamera AI</span>
  </div>
</header>

<main class="tab show" id="tab-dash">
  <section>
    <h2>Slot parkir</h2>
    <div class="bays">
      <div class="bay" id="slotA">Slot A<b>-</b></div>
      <div class="bay" id="slotB">Slot B<b>-</b></div>
    </div>
    <div class="count" id="count">Tersedia - dari -</div>
    <h2>Gerbang</h2>
    <div class="gates">
      <div class="gate" id="gateIn"><svg viewBox="0 0 140 70" class="pal" aria-hidden="true"><g class="arm"><rect x="22" y="34" width="112" height="8" rx="2" fill="#f3f1ea"/><rect x="48" y="34" width="14" height="8" fill="#ef5f5f"/><rect x="78" y="34" width="14" height="8" fill="#ef5f5f"/><rect x="108" y="34" width="14" height="8" fill="#ef5f5f"/></g><rect x="8" y="30" width="28" height="34" rx="3" fill="#555b66"/><circle cx="22" cy="38" r="5" fill="#f2c230"/><rect x="0" y="64" width="140" height="4" fill="#3a3f47"/></svg><span>Gate IN (micro:bit)</span><b>-</b></div>
      <div class="gate" id="gateOut"><svg viewBox="0 0 140 70" class="pal" aria-hidden="true"><g class="arm"><rect x="22" y="34" width="112" height="8" rx="2" fill="#f3f1ea"/><rect x="48" y="34" width="14" height="8" fill="#ef5f5f"/><rect x="78" y="34" width="14" height="8" fill="#ef5f5f"/><rect x="108" y="34" width="14" height="8" fill="#ef5f5f"/></g><rect x="8" y="30" width="28" height="34" rx="3" fill="#555b66"/><circle cx="22" cy="38" r="5" fill="#f2c230"/><rect x="0" y="64" width="140" height="4" fill="#3a3f47"/></svg><span>Gate OUT (ESP32)</span><b>-</b></div>
    </div>
    <button id="btnBle">Sambungkan micro:bit</button>
    <button class="alt" id="btnInOpen">Buka Gate IN</button>
    <button class="alt" id="btnInClose">Tutup Gate IN</button>
    <button class="alt" id="btnOutOpen">Buka Gate OUT</button>
  </section>

  <section>
    <h2>Deteksi mobil (Teachable Machine)</h2>
    <div id="cam"></div>
    <div id="bars"></div>
    <button id="btnCam">Aktifkan kamera</button>
    <label><input type="checkbox" id="auto" checked> Gate IN otomatis saat AI melihat mobil</label>
    <p class="hint" id="autoHint">Gate hanya dibuka bila micro:bit tersambung dan slot masih tersedia.</p>
  </section>

  <section class="wide">
    <h2>Log aktivitas</h2>
    <ul id="log"><li>Memuat log...</li></ul>
  </section>
</main>


<div class="page tab" id="tab-mb">
  <h2>Kode micro:bit (Gate IN) - MakeCode JavaScript</h2>
  <p class="hint">Di MakeCode: tambah extension bluetooth, Project Settings - Bluetooth - No Pairing Required, buka tab JavaScript, paste, lalu download ke micro:bit.</p>
  <button data-copy="code-mb">Salin kode</button><button class="alt" data-dl="code-mb" data-name="microbit-gate-in.ts">Unduh</button>
  <pre id="code-mb">// ============================================================
// SMART PARKING AI - micro:bit (GATE IN)
// ============================================================
// Wiring:
//   Servo Gate IN : signal -&gt; P0, VCC -&gt; 5V EKSTERNAL, GND -&gt; GND (disatukan dgn micro:bit)
//   IR Gate IN    : OUT -&gt; P1, VCC -&gt; 3V, GND -&gt; GND  (FC-51: LOW = ada mobil)
//
// Setup MakeCode:
//   1. Extensions -&gt; tambahkan &quot;bluetooth&quot;
//   2. Project Settings (ikon gear) -&gt; Bluetooth -&gt; &quot;No Pairing Required&quot;
//   3. Paste kode ini di tab JavaScript, lalu Download ke micro:bit
//
// Perintah dari website (Bluetooth UART, diakhiri baris baru):
//   IN:OPEN  -&gt; buka gate (otomatis menutup setelah WAKTU_BUKA)
//   IN:CLOSE -&gt; tutup gate
//   PING     -&gt; dibalas PONG
// Pesan ke website:
//   IN:OPEN / IN:CLOSED / IN:IR:1 (mobil di sensor) / IN:IR:0 / PONG
// Tombol: A = buka, B = tutup (untuk tes tanpa website)
// Jika Bluetooth belum tersambung, gate membuka sendiri saat sensor IR mendeteksi mobil.
// ============================================================

const SUDUT_TUTUP = 0
const SUDUT_BUKA = 90
const WAKTU_BUKA = 4000   // ms gate tetap terbuka

let terhubung = false
let gateTerbuka = false
let tutupPada = 0
let irAda = false

function kirim(pesan: string) {
    if (terhubung) {
        bluetooth.uartWriteLine(pesan)
    }
}

function setGate(buka: boolean) {
    gateTerbuka = buka
    if (buka) {
        pins.servoWritePin(AnalogPin.P0, SUDUT_BUKA)
        tutupPada = input.runningTime() + WAKTU_BUKA
        basic.showIcon(IconNames.Yes)
        kirim(&quot;IN:OPEN&quot;)
    } else {
        pins.servoWritePin(AnalogPin.P0, SUDUT_TUTUP)
        basic.showIcon(IconNames.No)
        kirim(&quot;IN:CLOSED&quot;)
    }
}

bluetooth.onBluetoothConnected(function () {
    terhubung = true
    basic.showIcon(IconNames.Heart)
})

bluetooth.onBluetoothDisconnected(function () {
    terhubung = false
    basic.showIcon(IconNames.Sad)
})

bluetooth.onUartDataReceived(serial.delimiters(Delimiters.NewLine), function () {
    let perintah = bluetooth.uartReadUntil(serial.delimiters(Delimiters.NewLine))
    if (perintah == &quot;IN:OPEN&quot;) {
        setGate(true)
    } else if (perintah == &quot;IN:CLOSE&quot;) {
        setGate(false)
    } else if (perintah == &quot;PING&quot;) {
        kirim(&quot;PONG&quot;)
    }
})

input.onButtonPressed(Button.A, function () {
    setGate(true)
})

input.onButtonPressed(Button.B, function () {
    setGate(false)
})

bluetooth.startUartService()
pins.setPull(DigitalPin.P1, PinPullMode.PullUp)
setGate(false)

basic.forever(function () {
    let adaMobil = pins.digitalReadPin(DigitalPin.P1) == 0

    // Laporkan perubahan sensor IR
    if (adaMobil != irAda) {
        irAda = adaMobil
        kirim(adaMobil ? &quot;IN:IR:1&quot; : &quot;IN:IR:0&quot;)
        // Mode mandiri: tanpa Bluetooth, sensor IR langsung membuka gate
        if (adaMobil &amp;&amp; !terhubung &amp;&amp; !gateTerbuka) {
            setGate(true)
        }
    }

    // Tutup otomatis setelah waktu habis DAN mobil sudah lewat (aman)
    if (gateTerbuka &amp;&amp; input.runningTime() &gt;= tutupPada &amp;&amp; !adaMobil) {
        setGate(false)
    }

    basic.pause(100)
})
</pre>
</div>
<div class="page tab" id="tab-esp">
  <h2>Kode ESP32 (Slot A/B + Gate OUT) - Arduino IDE</h2>
  <p class="hint">Isi nama Wi-Fi, password, dan link Firebase. Install library ESP32Servo lewat Library Manager.</p>
  <button data-copy="code-esp">Salin kode</button><button class="alt" data-dl="code-esp" data-name="smart_parking_ai.ino">Unduh</button>
  <pre id="code-esp">/*
  ============================================================
  SMART PARKING - ESP32 (Gate + Deteksi Slot + Firebase Realtime)
  ============================================================

  Fitur:
    1. Gate IN  : sensor IR mendeteksi mobil masuk -&gt; servo membuka portal
    2. Gate OUT : sensor IR mendeteksi mobil keluar -&gt; servo membuka portal
    3. Slot A &amp; Slot B: sensor IR mendeteksi status terisi/kosong
    4. Semua status dikirim ke Firebase Realtime Database, sehingga
       website bisa menampilkan kondisi terkini secara realtime
       (website &quot;mendengarkan&quot; perubahan lewat Server-Sent Events)

  ------------------------------------------------------------
  WIRING
  ------------------------------------------------------------
  Sensor IR Gate IN   : VCC-&gt;3V3, GND-&gt;GND, OUT-&gt;GPIO 32
  Sensor IR Gate OUT  : VCC-&gt;3V3, GND-&gt;GND, OUT-&gt;GPIO 33
  Sensor IR Slot A    : VCC-&gt;3V3, GND-&gt;GND, OUT-&gt;GPIO 34
  Sensor IR Slot B    : VCC-&gt;3V3, GND-&gt;GND, OUT-&gt;GPIO 35
  Servo Gate IN       : Signal-&gt;GPIO 13, VCC-&gt;5V EKSTERNAL, GND-&gt;GND
  Servo Gate OUT      : Signal-&gt;GPIO 27, VCC-&gt;5V EKSTERNAL, GND-&gt;GND

  PENTING: Servo JANGAN diberi daya dari pin 5V/3V3 ESP32.
  Pakai adaptor 5V terpisah, GND tetap disatukan dengan GND ESP32.

  ------------------------------------------------------------
  LIBRARY YANG WAJIB DI-INSTALL (Library Manager)
  ------------------------------------------------------------
  - &quot;ESP32Servo&quot; oleh Kevin Harrington / madhephaestus

  ------------------------------------------------------------
  STRUKTUR DATA DI FIREBASE (path: /parking)
  ------------------------------------------------------------
  {
    &quot;gateIn&quot;:  { &quot;status&quot;: &quot;tertutup&quot;, &quot;updated&quot;: 123 },
    &quot;gateOut&quot;: { &quot;status&quot;: &quot;tertutup&quot;, &quot;updated&quot;: 123 },
    &quot;slotA&quot;:   { &quot;occupied&quot;: false, &quot;updated&quot;: 123 },
    &quot;slotB&quot;:   { &quot;occupied&quot;: true,  &quot;updated&quot;: 123 },
    &quot;available&quot;: 1,
    &quot;total&quot;: 2
  }
*/

#include &lt;WiFi.h&gt;
#include &lt;WiFiClientSecure.h&gt;
#include &lt;HTTPClient.h&gt;
#include &lt;ESP32Servo.h&gt;

// ================== KONFIGURASI WAJIB DIISI ==================
const char* WIFI_SSID = &quot;NAMA_WIFI_KAMU&quot;;
const char* WIFI_PASSWORD = &quot;PASSWORD_WIFI_KAMU&quot;;

// URL Firebase Realtime Database, tanpa &quot;/&quot; di akhir
// contoh: https://smart-parking-abc12-default-rtdb.asia-southeast1.firebasedatabase.app
const char* FIREBASE_URL  = &quot;https://ISI-PROJECT-default-rtdb.asia-southeast1.firebasedatabase.app&quot;;
// ================================================================

// ---------- Pin ----------
#define IR_GATE_IN   32
#define IR_GATE_OUT  33
#define IR_SLOT_A    34
#define IR_SLOT_B    35
#define SERVO_IN     13
#define SERVO_OUT    27

// Sensor FC-51: LOW = ada objek terdeteksi
#define OBJECT_LEVEL LOW

// Sudut servo
#define SUDUT_TUTUP  0
#define SUDUT_BUKA   90

// Pengaturan waktu
const unsigned long WAKTU_GATE_TERBUKA = 4000;   // gate terbuka berapa lama (ms)
const unsigned long DEBOUNCE_GATE_MS   = 2000;   // jeda antar-deteksi gate
const unsigned long DEBOUNCE_SLOT_MS   = 1500;   // slot harus stabil sekian ms
const unsigned long HEARTBEAT_MS       = 30000;  // kirim ulang tiap 30 detik
const unsigned long WIFI_RETRY_MS      = 10000;

Servo servoIn;
Servo servoOut;

// State slot (dengan debounce, sama seperti versi monitoring sebelumnya)
bool stableA = false, stableB = false;
bool lastRawA = false, lastRawB = false;
unsigned long changeAtA = 0, changeAtB = 0;

// State gate
String statusGateIn  = &quot;tertutup&quot;;
String statusGateOut = &quot;tertutup&quot;;

unsigned long lastTriggerIn  = 0;
unsigned long lastTriggerOut = 0;
unsigned long lastSend       = 0;
unsigned long lastCmdCheck   = 0;
bool needSend = true;

// ---------- Fungsi bantu ----------
bool adaObjek(uint8_t pin) {
  return digitalRead(pin) == OBJECT_LEVEL;
}

void connectWiFi() {
  if (WiFi.status() == WL_CONNECTED) return;

  Serial.print(&quot;Menyambung WiFi&quot;);
  WiFi.mode(WIFI_STA);
  WiFi.begin(WIFI_SSID, WIFI_PASSWORD);

  unsigned long start = millis();
  while (WiFi.status() != WL_CONNECTED &amp;&amp; millis() - start &lt; WIFI_RETRY_MS) {
    delay(400);
    Serial.print(&quot;.&quot;);
  }

  if (WiFi.status() == WL_CONNECTED) {
    Serial.println(&quot;\nWiFi terhubung! IP: &quot; + WiFi.localIP().toString());
  } else {
    Serial.println(&quot;\nGagal, akan dicoba lagi.&quot;);
  }
}

// Kirim seluruh status ke Firebase (PATCH: tidak menimpa gateIn milik micro:bit)
bool sendToFirebase() {
  if (WiFi.status() != WL_CONNECTED) return false;

  int available = (!stableA ? 1 : 0) + (!stableB ? 1 : 0);
  unsigned long now = millis();

  String json = &quot;{&quot;;
  json += &quot;\&quot;gateOut\&quot;:{\&quot;status\&quot;:\&quot;&quot; + statusGateOut + &quot;\&quot;,\&quot;updated\&quot;:&quot; + String(now) + &quot;},&quot;;
  json += &quot;\&quot;slotA\&quot;:{\&quot;occupied\&quot;:&quot; + String(stableA ? &quot;true&quot; : &quot;false&quot;) + &quot;,\&quot;updated\&quot;:&quot; + String(now) + &quot;},&quot;;
  json += &quot;\&quot;slotB\&quot;:{\&quot;occupied\&quot;:&quot; + String(stableB ? &quot;true&quot; : &quot;false&quot;) + &quot;,\&quot;updated\&quot;:&quot; + String(now) + &quot;},&quot;;
  json += &quot;\&quot;available\&quot;:&quot; + String(available) + &quot;,&quot;;
  json += &quot;\&quot;total\&quot;:2&quot;;
  json += &quot;}&quot;;

  WiFiClientSecure client;
  client.setInsecure();  // skip verifikasi sertifikat, cukup untuk prototipe

  HTTPClient http;
  String url = String(FIREBASE_URL) + &quot;/parking.json&quot;;

  if (!http.begin(client, url)) {
    Serial.println(&quot;HTTP begin gagal&quot;);
    return false;
  }

  http.addHeader(&quot;Content-Type&quot;, &quot;application/json&quot;);
  int code = http.PATCH(json);  // PATCH: hanya ubah field milik ESP32
  http.end();

  Serial.printf(&quot;Kirim ke Firebase -&gt; HTTP %d\n&quot;, code);
  return (code == 200);
}


// Tulis log aktivitas ke Firebase (/logs)
void kirimLog(const String &amp;event) {
  if (WiFi.status() != WL_CONNECTED) return;
  WiFiClientSecure client;
  client.setInsecure();
  HTTPClient http;
  if (!http.begin(client, String(FIREBASE_URL) + &quot;/logs.json&quot;)) return;
  http.addHeader(&quot;Content-Type&quot;, &quot;application/json&quot;);
  http.POST(&quot;{\&quot;time\&quot;:&quot; + String(millis()) + &quot;,\&quot;source\&quot;:\&quot;esp32\&quot;,\&quot;event\&quot;:\&quot;&quot; + event + &quot;\&quot;}&quot;);
  http.end();
}

// Cek perintah buka Gate OUT dari website (/parking/cmd/gateOut == &quot;open&quot;)
void cekPerintahWebsite() {
  if (WiFi.status() != WL_CONNECTED) return;
  String url = String(FIREBASE_URL) + &quot;/parking/cmd/gateOut.json&quot;;
  String body = &quot;&quot;;
  {
    WiFiClientSecure client;
    client.setInsecure();
    HTTPClient http;
    if (!http.begin(client, url)) return;
    if (http.GET() == 200) body = http.getString();
    http.end();
  }
  if (body.indexOf(&quot;open&quot;) &lt; 0) return;
  {
    WiFiClientSecure client;
    client.setInsecure();
    HTTPClient http;
    if (http.begin(client, url)) { http.sendRequest(&quot;DELETE&quot;); http.end(); }  // hapus perintah
  }
  kirimLog(&quot;Gate OUT dibuka dari website&quot;);
  bukaGate(servoOut, statusGateOut, &quot;GATE OUT&quot;);
}

// Buka gate, tunggu, lalu tutup lagi. Status dikirim sebelum &amp; sesudah.
void bukaGate(Servo &amp;servo, String &amp;statusVar, const char* label) {
  Serial.print(label);
  Serial.println(&quot;: mobil terdeteksi, portal membuka&quot;);

  statusVar = &quot;terbuka&quot;;
  sendToFirebase();

  servo.write(SUDUT_BUKA);
  delay(WAKTU_GATE_TERBUKA);
  servo.write(SUDUT_TUTUP);

  statusVar = &quot;tertutup&quot;;
  sendToFirebase();

  Serial.print(label);
  Serial.println(&quot;: portal menutup kembali&quot;);
}

// ---------- Setup ----------
void setup() {
  Serial.begin(115200);
  delay(300);

  pinMode(IR_GATE_IN, INPUT);
  pinMode(IR_GATE_OUT, INPUT);
  pinMode(IR_SLOT_A, INPUT);
  pinMode(IR_SLOT_B, INPUT);

  ESP32PWM::allocateTimer(0);
  ESP32PWM::allocateTimer(1);
  servoIn.setPeriodHertz(50);
  servoOut.setPeriodHertz(50);
  servoIn.attach(SERVO_IN, 500, 2400);
  servoOut.attach(SERVO_OUT, 500, 2400);
  servoIn.write(SUDUT_TUTUP);
  servoOut.write(SUDUT_TUTUP);

  stableA = lastRawA = adaObjek(IR_SLOT_A);
  stableB = lastRawB = adaObjek(IR_SLOT_B);

  Serial.println(&quot;\n=== SMART PARKING AI - ESP32 SIAP ===&quot;);
  connectWiFi();
  sendToFirebase();
}

// ---------- Loop ----------
void loop() {
  unsigned long now = millis();

  if (WiFi.status() != WL_CONNECTED) {
    connectWiFi();
  }

  // Gate IN dikendalikan micro:bit (via website), bukan ESP32.

  // ----- Perintah dari website -----
  if (now - lastCmdCheck &gt; 1500) {
    lastCmdCheck = now;
    cekPerintahWebsite();
  }

  // ----- Gate OUT -----
  if (adaObjek(IR_GATE_OUT) &amp;&amp; (now - lastTriggerOut &gt; DEBOUNCE_GATE_MS)) {
    lastTriggerOut = now;
    bukaGate(servoOut, statusGateOut, &quot;GATE OUT&quot;);
  }

  // ----- Slot A (debounce) -----
  bool rawA = adaObjek(IR_SLOT_A);
  if (rawA != lastRawA) { lastRawA = rawA; changeAtA = now; }
  if (rawA != stableA &amp;&amp; now - changeAtA &gt;= DEBOUNCE_SLOT_MS) {
    stableA = rawA;
    needSend = true;
    Serial.printf(&quot;Slot A: %s\n&quot;, stableA ? &quot;TERISI&quot; : &quot;KOSONG&quot;);
    sendToFirebase();
    kirimLog(stableA ? &quot;Slot A terisi&quot; : &quot;Slot A kosong&quot;);
  }

  // ----- Slot B (debounce) -----
  bool rawB = adaObjek(IR_SLOT_B);
  if (rawB != lastRawB) { lastRawB = rawB; changeAtB = now; }
  if (rawB != stableB &amp;&amp; now - changeAtB &gt;= DEBOUNCE_SLOT_MS) {
    stableB = rawB;
    needSend = true;
    Serial.printf(&quot;Slot B: %s\n&quot;, stableB ? &quot;TERISI&quot; : &quot;KOSONG&quot;);
    sendToFirebase();
    kirimLog(stableB ? &quot;Slot B terisi&quot; : &quot;Slot B kosong&quot;);
  }

  // ----- Kirim jika ada perubahan atau waktunya heartbeat -----
  if (needSend || now - lastSend &gt;= HEARTBEAT_MS) {
    if (sendToFirebase()) needSend = false;
    lastSend = now;
  }

  delay(50);
}
</pre>
</div>
<div class="page tab" id="tab-guide">
  <h2>Panduan pemasangan</h2>
  <ol>
    <li>Firebase: buat Realtime Database, atur Rules ke mode test (read dan write true), lalu import <b>firebase_import_contoh.json</b>.</li>
    <li>Isi <b>FIREBASE_URL</b>, <b>MODEL_URL</b>, dan <b>CLASS_CAR</b> di bagian CFG pada file index.html ini, lalu deploy ke Vercel.</li>
    <li>Upload kode ESP32 (tab Kode ESP32). Wiring: IR Gate OUT GPIO 33, IR Slot A GPIO 34, IR Slot B GPIO 35, servo OUT GPIO 27 dengan daya 5V eksternal.</li>
    <li>Download kode micro:bit (tab Kode micro:bit). Wiring: servo P0 dengan daya 5V eksternal, IR Gate IN ke P1.</li>
    <li>Buka website di Chrome atau Edge, klik <b>Sambungkan micro:bit</b>, lalu <b>Aktifkan kamera</b>. Biarkan centang otomatis menyala.</li>
    <li>Tes: arahkan mobil ke kamera. Gate IN terbuka, lalu menutup sendiri setelah 4 detik dan mobil sudah lewat sensor.</li>
  </ol>
  <p class="hint">Password Wi-Fi sengaja tidak ditulis di halaman ini supaya tidak terbaca publik.</p>
</div>

<script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@1.3.1/dist/tf.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/@teachablemachine/image@0.8/dist/teachablemachine-image.min.js"></script>
<script>
// ================== ISI BAGIAN INI ==================
const CFG = {
  FIREBASE_URL: "https://ISI-PROJECT-default-rtdb.asia-southeast1.firebasedatabase.app", // tanpa "/" di akhir
  MODEL_URL: "https://teachablemachine.withgoogle.com/models/ISI_ID_MODEL/",            // dengan "/" di akhir
  CLASS_CAR: "Mobil",     // nama kelas mobil di Teachable Machine (harus sama persis)
  MIN_PROB: 0.85,         // keyakinan minimum
  MIN_FRAMES: 6,          // berapa kali berturut-turut (~0,2 dtk per kali)
  COOLDOWN_MS: 10000,     // jeda antar pembukaan otomatis
  REQUIRE_IR_IN: false    // true = gate hanya buka jika sensor IR gate juga mendeteksi mobil
};
// ====================================================

const $ = id => document.getElementById(id);
const FB = CFG.FIREBASE_URL;
let S = {}, model, webcam, camOn = false, hits = 0, lastAuto = 0;
const ble = { connected: false, rx: null, ir: false, dev: null };

const chip = (k, on) => $("chip-" + k).classList.toggle("on", on);
const fb = (p, m = "GET", body) => fetch(FB + p + ".json", {
  method: m, headers: { "Content-Type": "application/json" },
  body: body === undefined ? undefined : JSON.stringify(body)
}).then(r => r.json());

async function log(source, event) {
  try { await fb("/logs", "POST", { time: Date.now(), source, event }); loadLogs(); } catch (e) {}
}

async function loadLogs() {
  try {
    const r = await fetch(FB + '/logs.json?orderBy="$key"&limitToLast=25');
    const o = await r.json() || {};
    const rows = Object.keys(o).sort().reverse().map(k => o[k]);
    $("log").innerHTML = rows.map(l => {
      const t = l.time > 1e12 ? new Date(l.time).toLocaleTimeString("id-ID") : "";
      return `<li><em>${t}</em><span>${l.source}: ${l.event}</span></li>`;
    }).join("") || "<li>Belum ada aktivitas.</li>";
  } catch (e) {}
}

function render() {
  for (const k of ["slotA", "slotB"]) {
    const full = S[k] && S[k].occupied;
    $(k).className = "bay " + (full ? "full" : "free");
    $(k).innerHTML = `Slot ${k.slice(-1)}<b>${full ? "Terisi" : "Kosong"}</b>`;
  }
  $("count").textContent = `Tersedia ${S.available ?? "-"} dari ${S.total ?? "-"}`;
  for (const k of ["gateIn", "gateOut"]) {
    const st = S[k] ? S[k].status : "-";
    $(k).classList.toggle("open", st === "terbuka");
    $(k).querySelector("b").textContent = st;
  }
}

async function refresh() {
  try { S = await fb("/parking") || {}; render(); loadLogs(); } catch (e) {}
}

function listen() {
  const es = new EventSource(FB + "/parking.json");
  es.addEventListener("put", refresh);
  es.addEventListener("patch", refresh);
  es.onopen = () => chip("fb", true);
  es.onerror = () => chip("fb", false);
}

// ---------- Bluetooth micro:bit ----------
const UART = "6e400001-b5a3-f393-e0a9-e50e24dcca9e";
const RX = "6e400002-b5a3-f393-e0a9-e50e24dcca9e";
const TX = "6e400003-b5a3-f393-e0a9-e50e24dcca9e";

async function connectBle() {
  if (!navigator.bluetooth) { alert("Browser ini tidak mendukung Web Bluetooth. Pakai Chrome/Edge di laptop atau Android."); return; }
  try {
    ble.dev = await navigator.bluetooth.requestDevice({ filters: [{ namePrefix: "BBC micro:bit" }], optionalServices: [UART] });
    ble.dev.addEventListener("gattserverdisconnected", () => { ble.connected = false; chip("ble", false); log("website", "Bluetooth micro:bit terputus"); });
    const svc = await (await ble.dev.gatt.connect()).getPrimaryService(UART);
    ble.rx = await svc.getCharacteristic(RX);
    const tx = await svc.getCharacteristic(TX);
    await tx.startNotifications();
    let buf = "";
    tx.addEventListener("characteristicvaluechanged", e => {
      buf += new TextDecoder().decode(e.target.value);
      let i;
      while ((i = buf.indexOf("\n")) >= 0) { onMsg(buf.slice(0, i).trim()); buf = buf.slice(i + 1); }
    });
    ble.connected = true; chip("ble", true);
    log("microbit", "Bluetooth tersambung");
  } catch (e) { console.warn(e); }
}

function sendBle(cmd) {
  if (!ble.connected) { alert("micro:bit belum tersambung."); return false; }
  ble.rx.writeValueWithoutResponse(new TextEncoder().encode(cmd + "\n"));
  return true;
}

function onMsg(m) {
  if (m === "IN:OPEN" || m === "IN:CLOSED") {
    const status = m === "IN:OPEN" ? "terbuka" : "tertutup";
    fb("/parking/gateIn", "PATCH", { status, updated: Date.now() });
    log("microbit", "Gate IN " + status);
  } else if (m === "IN:IR:1" || m === "IN:IR:0") {
    ble.ir = m.endsWith("1");
    if (ble.ir) log("microbit", "Sensor IR Gate IN mendeteksi mobil");
  }
}

// ---------- Teachable Machine ----------
async function startCam() {
  $("btnCam").disabled = true;
  try {
    model = await tmImage.load(CFG.MODEL_URL + "model.json", CFG.MODEL_URL + "metadata.json");
    webcam = new tmImage.Webcam(320, 240, true);
    await webcam.setup(); await webcam.play();
    $("cam").appendChild(webcam.canvas);
    camOn = true; chip("cam", true);
    log("website", "Kamera & model Teachable Machine aktif");
    loop();
  } catch (e) {
    $("btnCam").disabled = false;
    alert("Kamera/model gagal dimuat. Cek izin kamera dan MODEL_URL.");
  }
}

async function loop() {
  if (!camOn) return;
  webcam.update();
  const preds = await model.predict(webcam.canvas);
  $("bars").innerHTML = preds.map(p =>
    `<div class="bar"><span>${p.className}</span><i style="width:${(p.probability * 100).toFixed(0)}%"></i><span>${(p.probability * 100).toFixed(0)}%</span></div>`).join("");
  const car = preds.find(p => p.className === CFG.CLASS_CAR);
  hits = car && car.probability >= CFG.MIN_PROB ? hits + 1 : 0;
  if ($("auto").checked && hits >= CFG.MIN_FRAMES) autoOpenIn();
  setTimeout(loop, 200);
}

function autoOpenIn() {
  const now = Date.now();
  if (now - lastAuto < CFG.COOLDOWN_MS || !ble.connected) return;
  if (CFG.REQUIRE_IR_IN && !ble.ir) return;
  lastAuto = now;
  if ((S.available ?? 0) <= 0) { log("website", "Parkir penuh, Gate IN tetap tertutup"); return; }
  log("website", "AI mendeteksi mobil, Gate IN dibuka otomatis");
  sendBle("IN:OPEN");
}

// ---------- Tombol ----------
$("btnBle").onclick = connectBle;
$("btnCam").onclick = startCam;
$("btnInOpen").onclick = () => { if (sendBle("IN:OPEN")) log("website", "Gate IN dibuka manual"); };
$("btnInClose").onclick = () => { if (sendBle("IN:CLOSE")) log("website", "Gate IN ditutup manual"); };
$("btnOutOpen").onclick = async () => {
  await fb("/parking/cmd/gateOut", "PUT", "open");   // dibaca ESP32
  log("website", "Perintah buka Gate OUT dikirim");
};

// ---------- Tab, salin, unduh ----------
document.querySelectorAll("#nav button").forEach(b => b.onclick = () => {
  document.querySelectorAll("#nav button").forEach(x => x.classList.toggle("on", x === b));
  document.querySelectorAll(".tab").forEach(t => t.classList.toggle("show", t.id === "tab-" + b.dataset.t));
});
document.querySelectorAll("[data-copy]").forEach(b => b.onclick = async () => {
  await navigator.clipboard.writeText($(b.dataset.copy).textContent);
  const t = b.textContent; b.textContent = "Tersalin"; setTimeout(() => b.textContent = t, 1500);
});
document.querySelectorAll("[data-dl]").forEach(b => b.onclick = () => {
  const a = document.createElement("a");
  a.href = URL.createObjectURL(new Blob([$(b.dataset.dl).textContent], { type: "text/plain" }));
  a.download = b.dataset.name; a.click();
});

refresh(); listen();
</script>
</body>
</html>
