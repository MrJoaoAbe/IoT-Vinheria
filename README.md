# IoT Vinheria Agnello

## Descrição do Projeto

A **Vinheria Agnello** sempre se destacou pela sua proximidade com os clientes, oferecendo um atendimento personalizado e de qualidade. Porém, com a chegada da pandemia, o movimento na loja foi drasticamente reduzido, impactando negativamente os negócios.

Diante desse cenário, o Sr. Agnello decidiu investir na transformação digital, criando uma plataforma online para manter as vendas ativas e alcançar novos clientes de forma remota.

Este projeto representa a **página inicial** dessa nova plataforma, com foco em proporcionar uma boa experiência ao usuário e reforçar a identidade da vinheria no meio digital.

---

### Testes 
- O projeto foi testado uma parte no Wokwi
- Foi projetado em um esp32 de forma presencial

---

### Tecnologias Utilizadas
- Arduino IDE
- Postman
- Javascript
- Tailwind
- Tinkercad
- Wokwi
- Visual Studio Code
- Node-RED
- GitHub

---

## Passo a passo para a recriação do Projeto

## Como iniciar um servidor AWS - EC2

### Site AWS - EC2
- Entre no link - https://aws.amazon.com/pt/ec2/,
- Inicie uma instancia ,
- Dê um nome à maquina virtual,
- Selecione Ubuntu como imagem,
- E o tipo t3 como instancia,
- Crie uma par de chaves no formato **PPK**,
- E indique o quanto de memória é necessário na MV.
- Em seguida vá em editar regras de entrada e configure as seguintes portas:
**1883, 1026, 4041, 8666, 27017 e o ICMP para IPV4**  

![Portas a serem liberadas](/PORTAS_A_SEREM_LIBERADAS_EC2.png "Portas a serem liberadas")  

- **SALVE O IP**

### PuTTY
- Dentro do PUTTY insira o **IP** e a **CHAVE**  

![Exemplo de como deve ser preenchido o IP](/puTTY%20inicial.png "Exemplo de como deve ser preenchido o IP")  

![Exemplo de como deve ser preenchido a chave](/puTTY%20chave.png "Exemplo de como deve ser preenchido a Chave")  

- Após isso clique em OPEN e siga os seguintes passos para iniciar o BROCKER
  - sudo apt update 
  - sudo apt-get install net-tools 
  - ifconfig 
  - sudo apt install git
  - sudo apt update
  - sudo apt install apt-transport-https ca-certificates curl software-properties-common
  - curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
  - sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
  - sudo apt update
  - apt-cache policy docker-ce
  - sudo apt install docker-ce
  - sudo systemctl status docker
  - git clone https://github.com/fabiocabrini/fiware
  - cd fiware
  - sudo docker compose up -d 
  - sudo docker stats 

### Postman
- Baixe este arquivo JSON → https://github.com/fabiocabrini/fiware/blob/main/FIWARE%20Descomplicado.postman_collection.json 
- Abra o arquivo JSON dentro do **POSTMAN**
- Substitua o placeholder **URL** pelo **IP** do servidor
- Faça isso para os três arquivos **(GET)** presentes no **POSTMAN**  

![Exemplo HEALTHCHECK](/POSTMAN.png "Exemplo de Healthcheck")  


### NODE-RED
![NODERED](/NODERED.png "NodeRED")  

- BLOCO 1 / MQTT IN
  - Servidor = IP:1883
  - Tópico = /TEF/device001/attrs/p
- BLOCO 2 / JSON
- BLOCO 3 / WRITE FILE
  - Caminho = bpm.txt
  - Ação = Sobrescrever Arquivo
- BLOCO 4 / CHANGE
  - NOME = BPM
  - msg.payload
  - msg.payload.BPM
- BLOCO 5 / CHANGE
  - NOME = timestamp
  - msg.payload
  - msg.payload.timestamp
- BLOCO 6 / HTTP IN
  - Método = GET
  - URL = /bpm
