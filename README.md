# Лабораторная работа №6 – Моделирование Kill-chain процедур
**Дисциплина:** Защита ПО в защищённых АС  
**Выполнили:** Кашников Андрей, Политыко Артём, Тоцкая Марина  
**Группа:** 21-КАС-2

**Вариант:** 6.2

## Моделирование Kill-chain процедуры на примере эксплуатации уязвимости CVE-2025-55812 (React2Shell) и фреймворка постэксплуатации Empire
На виртуальную машину были установлены NodeJS версии 20.9.0 и npm версии 10.8.2, на которых экпслуатация React Server Components возможна:

![картинко](https://github.com/Eadarinel/zpozas-lr6/blob/main/lr6_img/1.png)

Был создан и запущен новый проект на NextJS 16.0.6:
![картинко](https://github.com/Eadarinel/zpozas-lr6/blob/main/lr6_img/2.png)
![картинко](https://github.com/Eadarinel/zpozas-lr6/blob/main/lr6_img/3.png)

Веб-интерфейс стартовой страницы развёрнутого веб-сервера:
![картинко](https://github.com/Eadarinel/zpozas-lr6/blob/main/lr6_img/4.png)

Перейдём в веб-интерфейс Starkiller фреймворка постэксплуатации Empire. Воспользуемся Listener’ом и Stager’ом, созданными ранее в лабораторной работе №4, т.е. соединение с CnC-сервером будет установлено посредством тихого запуска python-скрипта, инициализирующего соединение по HTTP по порту 80.

Из готового стейджера необходимо вытащить всю команду и внедрить её в вредоносную нагрузку на React:
![картинко](https://github.com/Eadarinel/zpozas-lr6/blob/main/lr6_img/5.png)

Согласно уязвимости CVE-2025-55812 необходимо послать на сервер следующую нагрузку в HTTP POST запросе, но заменить команду на сгенерированную стейджером:
![картинко](https://github.com/Eadarinel/zpozas-lr6/blob/main/lr6_img/6.png)

Нагрузка была доставлена на веб-сервер с помощью утилиты Postman:
![картинко](https://github.com/Eadarinel/zpozas-lr6/blob/main/lr6_img/7.png)

В доставленной нагрузке происходит выполнение echo на вывод в stdout base64-строки, в которой закодирован скрипт, устанавливающий соединение с Empire-сервером, затем он декодируется посредством встроенной утилиты Ubuntu (base64 –d), запускается Python-shell, считывающий и запускающий скрипт из вывода stdout.
![картинко](https://github.com/Eadarinel/zpozas-lr6/blob/main/lr6_img/8.png)

В веб-интерфейсе Starkiller можно увидеть, что агент вышел на связь:
![картинко](https://github.com/Eadarinel/zpozas-lr6/blob/main/lr6_img/9.png)

Теперь удалённым хостом можно управлять через CnC-сервер:
![картинко](https://github.com/Eadarinel/zpozas-lr6/blob/main/lr6_img/10.png)
