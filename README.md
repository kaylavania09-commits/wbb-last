// Konfigurasi Pin
let IR_GATE_IN = DigitalPin.P0
let SERVO_PIN = AnalogPin.P1

let SUDUT_TUTUP = 0
let SUDUT_BUKA = 90
let statusGate = "tertutup"

// Inisialisasi awal
servos.P1.setAngle(SUDUT_TUTUP)
serial.redirect(SerialPin.P14, SerialPin.P15, BaudRate.BaudRate115200) // Default USB
basic.showIcon(IconNames.House)

function bukaGerbang() {
    if (statusGate == "tertutup") {
        statusGate = "terbuka"
        servos.P1.setAngle(SUDUT_BUKA)
        basic.showIcon(IconNames.Yes)
        serial.writeLine("GATE_IN_OPEN")
        
        basic.pause(4000) // Buka selama 4 detik
        
        servos.P1.setAngle(SUDUT_TUTUP)
        statusGate = "tertutup"
        basic.showIcon(IconNames.House)
        serial.writeLine("GATE_IN_CLOSED")
    }
}

// Menerima perintah dari Website
serial.onDataReceived(serial.delimiters(Delimiters.NewLine), function () {
    let cmd = serial.readString().trim()
    if (cmd == "OPEN_GATE_IN") {
        bukaGerbang()
    }
})

// Sensor IR lokal micro:bit
basic.forever(function () {
    if (pins.digitalReadPin(IR_GATE_IN) == 0 && statusGate == "tertutup") {
        bukaGerbang()
    }
    basic.pause(100)
})