- BLOCO 7 / READ FILE
  - bpm.txt
- BLOCO 8 / HTTP RESPONSE

---

## Materiais Utilizados
- ESP32;
- DHT22;
- LDR;
- LEDS(Vermelho, Verde e Amarelo);
- BUZZER;
- RESISTORES(220 ohms);
- PROTOBOARD;
- JUMPERS

## Bibliotecas Utilizadas
- <ArduinoJson.h> 
- "DHT.h"
- <Wire.h>
- <WiFi.h>
- <PubSubClient.h>

## Como Montar o Projeto

![SimuladorWokwi](/SimuladorWokwi.png "Simulador Wokwi")  


- Conecte aos pinos GND e 5v do ESP32 com jumpers e ligue à PROTOBOARD;
- Ligando as LEDS: 
  - Adicione o LED vermelho na PROTOBOARD, o lado negativo adicone um resistor de 220 ohms e no lado positivo ligue com um jumper até o pino 4 do ESP32,  
  - Adicione o LED amarelo na PROTOBOARD, o lado negativo adicone um resistor de 220 ohms e no lado positivo ligue com um jumper até o pino 16 do ESP32,  
  - Adicione o LED verde na PROTOBOARD, o lado negativo adicone um resistor de 220 ohms e no lado positivo ligue com um jumper até o pino 17 do ESP32,  
- Ligue o BUZZER à PROTOBOARD, o lado negativo conecte ao lado negativo da placa de teste, e o positivo conecte ao pino 14 do ESP32;
- Ligando o LDR:
  - Ligue o pino VCC do LDR à parte positiva da PROTOBOARD,
  - Ligue o pino GND do LDR à uma porta GND do ESP32,
  - Conecte o pino A0 do LDR ao pino 34 do ESP32
- Ligando o DHT:
  - Ligue o VCC do DHT ao pino 3v3 do ESP32,
  - Ligue o pino SDA do DHT ao pino 15 do ESP32,
  - Ligue o GND do DHT em um pino GND do ESP32

---

## Programação

- No Arduino IDE, instale as bibliotecas necessárias

- Placa -> DOIT ESP32 DEVKIT V1
`
**Configure o código para conectar no servidor**
````cpp
const char* default_SSID = "Wokwi-GUEST"; // Nome da rede Wi-Fi
const char* default_PASSWORD = ""; // Senha da rede Wi-Fi
const char* default_BROKER_MQTT = "3.144.236.56"; // IP do Broker MQTT
const int default_BROKER_PORT = 1883; // Porta do Broker MQTT
const char* default_TOPICO_SUBSCRIBE = "/TEF/device001/cmd"; // Tópico MQTT de escuta
const char* default_TOPICO_PUBLISH_1 = "/TEF/device001/attrs"; // Tópico MQTT de envio de informações para Broker
const char* default_TOPICO_PUBLISH_2 = "/TEF/device001/attrs/p"; // Tópico MQTT de envio de informações para Broker
const char* default_ID_MQTT = "fiware_001"; // ID MQTT
const int default_D4 = 2; // Pino do LED onboard
// Declaração da variável para o prefixo do tópico
const char* topicPrefix = "device001";

// Variáveis para configurações editáveis
char* SSID = const_cast<char*>(default_SSID);
char* PASSWORD = const_cast<char*>(default_PASSWORD);
char* BROKER_MQTT = const_cast<char*>(default_BROKER_MQTT);
int BROKER_PORT = default_BROKER_PORT;
char* TOPICO_SUBSCRIBE = const_cast<char*>(default_TOPICO_SUBSCRIBE);
char* TOPICO_PUBLISH_1 = const_cast<char*>(default_TOPICO_PUBLISH_1);
char* TOPICO_PUBLISH_2 = const_cast<char*>(default_TOPICO_PUBLISH_2);
char* ID_MQTT = const_cast<char*>(default_ID_MQTT);
````
**Carregue o código**

- Abra o Serial Monitor para verificar os JSONs enviados para o Node-RED.

## Código ESP32 - Monitoramento UMIDADE / TEMPERATURA / LUMINOSIDADE
`````cpp
#include <ArduinoJson.h> 
#include "DHT.h"
#include <Wire.h>

