# 🤖 Projeto ESP32 — Sensor Ultrassônico com Servo Motor

## 📌 Sobre o projeto

Este projeto foi desenvolvido utilizando um **ESP32**, um **sensor ultrassônico**, um **servo motor** e um **LED**.

O sistema tem como objetivo detectar a presença de um objeto utilizando a distância medida pelo sensor ultrassônico. Quando um objeto é identificado a uma distância de até **100 centímetros**, o servo motor é acionado para abrir e o LED é ligado.

Quando não há nenhum objeto dentro da distância configurada, o servo permanece fechado e o LED permanece desligado.

---

## 🎯 Objetivo

O objetivo do projeto é desenvolver um sistema automatizado utilizando sensores e atuadores conectados ao ESP32.

O projeto permite praticar conceitos de:

- Programação para microcontroladores;
- ESP32;
- Sensor ultrassônico;
- Servo motor;
- LED;
- Leitura de distância;
- Estruturas condicionais;
- Controle de entradas e saídas digitais;
- Comunicação serial;
- Automação.

---

## ⚙️ Funcionamento

O sensor ultrassônico realiza continuamente a medição da distância entre o sensor e um possível objeto.

O sistema funciona da seguinte maneira:

```text
        ┌──────────────────────┐
        │     ESP32            │
        │                      │
        │  Sensor Ultrassônico │
        └──────────┬───────────┘
                   │
                   ▼
             Mede distância
                   │
                   ▼
          ┌─────────────────┐
          │ Distância <=    │
          │    100 cm?      │
          └───────┬─────────┘
                  │
          ┌───────┴───────┐
          │               │
         SIM             NÃO
          │               │
          ▼               ▼
    Servo abre       Servo fecha
    LED ligado       LED desligado
```

### Quando o objeto está a até 100 cm

- O servo motor vai para **90°**;
- O LED é ligado;
- A mensagem "Objeto detectado!" é exibida no monitor serial.

### Quando o objeto está a mais de 100 cm

- O servo motor retorna para **0°**;
- O LED é desligado;
- O sistema informa que não há objeto próximo.

---

# 🔌 Componentes utilizados

| Componente | Quantidade | Função |
|---|---:|---|
| ESP32 | 1 | Controlador principal |
| Sensor ultrassônico HC-SR04 | 1 | Mede a distância |
| Servo motor | 1 | Realiza o movimento de abertura/fechamento |
| LED | 1 | Indica a detecção de um objeto |
| Resistores | Conforme montagem | Proteção do circuito |
| Jumpers | Conforme necessário | Conexão dos componentes |
| Protoboard | 1 | Montagem do circuito |

---

# 📍 Configuração dos pinos

O código utiliza os seguintes pinos do ESP32:

| Componente | Pino ESP32 | Definição no código |
|---|---:|---|
| Servo motor | GPIO 22 | `SERVO_PIN` |
| TRIG do sensor | GPIO 14 | `TRIG_PIN` |
| ECHO do sensor | GPIO 27 | `ECHO_PIN` |
| LED | GPIO 16 | `LED_PIN` |

---

# 📏 Distância para acionamento

A distância utilizada para determinar a abertura do servo está definida no código:

```cpp
#define DISTANCIA_ABERTURA 100
```

O valor representa:

```text
100 centímetros
```

Portanto:

```text
Distância <= 100 cm
        ↓
Objeto detectado
        ↓
Servo = 90°
LED = ligado
```

Enquanto:

```text
Distância > 100 cm
        ↓
Nenhum objeto próximo
        ↓
Servo = 0°
LED = desligado
```

---

# 📡 Sensor ultrassônico

O sensor utiliza dois pinos principais:

### TRIG

Responsável por enviar o pulso ultrassônico.

```cpp
digitalWrite(TRIG_PIN, LOW);
delayMicroseconds(2);

digitalWrite(TRIG_PIN, HIGH);
delayMicroseconds(10);

digitalWrite(TRIG_PIN, LOW);
```

### ECHO

Recebe o retorno do pulso e permite calcular o tempo necessário para o sinal retornar.

```cpp
long duracao = pulseIn(ECHO_PIN, HIGH, 30000);
```

O tempo obtido é utilizado para calcular a distância:

```cpp
float distancia = (duracao * 0.0343) / 2;
```

O resultado é apresentado em centímetros.

---

# 🔄 Controle do servo motor

O servo inicia na posição:

```cpp
meuServo.write(0);
```

Essa posição representa o estado fechado.

Quando um objeto é detectado dentro da distância configurada:

```cpp
meuServo.write(90);
```

O servo é movimentado para **90 graus**, representando a abertura.

---

# 💡 Controle do LED

O LED funciona como indicador visual do sistema.

Quando um objeto é detectado:

```cpp
digitalWrite(LED_PIN, HIGH);
```

O LED é ligado.

