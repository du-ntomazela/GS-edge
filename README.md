# **Verificador de Condições para Energia Solar com Produção de Hidrogênio Verde**

## **Descrição**
Este projeto utiliza um **ESP32** para verificar se um local é adequado para a instalação de sistemas de energia solar. Um potenciômetro é utilizado como entrada para simular um sensor de radiação UV, determinando os índices UV e avaliando as condições. A energia gerada pelos painéis solares será direcionada para a eletrólise da água, promovendo a produção sustentável de hidrogênio verde.

Os dados capturados são publicados em um broker MQTT para monitoramento remoto, e um LED indica visualmente os níveis simulados de radiação UV, alertando condições críticas.

---

## Link do vídeo 
https://youtu.be/vNYo_T2LGao

## Link da simulação no WOWKI
https://wokwi.com/projects/414992047973644289

---

## Código Fonte C++​

#include <WiFi.h>
#include <PubSubClient.h>

// Configurações de Wi-Fi
const char* wifiRede = "Wokwi-GUEST";       // Nome da rede Wi-Fi
const char* wifiSenha = "";                // Senha da rede Wi-Fi

// Configuração do MQTT
const char* servidorMQTT = "mqtt-dashboard.com"; // Endereço do broker MQTT
const int portaMQTT = 1883;                      // Porta MQTT
const char* topicoMQTT = "global-solutions/1";   // Tópico para publicar os dados

WiFiClient wifiCliente;
PubSubClient mqttCliente(wifiCliente);

// Pinos
const int pinoLED = 5;
const int sensorUV = 11;

void conectarWiFi() {
  Serial.print("Conectando ao Wi-Fi");
  WiFi.begin(wifiRede, wifiSenha);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("\nWi-Fi conectado!");
}

void conectarMQTT() {
  while (!mqttCliente.connected()) {
    Serial.print("Conectando ao broker MQTT...");
    if (mqttCliente.connect("ESP32Cliente")) {
      Serial.println("Conectado!");
    } else {
      Serial.print("Falha na conexão MQTT (rc=");
      Serial.print(mqttCliente.state());
      Serial.println("). Tentando novamente em 5 segundos.");
      delay(5000);
    }
  }
}

void setup() {
  pinMode(pinoLED, OUTPUT);
  pinMode(sensorUV, INPUT);
  Serial.begin(9600);

  // Conecta ao Wi-Fi
  conectarWiFi();

  // Configura MQTT
  mqttCliente.setServer(servidorMQTT, portaMQTT);
  conectarMQTT();
}

void loop() {
  if (!mqttCliente.connected()) {
    conectarMQTT();
  }
  mqttCliente.loop();

  // Leitura e cálculo do valor UV
  int valorBruto = analogRead(sensorUV);
  float indiceUV = map(valorBruto, 0, 4095, 0, 18);

  // Define o status UV e controla o LED
  const char* statusUV = (indiceUV < 2) ? "Baixo" :
                         (indiceUV < 6) ? "Moderado" : "Alto";
  digitalWrite(pinoLED, (indiceUV >= 6) ? HIGH : LOW);

  // Cria uma mensagem formatada
  char mensagem[100];
  sprintf(mensagem, "Valor Bruto: %d, Índice UV: %.1f, Status: %s", valorBruto, indiceUV, statusUV);

  // Publica os dados no broker MQTT
  if (mqttCliente.publish(topicoMQTT, mensagem)) {
    Serial.println("Mensagem publicada com sucesso:");
    Serial.println(mensagem);
  } else {
    Serial.println("Falha ao publicar mensagem.");
  }

  delay(1000); // Aguarda 1 segundo antes da próxima leitura
}

---

## **Funcionalidades**
- Simulação do índice UV utilizando um potenciômetro.
- Publicação de dados no broker MQTT, incluindo:
  - Valor bruto da entrada analógica.
  - Índice UV calculado.
  - Status do índice UV (Baixo, Moderado, Alto).
- Indicação visual do nível de UV por LED:
  - **Baixo/Moderado:** LED desligado.
  - **Alto:** LED aceso.
- Conexão com redes Wi-Fi para comunicação MQTT.

---

## **Requisitos**

### **Hardware**
- ESP32
- Potenciômetro (conectado ao pino analógico `11`)
- LED (conectado ao pino digital `5`)
- Fonte de alimentação para o ESP32.

### **Bibliotecas Arduino**
Certifique-se de instalar as seguintes bibliotecas na IDE Arduino:
- **WiFi.h**: Para conexão Wi-Fi.
- **PubSubClient.h**: Para comunicação MQTT.

---

## **Instalação**
1. Clone este repositório ou copie o código para a sua IDE Arduino.
2. Configure a rede Wi-Fi editando as variáveis `wifiRede` e `wifiSenha` no código.
3. Verifique as conexões do potenciômetro e do LED no ESP32:
   - LED no pino `5` (GPIO5).
   - Potenciômetro no pino `11` (GPIO11).
4. Faça o upload do código para o ESP32 utilizando a IDE Arduino.

---

## **Como Usar**
1. Após o upload do código, conecte o ESP32 à alimentação.
2. O ESP32 irá:
   - Conectar à rede Wi-Fi configurada.
   - Publicar os dados do índice UV no broker MQTT configurado (`mqtt-dashboard.com`) no tópico `global-solutions/1`.
   - Indicar o nível de UV por meio do LED:
     - **Baixo/Moderado:** LED apagado.
     - **Alto:** LED aceso.
3. Monitore os dados UV simulados pelo potenciômetro via broker MQTT.

---

## **Configuração MQTT**
O código está configurado para utilizar o seguinte broker MQTT público:

- **Broker:** `mqtt-dashboard.com`
- **Porta:** `1883`
- **Tópico:** `global-solutions/1`

Se necessário, altere o broker e o tópico nas variáveis `servidorMQTT` e `topicoMQTT`.

---

## **Tecnologias Utilizadas**
- **Linguagem:** C++ com Arduino Framework.
- **Hardware:** ESP32.
- **Protocolos:** Wi-Fi, MQTT.
- **Bibliotecas:** WiFi.h, PubSubClient.h.

---

## **Contribuição**
Contribuições são bem-vindas! Sinta-se à vontade para abrir um pull request ou enviar sugestões por meio das issues do repositório.

---

## **Licença**
Este projeto é distribuído sob a licença MIT. Veja o arquivo [LICENSE](LICENSE) para mais detalhes.

---

## **Autores**
- **Eduardo Tomazela** - RM: 556807  
- **Léo Masago** - RM: 557768  
- **Luiz Henrique** - RM: 555235  

Engenheiros em formação pela FIAP, com foco em soluções tecnológicas para sustentabilidade e energia limpa.
