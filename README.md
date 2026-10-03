# 🚦 Semáforo Inteligente com Contagem Regressiva — Raspberry Pi Pico

Projeto prático de sistema embarcado utilizando a placa **Raspberry Pi Pico** para simular um controle de trânsito inteligente com botão de pedestre e display de 7 segmentos para contagem regressiva.

---

## 📌 Sobre o Projeto

Este projeto consiste na implementação de um sistema de semáforo acionado por demanda. O circuito permanece por padrão no sinal verde. Quando o pedestre solicita a travessia pressionando um botão, o sistema executa a transição de segurança para o amarelo e, em seguida, fecha o trânsito (vermelho), iniciando uma contagem regressiva no display de 7 segmentos.

O circuito foi projetado e testado no simulador online **Wokwi** utilizando a linguagem **MicroPython**.

---

## 🛠️ Componentes e Mapeamento de Pinos

| Componente | Quantidade | Pino na Raspberry Pi Pico (GPIO) | Função / Descrição |
| :--- | :---: | :---: | :--- |
| **Raspberry Pi Pico** | 1 | — | Microcontrolador principal |
| **LED Verde** | 1 | GP2 | Sinalização de fluxo livre |
| **LED Amarelo** | 1 | GP3 | Sinalização de atenção |
| **LED Vermelho** | 1 | GP4 | Sinalização de trânsito fechado |
| **Push Button** | 1 | GP5 | Botão de solicitação (com *Pull-Up* interno) |
| **Display de 7 Segmentos** | 1 | GP28, GP27, GP17, GP19, GP21, GP26, GP22 | Exibição do tempo de travessia (A ao G) |
| **Resistores** | — | — | Limitadores de corrente para LEDs e segmentos |

### Mapeamento dos Segmentos do Display:
- **Segmento A:** GP28
- **Segmento B:** GP27
- **Segmento C:** GP17
- **Segmento D:** GP19
- **Segmento E:** GP21
- **Segmento F:** GP26
- **Segmento G:** GP22

---

## 🔄 Lógica de Funcionamento

1. **Estado Inicial (Repouso):** O LED verde permanece ligado e o display de 7 segmentos mantêm-se apagado.
2. **Solicitação do Pedestre:** O sistema monitora a entrada digital do botão. Como o pino está configurado com `PULL_UP`, a leitura padrão é `1` (HIGH) e muda para `0` (LOW) ao ser pressionado.
3. **Fase Amarela (Atenção):** Assim que o botão é pressionado, o LED verde desliga e o LED amarelo acende por **3 segundos**.
4. **Fase Vermelha (Contagem Regressiva):** O LED amarelo desliga e o LED vermelho acende. Simultaneamente, o display exibe uma contagem de **9 até 1**, atualizada a cada **1 segundo**.
5. **Retorno:** Após o término da contagem, o display apaga, o LED vermelho desliga, o LED verde reacende e o sistema volta ao estado de espera.

---

## 💻 Explicação do Código (`main.py`)

O código foi escrito em **MicroPython** e utiliza a biblioteca nativa `machine` para manipulação dos pinos de entrada e saída.

* **Inicialização de GPIOs:**
  Configuração dos pinos dos LEDs (`Pin.OUT`) e do botão (`Pin.IN, Pin.PULL_UP`).
* **Mapeamento de Dígitos (`numeros`):**
  Dicionário contendo listas binárias (`0` ou `1`) que representam a combinação de segmentos (A-G) necessária para acender os números de 0 a 9.
* **Funções Auxiliares:**
  * `mostrar_numero(numero)`: Lê a combinação do dicionário e aplica os valores binários nos pinos dos segmentos.
  * `apagar_display()`: Desliga todos os segmentos do display.
  * `semaforo_verde()`, `semaforo_amarelo()`, `semaforo_vermelho()`: Funções responsáveis pelo controle de estado dos LEDs.
* **Laço Principal (`while True`):**
  Executa a verificação contínua do botão (`BOTAO.value() == 0`) e gerencia os atrasos de tempo com `time.sleep()`.

---

## 🚀 Como Executar o Projeto

1. Acesse o [Wokwi](https://wokwi.com/) ou utilize a extensão do Wokwi no **VS Code**.
2. Crie um novo projeto para **Raspberry Pi Pico (MicroPython)**.
3. Monte o circuito conectando os LEDs, botão e display conforme a tabela de pinos.
4. Cole o código fornecido no arquivo `main.py`.
5. Inicie a simulação e clique no botão para testar a sequência do semáforo.

---

## 📜 Licença

Este projeto foi desenvolvido para fins educacionais e de aprendizado sobre sistemas embarcados.
