# Questão 25

## Na questão 25 a questão pede duas regras para o acionamento do led, primeiro se o botão k1 for pressionado nada acontece, se somente K0 for pressionado também nada acontece, se botão K0 e logo após exatamente 1 segundo o botão K1 forem pressionados respectivamente então o led D1 é acesso.

### A escolha dos pinos pe4 e pe3 foi exclusivamente porque eles são responsáveis por acionar os leds D1 e D2 respectivamente presentes no micro, apesar disso só é necessário acender o led D1 na questão 25.

````C 
/**
  ***********************************************************************************************************************
  * @file    Questao-25.c 
  * @author  Gabriel D, Luiz Neto 
  * @version V1.0.0
  * @date    05-October-2023
  * @brief  Led só acende se quando o botão K0 primeiro é precionado e depois de exatamente 1 segundo o K1 é precionado
 ***********************************************************************************************************************
*/

#include "stm32f4xx.h"
#include "Utility.h"

void questao25(void) {
	Utility_Init();
	RCC -> AHB1ENR |= (1 << 0) | (1 << 4);
	GPIOA -> MODER |= (0b01 << 12) ;
	GPIOE -> PUPDR |= (0b01 << 6) | (0b01 << 8);
	while (1) {
		int leitura_k0 = !(GPIOE->IDR & (1 << 4));
		int leitura_k1 = !(GPIOE->IDR & (1 << 3));

		if (leitura_k1) {
			continue;
		}
		if (leitura_k0) {
			int i = 0;
			while (leitura_k0) {
				leitura_k0 = !(GPIOE->IDR & (1 << 4));
				leitura_k1 = !(GPIOE->IDR & (1 << 3));

				if (leitura_k0 && leitura_k1 && i < 1000) {
					GPIOA-> ODR &= ~(1 << 6);
					i = 0;
				}
				else GPIOA -> ODR |= (1 << 6);
			Delay_ms(1);
			i++;
			}
		}
		GPIOA -> ODR |= (1 << 6);
		leitura_k1 = leitura_k0 = 0;
	}
}

int main(void){
    questao25();
}
````
