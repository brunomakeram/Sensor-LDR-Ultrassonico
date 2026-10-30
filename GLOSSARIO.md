# Glossário

Este glossário reúne os principais termos e conceitos usados no projeto de luz automática com Arduino Uno, sem alterar a lógica do código original.

# 1. Visão geral do projeto

## 1.1 `LDR`
É o resistor dependente da luz, usado para medir a luminosidade do ambiente. Em conjunto com uma resistência fixa, forma um divisor de tensão.
Aparece em: `#define PINO_LDR A0` e na descrição do projeto na `README.md`.
Exemplo: `Valor_Ldr = analogRead(PINO_LDR);`.
Referência: https://en.wikipedia.org/wiki/Photoresistor

## 1.2 `HC-SR04`
É o sensor ultrassônico usado para medir a distância até um objeto. Ele envia um pulso em `TRIG` e mede o tempo do eco em `ECHO`.
Aparece em: `#define PINO_TRIG 4` e `#define PINO_ECHO 8`.
Exemplo: `Duracao_Sensor = pulseIn(PINO_ECHO, HIGH);`.
Referência: https://docs.arduino.cc/learn/electronics/hc-sr04/

## 1.3 `histerese`
É o mecanismo que impede que o LED fique alternando entre ligado e desligado na borda da faixa de detecção.
Aparece em: `if (Distancia_Sensor >= 10 && Distancia_Sensor <= 40)`, `else if (Distancia_Sensor >= 60)` e no texto da `README.md`.
Exemplo: `Presenca_Sensor` só muda para `true` ou `false` fora da faixa de transição.
Referência: https://en.wikipedia.org/wiki/Hysteresis

# 2. Pinos e sinais

## 2.1 `PINO_LDR`
É o pino analógico usado para ler a luminosidade captada pelo LDR.
Aparece em: `#define PINO_LDR A0`.
Exemplo: `Valor_Ldr = analogRead(PINO_LDR);`.

## 2.2 `PINO_LED`
É o pino digital onde o LED é conectado.
Aparece em: `#define PINO_LED 2`.
Exemplo: `digitalWrite(PINO_LED, HIGH);`.

## 2.3 `PINO_TRIG`
É o pino digital do sensor ultrassônico responsável por disparar o pulso sonoro.
Aparece em: `#define PINO_TRIG 4`.
Exemplo: `digitalWrite(PINO_TRIG, HIGH);`.

## 2.4 `PINO_ECHO`
É o pino digital do sensor ultrassônico responsável por receber o eco do pulso enviado.
Aparece em: `#define PINO_ECHO 8`.
Exemplo: `Duracao_Sensor = pulseIn(PINO_ECHO, HIGH);`.

# 3. Variáveis principais

## 3.1 `Valor_Ldr`
Armazena o valor lido pelo LDR no pino analógico.
Aparece em: `int Valor_Ldr = 0;`.
Exemplo: `Valor_Ldr = analogRead(PINO_LDR);`.

## 3.2 `Duracao_Sensor`
Guarda o tempo em microssegundos que o pulso levou para voltar do sensor ultrassônico.
Aparece em: `long Duracao_Sensor = 0;`.
Exemplo: `Duracao_Sensor = pulseIn(PINO_ECHO, HIGH);`.

## 3.3 `Distancia_Sensor`
É a distância calculada em centímetros a partir da duração do pulso do ultrassônico.
Aparece em: `float Distancia_Sensor = 0;`.
Exemplo: `Distancia_Sensor = Duracao_Sensor * 0.0343 / 2;`.

## 3.4 `Estado_Led`
Representa o estado anterior do LED, usado para detectar quando houve mudança de estado.
Aparece em: `bool Estado_Led = false;`.
Exemplo: `if (Novo_Estado_Led != Estado_Led)`.

## 3.5 `Tempo_Anterior`
Guarda o instante da última leitura para controlar o intervalo entre as medições.
Aparece em: `unsigned long Tempo_Anterior = 0;`.
Exemplo: `if (millis() - Tempo_Anterior >= Intervalo)`.

