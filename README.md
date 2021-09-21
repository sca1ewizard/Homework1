# Homework1
Домашнее задание №1 по предмету "Функциональная логика и теория алгоритмов".
Сорокин Сергей ИУ4-31Б Вариант 2.
---------------------------------------------------------------------------
2. Пользователь вводит число в двоичной системе счисления, необходимо распечатать его в десятичной системе счисления.
Бонус: предусмотреть использование отрицательных чисел и возможность переполнения.
Пример ввода: 101
Пример вывода: 5
---------------------------------------------------------------------------

#include <stdint.h>

#include <stdio.h>

#include <limits.h>

#include <stdlib.h> //подключаем все нужные нам библиотеки

//#pragma warning(disable: 4996) //заглушаем предупреждения компилятора (в начале программа использовала scanf и компилятор сильно ругался))

#define TRUE 1   //используется для определения наличия у числа знака + или - 
#define FALSE 0  //используется для определения наличия у числа знака + или -
#define ERROR - 1   //объявляем все макроподстановки

int main() {
    char buffer; //непосредственно введённый символ
    unsigned int value = 0; //окончательное значение числа
    int count = 0; //подсчет символов окончательного числа (используется для определения переполнения) 
    int sign = FALSE;
    while (((buffer = getchar()) != EOF) && buffer != '\n' && buffer != '\0') {  //если введенный сивол не конец файла и не сивол перехода на новую строку и не сивол конца строки) 
        if ((buffer == '-') && (sign == FALSE)) { //определяем знак
            sign = TRUE;
        }
        else {

            if ((buffer == '1') || (buffer == '0')) {  //в двоичной записи числа содержаться только числа 1 и 0, так что для них...
                value <<= 1;                           //сдвигаем побитово value на 1 бит влево
                value += buffer - '0';                 //и прибавляем к нему значение буфера
                count++;                               //инкриминируем счётчик символов
            }
            else {                                     //иначе, если символ не 1 или 0 или -, значит символ введён неправильно и надо выдать ошибку 
                printf("Error.\n");                    //печатаем ошибку
                exit(ERROR);                           //завершаем программу с кодом ошибки -1
            }
        }
    }                                                  
    if (count > sizeof(int) * CHAR_BIT) {              //Проверка не переполнение
        printf("Error oversize.\n");                   //если кол символов больше чем максимально количество бит для данной архитектуры памяти, то выводим ошибку
        exit(ERROR);
    }
    if (sign == TRUE) {                      //если есть знак "-", то печатаем его
        printf("-");
    }
    printf("%u\n", value);                //выводим результат
    return 0;
}
//00000000000000000000000000000000       //Просто проверял по этим числам перепонение, чтобы каждый раз не вбивать 32 единицы
//11111111111111111111111111111111

