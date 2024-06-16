
# STM32 - C dilinde UART Transmit dönüşümleri

Burada En kolay ve en pratik uart transmit dönüşümünü anlatmaya çalışacağım


## Kullanım/Örnekler
UART üzerinden bilgiler uint8_t formatında gönderilir.

Aşağıdaki örnekte String olarak '1' '2' '3' yani 49 50 51 gönderilir.
```c
uint8_t buffer[3] = "123";
HAL_UART_Transmit(&huart1, buffer, 10, 200);
```

------------

Aşağıdaki örnekte küsüratlı bir sayı olan virgüllü bir sayı virgülsüz olarak dönüştürülür ve gönderilir. ve snprintf komutu ile buffer değeri aşağıdaki değere dönüşür.
buffer = { "1" , "2", "3" , "4" , "5", "6" ,"7" , "8", "9" ,"1" }
```c
double virgullu = 12.34567891;
char buffer[10];
snprintf(buffer, 10, "%f", virgullu);
// snprintf'deki 2. parametre (10) uzunluğu belirtir.
//burada buffer char veri tipindendir. uint8_t'ye çevirilmesi gerekir
HAL_UART_Transmit(&huart1, (uint8_t*)buffer, 10, 200);
// artık buffer = { "1" , "2", "3" , "4" , "5", "6" ,"7" , "8", "9" ,"1" } olarak veriler gönderilmiştir
```

------------

Aşğaıdaki örnekte eldeki bilgiyi göndermiş olmaktayız.
```c
char data[80] = 0x0a // dizideki her boşluğu \n ile doldurmak için 0x0a koyduk;
char buffer[2];
buffer[0] = '1';
buffer[1] = '2'
sprintf(data, "Giden %c%c", buffer[0],buffer[1]);
//sprintf'de snprintf'deki olduğu gibi uzunluk belirtmemize gerek yoktur.
HAL_UART_Transmit(&huart1, (uint8_t*)data, 80, 200);
// Giden 12
```

------------

Peki ya elimizde sayısal bir veri var ise o zaman nasıl yapacağız.
```c
char data[80] = 0x0a // dizideki her boşluğu \n ile doldurmak için 0x0a koyduk;
char buffer[2];
uint8_t sayi = 12;
sprintf(data, "Giden %c", sayi);
//uint8_t'deki sayısal değer chara dönüştürülür
HAL_UART_Transmit(&huart1, (uint8_t*)data, 80, 200);
// Giden 12
```