//Server
#include <WiFi.h>
#include <PubSubClient.h>

// Variáveis para conectar ao servidor
const char* default_SSID = "Wokwi-GUEST"; // Nome da rede Wi-Fi
const char* default_PASSWORD = ""; // Senha da rede Wi-Fi
const char* default_BROKER_MQTT = "3.144.236.56"; // IP do Broker MQTT
const int default_BROKER_PORT = 1883; // Porta do Broker MQTT
const char* default_TOPICO_SUBSCRIBE = "/TEF/device001/cmd"; // Tópico MQTT de escuta
const char* default_TOPICO_PUBLISH_1 = "/TEF/device001/attrs"; // Tópico MQTT de envio de informações para Broker
const char* default_TOPICO_PUBLISH_2 = "/TEF/device001/attrs/p"; // Tópico MQTT de envio de informações para Broker
const char* default_ID_MQTT = "fiware_001"; // ID MQTT
const int default_D4 = 2; // Pino do LED onboard
// Declaração da variável para o prefixo do tópico
const char* topicPrefix = "device001";

// Variáveis para configurações editáveis
char* SSID = const_cast<char*>(default_SSID);
char* PASSWORD = const_cast<char*>(default_PASSWORD);
char* BROKER_MQTT = const_cast<char*>(default_BROKER_MQTT);
int BROKER_PORT = default_BROKER_PORT;
char* TOPICO_SUBSCRIBE = const_cast<char*>(default_TOPICO_SUBSCRIBE);
char* TOPICO_PUBLISH_1 = const_cast<char*>(default_TOPICO_PUBLISH_1);
char* TOPICO_PUBLISH_2 = const_cast<char*>(default_TOPICO_PUBLISH_2);
char* ID_MQTT = const_cast<char*>(default_ID_MQTT);
int D4 = default_D4;

WiFiClient espClient;
PubSubClient MQTT(espClient);
char EstadoSaida = '0';

// Definições de pinos para ESP32
#define LED_VERDE     17
#define LED_AMARELO   16
#define LED_VERMELHO  4
#define BUZZER        14
#define DHTPIN        15       // Pode ser alterado se necessário
#define DHTTYPE       DHT22
#define LDR_PIN       34       // Pino ADC válido para ESP32

DHT dht(DHTPIN, DHTTYPE);

void initSerial(){
  Serial.begin(9600);
}

void reconectWiFi() {
  if (WiFi.status() == WL_CONNECTED) return;
  WiFi.begin(SSID, PASSWORD);
  while (WiFi.status() != WL_CONNECTED) {
    delay(100);
    Serial.print(".");
  }
  Serial.println("\nConectado ao Wi-Fi. IP: " + WiFi.localIP().toString());
  digitalWrite(D4, LOW);
}

void initWiFi() {
  delay(10);
  Serial.println("------Conexao WI-FI------");
  reconectWiFi();
}

void initMQTT() {
  MQTT.setServer(BROKER_MQTT, BROKER_PORT);
  MQTT.setCallback(mqtt_callback);
}

void mqtt_callback(char* topic, byte* payload, unsigned int length) {
  String msg;
  for (int i = 0; i < length; i++) msg += (char)payload[i];
  Serial.println("Mensagem recebida: " + msg);

// definindo um topico publisher se for device00N@on|
  if (msg.equals(String(topicPrefix) + "@buzzer_on|")) {
    digitalWrite(D4, HIGH);
    EstadoSaida = '1';
    digitalWrite(BUZZER, HIGH);
  }
  if (msg.equals(String(topicPrefix) + "@buzzer_off|")) {
    digitalWrite(D4, LOW);
    EstadoSaida = '0';
    digitalWrite(BUZZER, LOW);
  }
}

