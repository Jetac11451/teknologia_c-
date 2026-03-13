# Лабораторные работы по C++
Дисциплина: Технологии и методы программирования

## Описание проектов

Здесь лежат две лабораторные работы, связанные с побитовой обработкой двоичных строк (8 бит).

### Лабораторная 1 
**Файл:** `1лаба.sln`

Простая версия программы без функций:  
- Вводит две двоичные строки с клавиатуры  
- Проверяет корректность (только 0 и 1, не длиннее 8 символов)  
- Дополняет ведущими нулями до 8 бит  
- Выполняет побитовую конъюнкцию (логическое И)  
- Выводит результат

```C++
#include <iostream>
#include <string>

using namespace std;

int main()
{
    string bs1, bs2;
    cout << "bs 1: ";
    getline(cin, bs1);
    if (bs1.size() > 8 || bs1.find_first_not_of("01") != string::npos) {
        cout << "oshibka\n";
        return 1;
    }
    cout << "bs 2: ";
    getline(cin, bs2);
    if (bs2.size() > 8 || bs2.find_first_not_of("01") != string::npos) {
        cout << "oshibka\n";
        return 1;
    }
    bs1 = string(8 - bs1.size(), '0') + bs1;
    bs2 = string(8 - bs2.size(), '0') + bs2;
    string result;
    for (short i = 0; i < 8; ++i) {
        result += (bs1[i] == '1' && bs2[i] == '1') ? '1' : '0';
    }
    cout << "Result = " << result << endl;
    return 0;
} 
```
### Лабораторная 3
**Файл:** `3лабатех.sln`

Улучшенная, структурированная версия программы:

- Использует функции
- Читает первую строку из файла a.txt
- Вводит вторую строку с клавиатуры
- Проверяет корректность через функцию 
- Выполняет побитовое И (конъюнкцию)
- Выводит результаты на экран и сохраняет в файл result.txt
```
#include <iostream>
#include <fstream>
#include <string>

using namespace std;

void normalize(string& bs) {
    if (bs.size() > 8 || bs.find_first_not_of("01") != string::npos) {
        cout << "Incorrect input\n";
        exit(0);
    }
    bs = string(8 - bs.size(), '0') + bs;
}

void fileinput(string filename, string& bs) {
    ifstream file(filename);
    if (!file.is_open()) {
        cout << "File not found\n";
        exit(0);
    }

    getline(file, bs);
    normalize(bs);
    file.close();
}

void input(string& bs) {
    cout << "Input bs: ";
    getline(cin, bs);
    normalize(bs);
}

void output(string bs) {
    cout << "Stroka = " << bs << endl;
}

void fileoutput(string filename, string bs) {
    ofstream file(filename);
    file << "Stroka = " << bs;
    file.close();
}

string conjaction(string s1, string s2) {
    string res;

    for (int i = 0; i < 8; i++) {
        if (s1[i] == '1' && s2[i] == '1')
            res += '1';
        else
            res += '0';
    }

    return res;
}

int main() {

    string bs1, bs2, res;

    fileinput("a.txt", bs1);   
    input(bs2);                

    cout << "BS1: ";
    output(bs1);

    cout << "BS2: ";
    output(bs2);

    res = conjaction(bs1, bs2);

    cout << "Result: ";
    output(res);

    fileoutput("result.txt", res);

    system("pause");
    return 0;
}
```
