# Configuração dos Projetos

## Projetos do STM32CUBEmx

![imagens/chunk-0-img-0.jpeg](imagens/chunk-0-img-0.jpeg)

## Configuração NUCLEO (L476RG)

O Projeto do professor solicita:

**DIAGRAMA DE BLOCOS SIMPLIFICADO: SISTEMA CLIENTE-SERVIDOR E INTERFACE PC**
Objetivo: Comunicação, Controle de LED e Transmissão de Tabela via DMA

![imagens/chunk-0-img-1.jpeg](imagens/chunk-0-img-1.jpeg)
(Imagem gerada por IA)

por isso, adotamos as configurações:

![imagens/chunk-0-img-2.jpeg](imagens/chunk-0-img-2.jpeg)

realizamos a substituição do pin PC13

![imagens/chunk-0-img-3.jpeg](imagens/chunk-0-img-3.jpeg)

por fim, marquei o NVIC

![imagens/chunk-0-img-4.jpeg](imagens/chunk-0-img-4.jpeg)

# Para USART2, que será ligada com a máquina:

![imagens/chunk-0-img-5.jpeg](imagens/chunk-0-img-5.jpeg)

![imagens/chunk-0-img-6.jpeg](imagens/chunk-0-img-6.jpeg)

![imagens/chunk-0-img-7.jpeg](imagens/chunk-0-img-7.jpeg)

![imagens/chunk-0-img-8.jpeg](imagens/chunk-0-img-8.jpeg)

Para USART1, que será ligado na Discovery:

![imagens/chunk-0-img-9.jpeg](imagens/chunk-0-img-9.jpeg)

![imagens/chunk-0-img-10.jpeg](imagens/chunk-0-img-10.jpeg)

![imagens/chunk-0-img-11.jpeg](imagens/chunk-0-img-11.jpeg)

![imagens/chunk-0-img-12.jpeg](imagens/chunk-0-img-12.jpeg)

STM32

File

Window

Help

#

#

#

#

#

#

#

#

#

#

#

#

#

#

#

#

#

#

#

#

#

#

Home

STM32L476RGTx - NUCLEO-L476R3

otiente-nucleo.io - Project Manager

GENERATE CODE

#

#

#

#

#

#

#

#

#

#

#

#

#

#

#

#

#

P

P

P

P

P

P

P

P

P

P

P

P

P

P

P

P

P

P

P

P

P

P

P

P

P

P

P

P

P

P

P

P

P

P

P

P

P

P

P

P

Coloquei o botão para ser o EXTI

![imagens/chunk-0-img-13.jpeg](imagens/chunk-0-img-13.jpeg)

![imagens/chunk-0-img-14.jpeg](imagens/chunk-0-img-14.jpeg)

conferimos como o GPIO está:

![imagens/chunk-0-img-15.jpeg](imagens/chunk-0-img-15.jpeg)

e marcamos a opção do EXTi

![imagens/chunk-0-img-16.jpeg](imagens/chunk-0-img-16.jpeg)

# COMUNICAÇÃO:

![imagens/chunk-0-img-17.jpeg](imagens/chunk-0-img-17.jpeg)

![imagens/chunk-0-img-18.jpeg](imagens/chunk-0-img-18.jpeg)

![imagens/chunk-0-img-19.jpeg](imagens/chunk-0-img-19.jpeg)

![imagens/chunk-0-img-20.jpeg](imagens/chunk-0-img-20.jpeg)

# Ligações físicas

Dados:
Núcleo GPIO D2(PA10) [USART1_RX] — jumper roxo---&gt; [USART3_TX] PB10 Discovery.
Núcleo GPIO D8(PA9) [USART1_TX] — jumper cinza---&gt; [USART3_RX] PB11 Discovery.

## Alimentação:
Para gravar: ambas ligadas no USB

Para rodar o Projeto:
- 3v3 da núcleo — jumper marrom —&gt; 3v da Discovery
- GND da Nucleo — jumper preto —&gt; GND da discovery

(seguimos nesse modelo como solicitação o prof. em sala.).

# Codigo

# Codigo Nucleo

![imagens/chunk-0-img-21.jpeg](imagens/chunk-0-img-21.jpeg)

# EXTi do botão

![imagens/chunk-0-img-22.jpeg](imagens/chunk-0-img-22.jpeg)

# reciver do contador na nucleo e Callback do DMA

![imagens/chunk-0-img-23.jpeg](imagens/chunk-0-img-23.jpeg)

blink do led, feito por toggle (daí as piscadas pela metade. é um acendendo e outro apagando)

# Codigo Discovery

```c
435
436 USER CODE BEGIN 4 */
437 Void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin)
438 {
439 if(GPIO_Pin == GPIO_PIN_0)
440 {
441   contador++;
442     // só toggle, sem delay
443     HAL_GPIO_TogglePin(GPIOD, GPIO_PIN_12);
444 }
445 }
446 Void HAL_UART_RxCpltCallback(UART_HandleTypeDef *huart)
447 {
448   if(huart-&gt;Instance == USART3)
449   {
450     if(rx == 0x5A)
451   {
452     }
```

codigo e toggle do led.

```c
453 {
454 }
455 Void HAL_UART_RxCpltCallback(UART_HandleTypeDef *huart)
456 {
457   if(huart-&gt;Instance == USART3)
458   {
459     if(rx == 0x5A)
460   {
461     //HAL_UART_Transmit_IT(&amp;huart3, &amp;contador, 1);
462         //HAL_UART_Transmit_DHA(&amp;huart3, (uint8_t*)tabela, sizeof(tabela));
463         HAL_UART_Transmit(&amp;huart3, &amp;contador, 1, 100);
464         HAL_Delay(10); // pequeno delay
465         //HAL_UART_Transmit(&amp;huart3, (uint8_t*)tabela, sizeof(tabela), 100);
466         uint16_t tamanho = strlen(tabela);
467         HAL_UART_Transmit(&amp;huart3, (uint8_t*)&amp;tamanho, 2, 100);
468         HAL_UART_Transmit(&amp;huart3, (uint8_t*)tabela, tamanho, 100);
469 }
```

recebe a senha e faz o envio do contador e tabela.

# Teste

drivers: https://www.st.com/en/development-tools/stsw-link009.html

Advanced serial port terminal: https://www.com-port-monitoring.com/serial-port-terminal/

1. Localize a porta COM

![imagens/chunk-0-img-24.jpeg](imagens/chunk-0-img-24.jpeg)

![imagens/chunk-0-img-25.jpeg](imagens/chunk-0-img-25.jpeg)
2. Preencha a new session

![imagens/chunk-0-img-26.jpeg](imagens/chunk-0-img-26.jpeg)
3. bauld rate // speed

4. código de inicialização

![imagens/chunk-0-img-27.jpeg](imagens/chunk-0-img-27.jpeg)

5. qnt de botões apertadas recebido

```txt
[INIT] Sistema iniciado...
[BTN] Botão pressionado - enviando 0x5A...
[UART1 TX] Comando enviado.
[UART1 RX] Contador recebido: 6
[DMA] Recebendo tabela...
[STATE] -&gt; PROCESSING
[LED] Piscar concluído.
Número de eventos = 6



