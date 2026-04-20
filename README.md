# bit box - 네오픽셀 LED 확장 블록

마이크로비트용 네오픽셀(WS2812B) LED 제어 확장 블록입니다.  

## 기본 사용법

```blocks
// P0 핀에 연결된 8개의 RGB(GRB 형식) 라이트 초기화
let strip = bitbox.create(DigitalPin.P0, 8, NeoPixelMode.RGB)

// 모든 라이트를 빨간색으로 켜기
strip.showColor(bitbox.colors(NeoPixelColors.Red))

// 특정 위치 라이트 색상 설정 후 표시
strip.setPixelColor(0, bitbox.colors(NeoPixelColors.Blue))
strip.show()

// 모든 라이트 끄기
strip.clear()
```

## 빛박 제어(기초)

| 블록 | 설명 |
|---|---|
| `라이트를 모두 ~ 으로 켜기` | 전체 LED를 지정 색상으로 켭니다 |
| `라이트 모두 끄기` | 전체 LED를 끕니다 |
| `라이트의 밝기를 ~ 로 변경하기` | 밝기를 0~255로 설정합니다 |
| `~번째부터 ~개의 라이트` | LED 일부 구간을 range 변수로 선택합니다 |

```blocks
let strip = bitbox.create(DigitalPin.P0, 8, NeoPixelMode.RGB)

// 밝기 설정
strip.setBrightness(128)

// 앞 4개만 초록색으로 켜기
let range = strip.createRange(0, 4)
range.showColor(bitbox.colors(NeoPixelColors.Green))
```

## 빛박 제어(심화)

| 블록 | 설명 |
|---|---|
| `라이트를 설정한대로 켜기` | setPixelColor 등 설정 후 화면에 반영합니다 |
| `~번째 라이트 색상을 ~ 으로 설정하기` | 개별 LED 색상을 설정합니다 |
| `라이트 무지개 효과` | 전체 LED에 무지개 그라데이션을 표시합니다 |
| `라이트 그래프 효과` | 값에 따라 막대 그래프로 표시합니다 |
| `라이트 ~칸 이동` | LED를 한 방향으로 이동시킵니다 |
| `라이트 ~칸 회전` | LED를 회전시킵니다 |

```blocks
let strip = bitbox.create(DigitalPin.P0, 8, NeoPixelMode.RGB)

// 무지개 효과
strip.showRainbow(1, 360)

// 개별 픽셀 설정 후 표시
strip.setPixelColor(3, bitbox.colors(NeoPixelColors.Yellow))
strip.show()

// 가속도 센서 값으로 그래프 표시
strip.showBarGraph(input.acceleration(Dimension.X), 1023)

// 라이트 회전 반복
basic.forever(function () {
    strip.rotate(1)
    strip.show()
    basic.pause(100)
})
```

## 색상 블록

```blocks
// 색상 이름으로 지정
let c1 = bitbox.colors(NeoPixelColors.Red)

// RGB 직접 입력
let c2 = bitbox.rgb(255, 128, 0)

// 색상 팔레트에서 선택
let c3 = bitbox.pickColor(0xff0000)

// HSL로 색상 만들기
let c4 = bitbox.hsl(120, 99, 50)
```

## AI 데이터 활용

블루투스로 수신한 AI 인식 데이터를 파싱하는 블록입니다.

### 사물인식

```blocks
let data = ""
bluetooth.onBluetoothConnected(function () { })
bluetooth.onUartDataReceived(serial.delimiters(Delimiters.Newline), function () {
    data = bluetooth.uartReadUntil(serial.delimiters(Delimiters.Newline))
    let x = bitbox.parseUARTUnified(data, bitbox.UARTDataType.X, bitbox.ReturnFormat.Number)
    let count = bitbox.parseUARTUnified(data, bitbox.UARTDataType.D, bitbox.ReturnFormat.Number)
})
```

### 컬러인식

```blocks
let r = bitbox.parseColorUnified(data, bitbox.ColorDataType.R, bitbox.ReturnFormat.Number)
let g = bitbox.parseColorUnified(data, bitbox.ColorDataType.G, bitbox.ReturnFormat.Number)
let b = bitbox.parseColorUnified(data, bitbox.ColorDataType.B, bitbox.ReturnFormat.Number)
```

### 얼굴인식

```blocks
let x    = bitbox.parseFaceUnified(data, bitbox.FaceDataType.X)
let yaw  = bitbox.parseFaceUnified(data, bitbox.FaceDataType.Yaw)
let smile = bitbox.parseFaceUnified(data, bitbox.FaceDataType.Smile)
```

### 핸드포즈

패킷 포맷: `L{방향}{속도}R{방향}{속도}` (예: `LF255RB128`)

```blocks
let leftDir   = bitbox.parseHandPoseUnified(data, bitbox.HandDataType.LeftDir, bitbox.ReturnFormat.Number)
let leftSpeed = bitbox.parseHandPoseUnified(data, bitbox.HandDataType.LeftSpeed, bitbox.ReturnFormat.Number)
```

## 지원 환경

* PXT/microbit

## 라이선스

MIT
