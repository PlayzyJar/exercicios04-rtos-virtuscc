# Exercícios - Aula 4: Semáforos com FreeRTOS (CMSIS-RTOS v2)

**Versão:** 1.0 - 06 de maio de 2025  
**Aluno:** Carlos Elias Fialho de Lima

## 📋 Descrição

Este repositório contém a implementação de 6 exercícios práticos sobre semáforos e mutex utilizando FreeRTOS (CMSIS-RTOS v2) no microcontrolador STM32F446RE.

## 🗂️ Estrutura do Projeto

```
STM32-RTOS-Exercises/
├── README.md                    # Este arquivo
├── .gitignore                   # Arquivos ignorados pelo Git
├── exercicio-01/                # Semáforo Contador
│   ├── README.md
│   └── [arquivos do projeto]
├── exercicio-02/                # Liberação via Interrupção
│   ├── README.md
│   └── [arquivos do projeto]
├── exercicio-03/                # Teste de Disponibilidade
│   ├── README.md
│   └── [arquivos do projeto]
├── exercicio-04/                # Tentativa Indevida de Mutex
│   ├── README.md
│   └── [arquivos do projeto]
├── exercicio-05/                # Coleta e Processamento
│   ├── README.md
│   └── [arquivos do projeto]
└── exercicio-06/                # Sincronização Completa
    ├── README.md
    └── [arquivos do projeto]
```

## 🎯 Exercícios Implementados

### Exercício 1 – Observação de Comportamento com Semáforo Contador
**Objetivo:** Avaliar o funcionamento de tarefas que compartilham um semáforo contador.

**Atividades:**
• Criar três tarefas que utilizam osSemaphoreAcquire()  
• Observar quem executa com semáforo disponível (2 recursos)  
• Usar osSemaphoreGetCount() para depuração  
• Explicar o que ocorre quando uma tarefa tenta adquirir semáforo zerado

### Exercício 2 – Liberação de Semáforo via Interrupção
**Objetivo:** Sincronizar tarefas com liberação do semáforo por interrupção externa (botão).

**Atividades:**
• Criar semáforo com valor inicial = 0  
• Cada tarefa aguarda acquire()  
• Liberar com release() no HAL_GPIO_EXTI_Callback()  
• Observar execução sequencial conforme botão é pressionado

### Exercício 3 – Teste de Disponibilidade e Liberação Manual de Semáforo
**Objetivo:** Demonstrar que o semáforo contador não garante exclusividade.

**Atividades:**
• Testar osSemaphoreGetCount()  
• Se for 0, fazer release() e em seguida acquire()  
• Mostrar que a tarefa pode acessar o recurso sem ser "dona" dele  
• Comparar com o comportamento de um mutex

### Exercício 4 – Tentativa Indevida de Liberação de Mutex
**Objetivo:** Mostrar que apenas a tarefa que adquire um mutex pode liberá-lo.

**Atividades:**
• Criar mutex  
• Task1 faz acquire() e não libera  
• Task2 tenta release() e envia mensagem com o status  
• Verificar erro com osErrorResource e comentar o motivo

### Exercício 5 – Coleta, Processamento e Reinicialização com Interrupção Externa
**Objetivo:** Usar dois semáforos para coordenar ADC, cálculo de média e envio UART, com reinício por botão.

**Atividades:**
• TaskADC coleta 100 amostras e libera semCalcHandle  
• TaskCalc calcula média e libera semSendHandle  
• TaskSend envia média via UART. Repete 10 vezes  
• Após 10 envios, exibe: "Processo concluído. Aguardando botão..."  
• Botão reinicia processo via interrupção externa (EXTI13)

### Exercício 6 – Sincronização Completa por Evento Externo com Armazenamento e Transmissão de Dados
**Objetivo:** Implementar uma cadeia de tarefas sincronizadas por um botão de usuário que, ao ser pressionado, inicia um ciclo completo: aquisição ADC → cálculo de média → armazenamento em memória → envio UART.

## 🔄 Fluxo de Execução do Exercício 6

1. **[Botão pressionado]**
   - No HAL_GPIO_EXTI_Callback(), libere o semáforo ou sinal que inicia a cadeia de tarefas (ex: semStartHandle)

2. **TaskADC (Aquisição):**
   - Espera semStartHandle
   - Realiza 100 leituras ADC (simuladas ou reais)
   - Armazena em um vetor uint16_t bufferADC[100]
   - Ao fim, libera semCalcHandle

3. **TaskCalc (Cálculo):**
   - Aguarda semCalcHandle
   - Calcula a média dos 100 valores
   - Salva em uma variável mediaADC
   - Libera semSaveHandle

4. **TaskSave (Memória):**
   - Aguarda semSaveHandle
   - Salva mediaADC em um bloco de memória estático (ex: vetor float historico[10])
   - Libera semSendHandle

5. **TaskSend (Transmissão):**
   - Aguarda semSendHandle
   - Envia a média pela UART3
   - Exibe no terminal: "Média enviada: [valor]"

## ⚙️ Requisitos Técnicos

### Hardware Comum:
- **Microcontrolador:** STM32F446RE
- **Placa:** NUCLEO-F446RE
- **Periféricos:** GPIO, UART/UART3, ADC, EXTI

### Software:
- **RTOS:** FreeRTOS (CMSIS-RTOS v2)
- **IDE:** STM32CubeIDE
- **Semáforos binários:** semStartHandle, semCalcHandle, semSaveHandle, semSendHandle
- **Vetor para histórico:** float historico[10] com controle de índice circular
- **Tarefas:** Mesma prioridade (ou definidas conforme necessário)

## 🎯 Critérios de Avaliação

- ✅ Tarefas não devem iniciar sem o evento externo (botão)
- ✅ Sincronização correta entre as tarefas
- ✅ Armazenamento funcional em vetor de histórico
- ✅ Transmissão confiável pela UART3
- ✅ Fluxo de execução observável no terminal e/ou debug

## 🚀 Como Usar

1. **Clone o repositório:**
   ```bash
   git clone https://github.com/PlayzyJar/exercicios04-rtos-virtuscc.git
   ```

2. **Importe no STM32CubeIDE:**
   ```
   File → Import → Existing Projects into Workspace
   ```

3. **Para cada exercício:**
   - Navegue até a pasta do exercício
   - Leia o README.md específico
   - Compile e grave na placa NUCLEO-F446RE
   - Observe o comportamento no monitor serial (ex.: Termite)

## 📝 Observações

- Os códigos produzidos estão inseridos no GitHub do projeto
- Cada exercício possui seu próprio README.md com explicações detalhadas
- Use monitor serial (115200 bps/8 bit/none) para visualizar os resultados

---
**Disciplina:** Sistemas Embarcados / RTOS  
**Instituição:** EMBRAPII/VIRTUS-CC  
**Período:** 2025.1