# MQTT
# Ecosistema rede IOT

Este projeto visa desenvolver um ecossistema de rede IoT, integrando dispositivos inteligentes para coletar, processar e transmitir dados de forma eficiente. Utilizando microcontroladores como ESP32 e Arduino, além de protocolos de comunicação sem fio e armazenamento em nuvem, o sistema permitirá automação e monitoramento em diversas aplicações, como cidades inteligentes e automação residencial. A solução será escalável e segura, garantindo interação eficiente entre os dispositivos conectados.

---

# Comandos

### **Instalando o Mosquitto**

Os passos exatos para instalar o Mosquitto dependem do sistema operacional, mas para a maioria das distribuições Linux, é possível utilizar um gerenciador de pacotes. No Ubuntu, por exemplo, a instalação pode ser feita com o seguinte comando:

```bash
sudo apt-get install mosquitto mosquitto-clients
```

Para Windows ou macOS, o instalador pode ser baixado no site oficial do Mosquitto, seguindo as instruções fornecidas.

### **Iniciando o Mosquitto Broker**

Após a instalação, o próximo passo é iniciar o broker MQTT. No Linux, ele pode ser iniciado com o comando:

```bash
sudo systemctl start mosquitto
```

No Windows, o broker normalmente inicia automaticamente após a instalação. Caso contrário, pode ser iniciado manualmente pelo aplicativo **Serviços**.

### **Publicando uma mensagem**

Para publicar uma mensagem, utiliza-se a ferramenta de linha de comando `mosquitto_pub`, que vem com o pacote Mosquitto. A sintaxe básica é:

```bash
mosquitto_pub -h <host> -t <tópico> -m <mensagem>
```

Explicação das opções do comando:

- `h` especifica o endereço do broker MQTT.
- `t` define o tópico no qual a mensagem será publicada.
- `m` indica a mensagem que será enviada.

### **Assinando um tópico**

Para assinar um tópico e receber mensagens, utiliza-se a ferramenta `mosquitto_sub`, com a sintaxe:

```bash
mosquitto_sub -h <host> -t <tópico>
```

Explicação das opções:

- `h` indica o endereço do broker MQTT.
- `t` especifica o tópico ao qual deseja se inscrever.

### **Testando a configuração**

Após publicar uma mensagem e assinar um tópico, o próximo passo é testar a comunicação. Para isso, basta enviar uma mensagem de teste e verificar se o assinante a recebe. Se tudo estiver funcionando corretamente, a mensagem aparecerá no console do assinante.

# Configuração na raspyberry

### **Passo 1: Baixar e Instalar o Raspberry Pi OS**

1. **Baixe o Raspberry Pi Imager** no site oficial:
    
    🔗 https://www.raspberrypi.com/software/
    
2. **Insira um cartão microSD** (mínimo de 8GB, recomendado 16GB ou mais) no computador.
3. **Abra o Raspberry Pi Imager** e:
    - Escolha o **Raspberry Pi OS** (ou outro sistema desejado).
    - Selecione o cartão microSD.
    - Clique em **"Write"** e aguarde a gravação.

---

## **2. Configuração Inicial**

1. **Insira o cartão microSD na Raspberry Pi** e ligue-a (com monitor, teclado e mouse conectados).
2. **Siga o assistente de configuração**:
    - Escolha o idioma e fuso horário.
    - Configure o Wi-Fi (se aplicável).
    - Defina um nome de usuário e senha.
    - Atualize o sistema, se solicitado.

---

## **3. Configurar Acesso Remoto (SSH e VNC)**

Se quiser controlar sua Raspberry Pi remotamente, configure SSH e/ou VNC.

### **Ativar SSH (Acesso via Terminal)**

1. Abra um terminal e digite:
    
    ```bash
    sudo raspi-config
    ```
    
2. Vá para **Interface Options** > **SSH** > **Enable**.

Agora, você pode acessar via SSH usando:

```bash
ssh pi@IP_DA_RASPBERRY
```

(senha padrão: `raspberry`, se não foi alterada)

### **Ativar VNC (Acesso Gráfico)**

1. No **raspi-config**, vá em **Interface Options** > **VNC** > **Enable**.
2. Instale o **VNC Viewer** no seu computador e conecte-se à Raspberry Pi.

---

## **4. Conectar à Internet (Wi-Fi ou Ethernet)**

Se ainda não conectou no assistente inicial:

1. Para Wi-Fi, edite o arquivo de configuração:
    
    ```bash
    sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
    ```
    
2. Adicione:
    
    ```
    plaintext
    CopiarEditar
    network={
        ssid="NOME_DA_REDE"
        psk="SENHA_DA_REDE"
    }
    
    ```
    
3. Salve (`Ctrl + X`, `Y`, `Enter`) e reinicie.

---

## **5. Instalar Pacotes Essenciais**