void reconnectMQTT() {
  while (!MQTT.connected()) {
    Serial.print("Tentando conectar ao broker...");
    if (MQTT.connect(ID_MQTT)) {
      Serial.println("Conectado!");
      MQTT.subscribe(TOPICO_SUBSCRIBE);
    } else {
      Serial.println("Falha. Tentando em 2s.");
      delay(2000);
    }
  }
}

void VerificaConexoesWiFIEMQTT() {
  if (!MQTT.connected()) reconnectMQTT();
  reconectWiFi();
}

void setup() {
  initSerial();

  pinMode(LED_VERDE, OUTPUT);
  pinMode(LED_AMARELO, OUTPUT);
  pinMode(LED_VERMELHO, OUTPUT);
  pinMode(BUZZER, OUTPUT);

  initWiFi();
  initMQTT();

  dht.begin();
}

void loop() {
  VerificaConexoesWiFIEMQTT();
  MQTT.loop();

  float umidade = dht.readHumidity();
  float temperatura = dht.readTemperature();
  int LDR = analogRead(LDR_PIN);

  if (isnan(umidade) || isnan(temperatura)) {
    Serial.println("Erro ao ler o sensor DHT!");
    return;
  }

  Serial.print("Umidade: ");
  Serial.print(umidade, 1);
  Serial.print("% ");

  Serial.print("Temperatura: ");
  Serial.print(temperatura, 1);
  Serial.print(" C ");

  Serial.print("LDR: ");
  Serial.println(LDR);

StaticJsonDocument<256> doc;
doc["umidade"] = umidade;
doc["temperatura"] = temperatura;
doc["LDR"] = LDR;

char buffer[256];
serializeJson(doc, buffer);
Serial.println(buffer);

MQTT.publish(TOPICO_PUBLISH_2, buffer);
Serial.print("Enviado: ");
Serial.println(buffer);

  // Controle de LEDs e Buzzer baseado no valor do LDR
  if (LDR >= 3000) {  // ESP32 tem resolução de 12 bits (0 a 4095)
    digitalWrite(LED_VERDE, HIGH);
    digitalWrite(LED_AMARELO, LOW);
    digitalWrite(LED_VERMELHO, LOW);
    // digitalWrite(BUZZER, LOW);
  } else if (LDR >= 1500 && LDR < 3000) {
    digitalWrite(LED_VERDE, LOW);
    digitalWrite(LED_AMARELO, HIGH);
    digitalWrite(LED_VERMELHO, LOW);
    digitalWrite(BUZZER, HIGH);
    delay(3000);
    // digitalWrite(BUZZER, LOW);
  } else {
    digitalWrite(LED_VERDE, LOW);
    digitalWrite(LED_AMARELO, LOW);
    digitalWrite(LED_VERMELHO, HIGH);
    digitalWrite(BUZZER, HIGH);
    delay(3000);
    // digitalWrite(BUZZER, LOW);
  }

  delay(500);  // Pequeno delay entre as leituras
}

`````
`
## Página WEB com DASHBOARDS personalizados
````cpp
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel="stylesheet" href="css/flex-style.css">
  <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
  <script src="https://cdn.jsdelivr.net/npm/apexcharts"></script>
  <title>Vinheria Agnello</title>
</head>