Quando nenhum objeto está próximo:

```cpp
digitalWrite(LED_PIN, LOW);
```

O LED é desligado.

---

# 🖥️ Monitor Serial

O projeto utiliza comunicação serial com velocidade de:

```cpp
Serial.begin(9600);
```

Durante a execução, informações são apresentadas no monitor serial.

Exemplo:

```text
Sistema iniciado!

Distancia: 75.43 cm
Objeto detectado!
Abrindo Servo
LED ligado
```

Quando o objeto está distante:

```text
Distancia: 150.21 cm
Nenhum objeto proximo.
Fechando Servo
LED desligado
```

Caso nenhum eco seja recebido:

```text
Nenhum objeto detectado.
```

---

# 🧠 Lógica principal do programa

A lógica utilizada pelo sistema pode ser representada da seguinte maneira:

```text
Início
  ↓
Configura ESP32
  ↓
Configura sensor
  ↓
Configura servo
  ↓
Configura LED
  ↓
Envia pulso ultrassônico
  ↓
Recebe o eco
  ↓
Calcula distância
  ↓
Distância <= 100 cm?
  │
  ├── SIM → Servo 90° + LED ligado
  │
  └── NÃO → Servo 0° + LED desligado
  ↓
Aguarda 100 ms
  ↓
Realiza nova medição
```

---

# 🛠️ Tecnologias utilizadas

- **ESP32**
- **Arduino IDE**
- **C/C++**
- **ESP32Servo**
- Sensor ultrassônico
- Servo motor
- LED
- Comunicação Serial

---

# 📦 Biblioteca utilizada

Para controlar o servo motor, o projeto utiliza a biblioteca:

```cpp
#include <ESP32Servo.h>
```

A biblioteca **ESP32Servo** permite controlar servos utilizando o ESP32.

---

# 💻 Como executar o projeto

## 1. Instalar a Arduino IDE

Instale a Arduino IDE no computador.

## 2. Configurar o ESP32

Adicione o suporte às placas ESP32 na Arduino IDE.

## 3. Instalar a biblioteca

Instale a biblioteca:

```text
ESP32Servo
```

## 4. Montar o circuito

Conecte os componentes de acordo com os pinos definidos no código.

### Resumo das conexões

```text
ESP32 GPIO 22 → Servo
ESP32 GPIO 14 → TRIG
ESP32 GPIO 27 → ECHO
ESP32 GPIO 16 → LED
```

## 5. Abrir o código

Abra o arquivo `.ino` na Arduino IDE.

## 6. Selecionar a placa

Selecione a placa ESP32 correspondente ao modelo utilizado.

## 7. Selecionar a porta

Selecione a porta COM correspondente ao ESP32 conectado.

## 8. Enviar o código

Clique em **Upload** para enviar o programa para o ESP32.

## 9. Abrir o Monitor Serial

Abra o Monitor Serial e configure a velocidade para:

```text
9600 baud
```

---

# 📁 Estrutura do projeto

Uma estrutura simples para o repositório é:

```text
projeto-esp32/
│
├── projeto-esp32.ino
├── README.md
└── imagens/
    └── circuito.jpg
```

Caso seja utilizada uma imagem da montagem, ela pode ser adicionada à pasta `imagens`.

---

# ⚠️ Observações

O sistema depende da leitura correta do sensor ultrassônico.

A distância de acionamento pode ser alterada modificando:

```cpp
#define DISTANCIA_ABERTURA 100
```

Por exemplo, para utilizar uma distância de 50 cm:

```cpp
#define DISTANCIA_ABERTURA 50
```

Também é possível modificar o ângulo de abertura do servo:

```cpp
meuServo.write(90);
```

Por exemplo:

```cpp
meuServo.write(120);
```

---

# 🚀 Possíveis melhorias

Algumas funcionalidades podem ser adicionadas futuramente:

- Display LCD para mostrar a distância;
- Buzzer para indicar a detecção;
- Mais LEDs para indicar diferentes faixas de distância;
- Controle da distância pelo aplicativo;
- Envio dos dados pela internet;
- Integração com Bluetooth;
- Integração com Wi-Fi;
- Sistema de abertura automática;
- Registro das distâncias medidas.

---

# 📚 Conceitos aprendidos

Este projeto permite praticar:

- Programação em C/C++;
- Estruturas `if/else`;
- Variáveis;
- Constantes;
- Funções;
- `setup()` e `loop()`;
- Entrada e saída digital;
- PWM para controle do servo;
- Sensor ultrassônico;
- Cálculo de distância;
- Comunicação Serial;
- Automação com ESP32.

---

# 👩‍💻 Autora

**Thais Costa Patto de Souza**

Projeto acadêmico desenvolvido para fins educacionais.

---

# 📄 Licença

Este projeto foi desenvolvido para fins **acadêmicos e educacionais**.
