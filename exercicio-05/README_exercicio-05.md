# Exercício 05 - Coleta, Processamento e Reinicialização com Interrupção Externa

## Observação de Comportamento

R: Tive algumas complicaçõezinhas na hora de sincronizar tudo, mas no geral foi simples. Fiz conforme o roteiro e hierarquicamente as tarefas liberam os semáforos para a que está abaixo dela. TaskADC libera para a TaskCalc que por sua vez libera para a TaskSend. Nesse processo, TaskSend aumenta um contador de ciclo, que ao chegar ao décimo, trava a execução da TaskADC pela variável processo_ativo. Nisso, o programa aguarda pela interrupção do B1 (user btn) que alterna o estado de processo_ativo, e ao apertar o botão o processo se repete como esperado. (obs.: estou fazendo leituras de um potenciômetro).

## Hardware
- Microcontrolador: STM32F446RE
- Placa: NUCLEO-F446RE
- Periféricos utilizados: GPIO, UART, ADC, EXTI

## Imagens

![Comportamento](images/img-exercicio-05.jpeg)