<body>

  <nav>
    <ul>
      <li id="navTitulo">
        <h2 class="font-bold text-3xl">Vinheria Agnello</h2>
      </li>
      <li id="navHome">
        <h2 class="font-bold text-xl"> HOME</h2>
      </li>
    </ul>
    <div class="navDireita">
      <input type="text" placeholder="Pesquisar...">
      <div class="icone">🍷</div>
    </div>
  </nav>

  <header>
    <h1 class="text-5xl">MONITORAMENTO VINHERIA AGNELLO</h1>
  </header>

  <div class="flex justify-center mt-10 min-h-screen">
    <section class="grid grid-cols-3 grid-rows-3 gap-4 p-4 w-full max-w-6xl">

      <div class="col-span-1 row-span-1 bg-[#F0DACD] flex flex-col justify-center items-center rounded-xl ">
        <div class="flex items-center gap-2">
          <img src="./img/leitor-de-temperatura.png" alt="icone temperatura" class="w-10">
          <h3 class="font-bold text-xl">Temperatura da Vinheria </h3>
        </div>

        <div class="flex items-center gap-2">
          <span id="temperatura" class="font-bold text-6xl">N</span>
          <span class="font-bold text-3xl">°C</span>
        </div>

      </div>
      <div class="col-span-1 row-span-1 bg-[#F0DACD] flex flex-col justify-center items-center rounded-xl">
        <div class="flex items-center gap-2">
          <img src="./img/umidade.png" alt="icone umidade" class="w-10">
          <h3 class="font-bold text-xl">Umidade da Vinheria </h3>
        </div>

        <div class="flex items-center gap-2">
          <span id="umidade" class="font-bold text-6xl">N</span>
          <span class="font-bold text-3xl">%</span>
        </div>
      </div>

      <div class="col-span-1 row-span-1 bg-[#F0DACD] flex flex-col justify-center items-center rounded-xl p-6 relative">
        <h3 class="absolute top-8 left-12 text-black font-bold text-2xl ">
          Gráfico de Umidade
        </h3>
        <div id="chartUmidade"></div>
      </div>

      <div class="col-span-3 row-span-1 bg-[#F0DACD] flex justify-center items-center rounded-xl p-7">
        <div id="graficoLinha" class="w-full"></div>
      </div>
      <div class="col-span-3 row-span-1 bg-[#F0DACD] flex justify-center items-center rounded-xl">
        <div id="graficoLDR" class="w-full"></div>
      </div>

    </section>
  </div>

  <footer>
    <p>Vinheria Agnello • Rua dos Vinhos, 123 – São Paulo – SP</p>
    <p>Telefone: (11) 99999-9999 | <a href="#">clube@vinheriaagnello.com</a></p>
    <p>Siga nosso clube:
      <a href="#">Instagram</a> |
      <a href="#">Facebook</a>
    </p>
    <p>&copy; 2025 Clube Agnello. Todos os direitos reservados.</p>
  </footer>

  <script src="https://cdn.jsdelivr.net/npm/apexcharts"></script>


  <script>
    // Configuração do gráfico umidade
    const options = {
      chart: {
        height: 280,
        type: "radialBar",
      },
      series: [75], 
      plotOptions: {
        radialBar: {
          startAngle: -90,
          endAngle: 90,   
          hollow: {
            margin: 0,
            size: "70%", 
          },
          track: {
            background: "black", 
            strokeWidth: "100%",
          },
          dataLabels: {
            name: {
              show: true,
              color: "black",
              fontSize: "16px",
              offsetY: 20
            },
            value: {
              show: true,
              fontSize: "36px",
              color: "Black",
              offsetY: -30,
              formatter: (val) => `${val}%` 
            }
          }
        }
      },
      fill: {
        colors: '#7C1E52', 
      },
      stroke: {
        lineCap: "round", 
      },
      labels: ["Umidade"],
    };

    // Renderiza o gráfico dentro da div #chart id
    const chart = new ApexCharts(document.querySelector("#chartUmidade"), options);
    chart.render();

    // Grafico Temperatura
    var optionsTemp = {
      chart: {
        type: 'line', height: 350, toolbar: { show: false },
        background: 'transparent'
      },
      series: [{
        name: 'Temperatura (°C)',
        data: [22, 24, 25, 23, 26, 27, 28]
      }],
      xaxis: {
        categories: ['Seg', 'Ter', 'Qua', 'Qui', 'Sex', 'Sáb', 'Dom'],
        labels: { style: { colors: '#333', fontSize: '14px' } }
      },
      yaxis: {
        title: { text: 'Temperatura (°C)' },
        labels: { style: { colors: '#333' } }
      },
      stroke: {
        curve: 'smooth',
        width: 3
      }, colors: ['#7C1E52'],
      // cor da linha 
      markers: {
        size: 5, colors: ['#fff'],
        strokeColors: ['#7C1E52'], strokeWidth: 2
      },
      title: {
        text: 'Temperatura da Vinheria ',
        align: 'center', style: { fontSize: '18px', color: '#111' }
      },
      grid: { borderColor: '#eee' }
    };

    var chartTemp = new ApexCharts(document.querySelector("#graficoLinha"), optionsTemp);
    chartTemp.render();

    // Grafico LDR
    var optionsLDR = {
      chart: {
        type: 'line', height: 350, toolbar: { show: false },
        background: 'transparent'
      },
      series: [{
        name: 'Luminosidade',
        data: [22, 24, 25, 23, 26, 27, 28]
      }],
      xaxis: {
        categories: ['Seg', 'Ter', 'Qua', 'Qui', 'Sex', 'Sáb', 'Dom'],
        labels: { style: { colors: '#333', fontSize: '14px' } }
      },
      yaxis: {
        title: { text: 'Luminoisidade' },
        labels: { style: { colors: '#333' } }
      },
      stroke: {
        curve: 'smooth',
        width: 3
      }, colors: ['#7C1E52'],
      // cor da linha 
      markers: {
        size: 5, colors: ['#fff'],
        strokeColors: ['#7C1E52'], strokeWidth: 2
      },
      title: {
        text: 'Luminosidade na Vinheria',
        align: 'center', style: { fontSize: '18px', color: '#111' }
      },
      grid: { borderColor: '#eee' }
    };

    var chartLDR = new ApexCharts(document.querySelector("#graficoLDR"), optionsLDR);
    chartLDR.render();

    let historicoTemperatura = [];
    let historicoUmidade = [];
    let historicoLDR = [];

    let contador = 0;


    function atualizarDados() {
      fetch('http://localhost:1880/') // URL do Node-RED
        .then(response => response.json())
        .then(data => {
          console.log("Json recebido:", data);

          document.getElementById('temperatura').textContent = data.temperatura.toFixed(1);
          document.getElementById('umidade').textContent = data.umidade.toFixed(1);

          contador++;

          historicoTemperatura.push(data.temperatura);
          historicoUmidade.push(data.umidade);
          historicoLDR.push(data.LDR);

          if (historicoTemperatura.length > 20) historicoTemperatura.shift();
          if (historicoUmidade.length > 20) historicoUmidade.shift();
          if (historicoLDR.length > 20) historicoLDR.shift();

          // ✅ Atualiza gráficos
          chartTemp.updateSeries([{
            name: 'Temperatura (°C)',
            data: historicoTemperatura
          }]);

          chartLDR.updateSeries([{
            name: 'Luminosidade',
            data: historicoLDR
          }]);

          chart.updateSeries([data.umidade]); 

          const eixoX = Array.from({ length: historicoTemperatura.length }, (_, i) => i + 1);
          chartTemp.updateOptions({ xaxis: { categories: eixoX } });
          chartLDR.updateOptions({ xaxis: { categories: eixoX } });
        })
        .catch(e => {
          console.log("Falha ao buscar dados:", e);
        });
    }

    setInterval(atualizarDados, 5000);
    atualizarDados();

  </script>
</body>
</html>
````

## Autores

- Eduardo Santiago Bassan — RM: 561474
- Vitor Fernandes dos Santos — RM: 566275
- Henry Andrade Browne — RM: 562622
- João Victor de Souza Abe — RM: 561446

---

<!-- ## 📂 Repositório GitHub

[https://github.com/JoaoAbe/CP3-Frontend](https://github.com/JoaoAbe/CP3-Frontend)


## ✅ Link do GitHub Pages

🔗 [Acesse o site clicando aqui](https://joaoabe.github.io/CP3-Frontend/)

--- -->
