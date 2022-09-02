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


