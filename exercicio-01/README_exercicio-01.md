# Exercício 01 - Observação de Comportamento com Semáforo Contador

## Observação de Comportamento

R: Coloquei três tarefas de prioridades iguais, o que ocorre é que a primeira e a segunda que são executadas pegam uma unidade do semáforo cada, e não o liberam. Assim, quando a terceira tarefa a ser executada tenta dar Acquire, não consegue e trava a execução do código, já que consequentemente as outras tarefas também tentam pegá-lo e não conseguem.

## Hardware
- Microcontrolador: STM32F446RE
- Placa: NUCLEO-F446RE
- Periféricos utilizados: GPIO, UART

## Imagens

![Comportamento](images/img-exercicio-01.jpeg)