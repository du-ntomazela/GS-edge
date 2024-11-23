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
