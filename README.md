# Домашнее задание к занятию "12.4 Развертывание кластера на собственных серверах, лекция 2"
Новые проекты пошли стабильным потоком. Каждый проект требует себе несколько кластеров: под тесты и продуктив. Делать все руками — не вариант, поэтому стоит автоматизировать подготовку новых кластеров.

## Задание 1: Подготовить инвентарь kubespray
Новые тестовые кластеры требуют типичных простых настроек. Нужно подготовить инвентарь и проверить его работу. Требования к инвентарю:
* подготовка работы кластера из 5 нод: 1 мастер и 4 рабочие ноды;
* в качестве CRI — containerd;
* запуск etcd производить на мастере.

### Ответ: 

Для начала подготавливаем при помощи скрипта инфраструктуру:

![image](https://user-images.githubusercontent.com/92969676/188070247-462ea058-151e-448b-bb01-1cd8d004a6bc.png)

![image](https://user-images.githubusercontent.com/92969676/188070499-d1dc4be1-8fac-454d-8bc0-1ef4405cfa48.png)

Далее подеключаемся к мастер ноде, устанавливаем git "sudo apt install git", 

![image](https://user-images.githubusercontent.com/92969676/188087918-0cac6241-612a-4c71-9c98-fbd703ae2711.png)

клонируем репозиторий kubespray git clone https://github.com/kubernetes-sigs/kubespray

![image](https://user-images.githubusercontent.com/92969676/188087856-0fbebb48-8319-40fe-85e4-2cca19a8f3f7.png)

Ставим python (sudo apt install pip) и далее ставим зависимости: sudo pip3 install -r requirements.txt 

![image](https://user-images.githubusercontent.com/92969676/188088882-da968179-6dc7-4d47-89ba-74a430890580.png)

Создаем папку с копией конфигурации, которую в дальнейшем будем использовать (cp -rfp inventory/sample inventory/mycluster).

Далее создаем host.yaml с перечнем наших нод, и редактируем inventory файл таким образом, как сказано в задании, т.е. etcd должно быть на master ноде:

![image](https://user-images.githubusercontent.com/92969676/188114936-83666021-4f52-4cea-a732-4ea336ab7aeb.png)

В качестве CRI у нас conteinerd, это по умолчанию уже установлено для конфигурации кластера: 

![image](https://user-images.githubusercontent.com/92969676/188117124-b7dca833-8df9-499a-8750-b2246677e1d9.png) 

Далее запускаем установку кластера: 

![image](https://user-images.githubusercontent.com/92969676/188116269-a5a66ce2-d5e1-45bc-bd55-624203f56f47.png)

В итоге кластер развернут, это заняло (на 1 мастер и 4 воркер ноды) 20 минут 35 секунд:

![image](https://user-images.githubusercontent.com/92969676/188119920-d6571e8e-6ff3-4c68-9fc0-18918825efc9.png)

Проверяем, что кластер действительно развернут:

![image](https://user-images.githubusercontent.com/92969676/188120402-d88cc354-8458-46ab-b08e-1a811335973a.png)

На лекции еще сказали, что надо будет сделать так, чтобы через kubectl можно было подключиться к внешнему адресу нашего мастера.


Проверяем, что сейчас с локальной машины мы не можем управлять кластером: 

![image](https://user-images.githubusercontent.com/92969676/188121099-a0918201-7ce6-44cc-92e1-a65fa2618292.png)

Редактируем наш локальный конфиг, и добавляем информацию об удаленном кластере, о его пользователе, наименовании, адресации и ключах авторизации:

![image](https://user-images.githubusercontent.com/92969676/188123340-520d3449-cc50-4af3-aa0a-01d44ae4d7b8.png)

Далее смотрим, что у нас контекст появился в конфиге:

![image](https://user-images.githubusercontent.com/92969676/188123756-8a3b23a5-f2f5-4094-8eec-2d96dcea35aa.png)

Переключаемся на него:

![image](https://user-images.githubusercontent.com/92969676/188123808-1c8075bb-5079-434c-b05e-885de897a90e.png)

Но подключение не получилось, т.к. выданный сертификат был выдан на другие ip адреса: 

![image](https://user-images.githubusercontent.com/92969676/188124016-8c469509-b5ba-485f-9b1b-04bdfd2ee57d.png)

Для разрешения данной проблемы меняем на master информацию о том, что сертификат надо выдать на внешний адрес, а именно: 

![image](https://user-images.githubusercontent.com/92969676/188124678-9458b2fa-4d49-40a0-854b-066c358a150a.png)

И запускаем повторную установку кластера. Повторная установка пройдет быстрее, т.к. ansible уже проверит на идемпотентность код и будет пропускать многие шаги.

Повторная установка прошла за 11 минут 40 секунд.

![image](https://user-images.githubusercontent.com/92969676/188126765-9a88a594-76c6-4c0e-b008-e0d6e7588c26.png)

Теперь повторно проверяем с нашей локальной машины управление удаленным кластером, подключение проходит успешно:

![image](https://user-images.githubusercontent.com/92969676/188126887-e21151d2-b48d-4b0d-ac3b-ec3ee35c3198.png)