Após conectar à internet, execute:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install git python3 pip 
```

Outros pacotes úteis:

- **Node.js**: `sudo apt install nodejs -y`
- **Flask (para APIs)**: `pip install flask`
- **VS Code**: `sudo apt install code -y`

---

## **6. Configurar Boot e Autostart**

Se precisar rodar um script automaticamente na inicialização:

1. Edite o crontab:
    
    ```bash
    crontab -e
    ```
    
2. Adicione a linha para executar um script Python na inicialização:
    
    ```
    plaintext
    CopiarEditar
    @reboot python3 /home/pi/seu_script.py &
    
    ```
    

# Segundo passo- instalação da raspyberry pi

1. **Raspberry Pi ligada e conectada à internet**
    
    Você precisa que sua Raspberry esteja funcionando normalmente, com acesso à internet via Wi-Fi ou cabo Ethernet. A conexão com a internet é necessária para baixar os pacotes do repositório oficial.
    
2. **Cartão SD com Raspberry Pi OS funcionando**
    
    Certifique-se de que o sistema operacional (como o Raspberry Pi OS - o antigo Raspbian) já está instalado no cartão SD e a Raspberry Pi está inicializando corretamente com ele.
    
3. **Terminal aberto**
    
    Pode ser:
    
    - Diretamente na Raspberry Pi (com teclado, mouse e monitor conectados),
    - Ou remotamente via **SSH**, se você estiver acessando pelo seu computador.

---

## 🧾 **Passo 1: Atualizar os pacotes**

### Comandos:

```bash
bash
CopiarEditar
sudo apt update
sudo apt upgrade

```

Vamos entender o que cada um faz:

---

### 🧩 `sudo apt update`

- Esse comando **atualiza a lista de pacotes disponíveis** e suas versões.
- Ele consulta os repositórios configurados na sua Raspberry e verifica se há **novas versões** dos pacotes que você pode instalar.
- **Importante**: Esse comando **não instala nada ainda**, ele apenas prepara o sistema para saber quais são as versões mais recentes disponíveis.

### Exemplo de saída:

```
go
CopiarEditar
Hit:1 http://raspbian.raspberrypi.org/raspbian bullseye InRelease
Reading package lists... Done

```

---

### 🧩 `sudo apt upgrade`

- Este comando **instala as atualizações** dos pacotes que já estão instalados no seu sistema, **com base na lista atualizada** pelo comando anterior.
- Ele pode pedir sua confirmação (`y/n`) para começar a atualização.

### Exemplo de saída:

```
vbnet
CopiarEditar
The following packages will be upgraded:
  libcurl4 curl raspberrypi-bootloader
Do you want to continue? [Y/n]

```

- Você digita `Y` e pressiona Enter para continuar.

---

### 🔐 Por que usar `sudo`?

- `sudo` dá **permissão de superusuário (root)** para o comando.
- É necessário para modificar ou instalar pacotes no sistema

- CODIGO QUE FOI USADO NA COMUNICAÇÃO ENTRE O GRUPO 1 E GRUPO 3
- #include <WiFi.h>
#include <PubSubClient.h>

#define LED 18
#define BOTAO 4

const char* ssid = "iot";
const char* password = "iotsenai502";
const char* mqtt_server = "192.168.0.173";
const char* mqtt_topic = "chat/grupo1e3";

WiFiClient espClient;
PubSubClient client(espClient);

bool botaoAntigo = HIGH;
bool ledLigado = false;

void setup_wifi() {
  Serial.begin(115200);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("\nWi-Fi conectado!");
}

void callback(char* topic, byte* message, unsigned int length) {
  String msg;
  for (int i = 0; i < length; i++) {
    msg += (char)message[i];
  }
  Serial.print("Recebido: ");
  Serial.println(msg);

  if (msg == "1") {
    digitalWrite(LED, HIGH);
    Serial.println("LED ligado (recebido 1)");
  } else if (msg == "0") {
    digitalWrite(LED, LOW);
    Serial.println("LED desligado (recebido 0)");
  }
}

void reconnect() {
  while (!client.connected()) {
    if (client.connect("ESP32_Grupo2")) {
      client.subscribe(mqtt_topic);
    } else {
      delay(2000);
    }
  }
}

void setup() {
  pinMode(LED, OUTPUT);
  pinMode(BOTAO, INPUT_PULLUP);
  digitalWrite(LED, LOW);
  
  setup_wifi();
  client.setServer(mqtt_server, 1883);
  client.setCallback(callback);
}

void loop() {
  if (!client.connected()) reconnect();
  client.loop();

  bool estadoBotao = digitalRead(BOTAO);
  if (botaoAntigo == HIGH && estadoBotao == LOW) {
    ledLigado = !ledLigado;
    if (ledLigado) {
      client.publish(mqtt_topic, "a");
      Serial.println("Enviado: a");
    } else {
      client.publish(mqtt_topic, "b");
      Serial.println("Enviado: b");
    }
    delay(300);
  }
  botaoAntigo = estadoBotao;
} 
