// Definição dos pinos dos displays de 7 segmentos

// Array que contém os valores hexadecimais para cada dígito de 0 a 9
// Esses valores correspondem à configuração dos segmentos do display de 7 segmentos
int display7c[10] = {0x3f, 0x06, 0x5b, 0x4f, 0x66, 0x6d, 0x7d, 0x07, 0x7f, 0x67};

// Declaração dos pinos que controlam os segmentos do display
byte a = 2;
byte b = 3;
byte c = 4;
byte d = 5;
byte e = 6;
byte f = 7;
byte g = 8;

// Declaração dos pinos que controlam os transistores para cada dígito do display
byte t1 = 9;
byte t2 = 10;
byte t3 = 11;
byte t4 = 12;

// Declaração dos pinos dos botões
byte buttonStartPause = A1; // Pino do botão para iniciar/pausar
byte buttonReset = A2; // Pino do botão para reiniciar

// Variáveis para controle do tempo
long tempor = 0; // Contador que vai de 0 a 999
long contret = 0; // Contador para controlar a atualização do display
bool running = false; // Estado do cronômetro (executando ou pausado)

// Função que aciona os pinos do display de 7 segmentos
// bits: valor que representa os segmentos a serem acesos
// ini: índice inicial dos pinos
// fin: índice final dos pinos
void porta(int bits, int ini, int fin) {
  for (int i = ini; i <= fin; i++) {
    // Aciona o pino correspondente ao bit atual
    digitalWrite(i, bitRead(bits, i - ini));
  }
}

// Função que exibe o valor atual do contador no display
void mostrar() {
  int dig[4]; // Array para armazenar os dígitos do número

  // Calcula cada dígito do número atual (tempor)
  dig[0] = tempor / 1000; // Milhares
  dig[1] = (tempor - dig[0] * 1000) / 100; // Centenas
  dig[2] = (tempor - dig[0] * 1000 - dig[1] * 100) / 10; // Dezenas
  dig[3] = (tempor - dig[0] * 1000 - dig[1] * 100 - dig[2] * 10); // Unidades

  // Mostra cada dígito no display
  for (int i = t1; i <= t4; i++) {
    // Aciona os segmentos do display para o dígito atual
    porta(display7c[dig[i - t1]], a, g);
    
    // Ativa o pino do dígito atual
    digitalWrite(i, HIGH);
    
    // Mantém o dígito aceso por 1 milissegundo
    delay(1);
    
    // Desativa o pino do dígito atual
    digitalWrite(i, LOW);
  }
}

// Função que controla a temporização do display
void temporizacao() {
  contret = 50; // Define quantas vezes o display será atualizado
  while (contret > 0) {
    mostrar(); // Chama a função para mostrar o número no display
    contret--; // Decrementa o contador
  }
}

// Função de configuração inicial
void setup() {
  // Configura todos os pinos como saídas
  for (int i = a; i <= t4; i++) {
    pinMode(i, OUTPUT);
  }
  
  // Configuração dos pinos dos botões
  pinMode(buttonStartPause, INPUT);
  pinMode(buttonReset, INPUT);
}

// Função principal que executa em loop
void loop() {
  // Verifica se o botão de iniciar/pausar foi pressionado
  if (digitalRead(buttonStartPause) == LOW) {
    delay(100); // Debounce para evitar múltiplas leituras
    running = !running; // Alterna o estado do cronômetro
  }

  // Se o cronômetro estiver em execução, incrementa o contador
  if (running) {
    temporizacao(); // Atualiza o display
    tempor++; // Incrementa o contador
    if (tempor > 999) {
      tempor = 999; // Limita o contador a 999
      running = !running; // Alterna o estado do cronômetro
    }
  }

  // Atualiza o display com o valor atual do contador, mesmo se estiver pausado
  mostrar();

  // Verifica se o botão de reiniciar foi pressionado
  if (digitalRead(buttonReset) == LOW && !running) {
    tempor = 0; // Reinicia o contador se o cronômetro estiver pausado
    delay(100); // Debounce para evitar múltiplas leituras
  }
}
