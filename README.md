# Containerization-Seminar_1  
Задание: необходимо продемонстрировать изоляцию одного и того же приложения (как решено на семинаре - командного интерпретатора) в одном из различных пространствах имен.

Запустим Bash в новом пространстве

    sudo unshare -pf -n --mount-proc bash
  
 
 ![1 1](https://github.com/vladislavkrutov8/Containerization-Seminar_1/assets/110223646/4528fb06-a40e-4605-9b7c-e94fc81db480)


В параллельном терминале смотрим вводим команду ps -afx и сравниваем изменения 

      ps -afx
    
![3 3](https://github.com/vladislavkrutov8/Containerization-Seminar_1/assets/110223646/f7fdd8dc-e0a8-4ce6-af06-c2e96629b2f7)

Для проверки и наглядности смотрим той же командой ps -afx в изолированном терминале

![1 1 — копия](https://github.com/vladislavkrutov8/Containerization-Seminar_1/assets/110223646/0fda073c-e685-4bbe-b7c8-6f53bc26831f)


Здесь мы сожем видеть, что изолированная оболчка видит всего два процесса (и то, второй процесс сразу же, после выполнения команды, исчезнет). При этом PID процессов начинается с 1.

Далее, из этого же терминала, пробуем запустить пинг любого сайта, например ya.ru.
ping ya.ru


![1 1 — копия (2)](https://github.com/vladislavkrutov8/Containerization-Seminar_1/assets/110223646/00b19d82-af6a-417d-9b96-4fe807d46c8a)


Пинг до указанного сайта не может быть осуществлен, так как сеть в этом пронстранстве имен имеется только локальная, т.е. localhost.

А теперь, ту же команду запустим из параллельного терминала.



![3 3](https://github.com/vladislavkrutov8/Containerization-Seminar_1/assets/110223646/87a3c2c8-73d7-49eb-8eac-ea83b99cf62b)


Видим, что пакеты до сайта уходят нормально и ответ от сервера принимается.

Теперь, в оба терминала отправляем команду $ hostname, что бы увидеть наш хост.
hostname


![хост1](https://github.com/vladislavkrutov8/Containerization-Seminar_1/assets/110223646/e8fd21a3-0c91-4ee2-b0cc-cda3e07e9abf)

![хост2](https://github.com/vladislavkrutov8/Containerization-Seminar_1/assets/110223646/7865d4eb-8fa5-4117-9c7e-d83bd3770a20)

Как мы можем заметить - хоcт и там и там одинаков. А теперь, в изолированном терминале выполняем команду:


    sudo unshare -u bash
  
Команда $ unshare запускает программу (опционально) в новом namespace. Флаг -u говорит ей запустить bash в новом UTS namespace. Обратите внимание, что наш новый процесс bash указывает на другой файл UTS, тогда как все остальные остаются прежними.

Теперь мы можем изменить системный hostname из нашего нового процесса bash и это не повлияет ни на какой другой процесс в системе. Изменим наш хост в изолированном терминале, например:


    hostname geekbrains

Эта команда никак не затронула хост основной системы. Можем проверить это, выполнив hostname в первом терминале и увидев, что имя хоста там не изменилось.




![new_хост](https://github.com/vladislavkrutov8/Containerization-Seminar_1/assets/110223646/a308de35-8462-4129-92fc-a52f77634d90)


![old_хост](https://github.com/vladislavkrutov8/Containerization-Seminar_1/assets/110223646/432fcd51-dd3d-42f2-bd9e-df07fa9c60b5)