## 3.6 `Intervalo`
É o tempo em milissegundos entre cada ciclo de leitura dos sensores.
Aparece em: `unsigned long Intervalo = 100;`.
Exemplo: `if (millis() - Tempo_Anterior >= Intervalo)`.

## 3.7 `Presenca_Sensor`
Indica se há presença detectada dentro da faixa de distância configurada.
Aparece em: `bool Presenca_Sensor = false;`.
Exemplo: `if (Distancia_Sensor >= 10 && Distancia_Sensor <= 40) Presenca_Sensor = true;`.

## 3.8 `Novo_Estado_Led`
É o estado calculado para o LED após combinar presença e luminosidade.
Aparece em: `bool Novo_Estado_Led = false;`.
Exemplo: `Novo_Estado_Led = Presenca_Sensor && (Valor_Ldr < 500);`.

# 4. Funções do Arduino

## 4.1 `analogRead()`
Lê o valor analógico de um pino, útil para o LDR.
Aparece em: `Valor_Ldr = analogRead(PINO_LDR);`.
Exemplo: `analogRead(PINO_LDR)`.
Referência: https://www.arduino.cc/reference/en/language/functions/analog-io/analogread/

## 4.2 `pulseIn()`
Mede a duração de um pulso em nível HIGH em um pino digital.
Aparece em: `Duracao_Sensor = pulseIn(PINO_ECHO, HIGH);`.
Exemplo: `pulseIn(PINO_ECHO, HIGH)`.
Referência: https://www.arduino.cc/reference/en/language/functions/advanced-io/pulsein/

## 4.3 `delayMicroseconds()`
Gera um curto atraso em microssegundos, usado para enviar o pulso do sensor ultrassônico.
Aparece em: `delayMicroseconds(2);` e `delayMicroseconds(10);`.
Exemplo: `digitalWrite(PINO_TRIG, HIGH); delayMicroseconds(10);`.
Referência: https://www.arduino.cc/reference/en/language/functions/time/delaymicroseconds/

## 4.4 `millis()`
Retorna o tempo em milissegundos desde o início do programa, permitindo o controle sem bloquear o código com `delay()`.
Aparece em: `if (millis() - Tempo_Anterior >= Intervalo)`.
Exemplo: `Tempo_Anterior = millis();`.
Referência: https://www.arduino.cc/reference/en/language/functions/time/millis/

# 5. Lógica de decisão

## 5.1 `Presenca detectada`
É a condição em que a distância está entre 10 cm e 40 cm e a pessoa está próxima do sensor.
Aparece em: `if (Distancia_Sensor >= 10 && Distancia_Sensor <= 40) { Presenca_Sensor = true; }`.

## 5.2 `Presenca desligada`
É a condição em que a distância fica em 60 cm ou mais e o sensor considera que não há mais pessoa próxima.
Aparece em: `else if (Distancia_Sensor >= 60) { Presenca_Sensor = false; }`.

## 5.3 `ambiente escuro`
É a condição em que o valor lido pelo LDR está abaixo de 500.
Aparece em: `Novo_Estado_Led = Presenca_Sensor && (Valor_Ldr < 500);`.

# 6. Ciclo principal

## 6.1 `setup()`
Configura a comunicação serial, o LED e os pinos do sensor ultrassônico.
Aparece em: `void setup() { ... }`.

## 6.2 `loop()`
Executa continuamente as leituras dos sensores e decide se o LED deve ficar aceso ou apagado.
Aparece em: `void loop() { ... }`.

## 6.3 `Serial.print()` / `Serial.println()`
São usados para enviar mensagens ao Monitor Serial com os valores e o estado final do LED.
Aparece em: `Serial.print("Presenca detectada - Distancia: ");` e `Serial.println(" - LED ligado");`.
Referência: https://www.arduino.cc/reference/en/language/functions/communication/serial/
