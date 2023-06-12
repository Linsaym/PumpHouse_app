<h1 align="center"><img src="https://vuejs.org/logo.svg" height="32"/> Водокачка+ <img src="https://vuejs.org/logo.svg" height="32"/></h1>
<h2 align="center">Vue + Laravel</h2>
<h3 align="center">Приложение представляет собой небольшую АИС, в который вы как хозяин водокачки можете заносить и изменять показания счётчика, изменять цену воды за кубометр и добавлять новых пользователей к вашему огороду</h3>
<h3 align="center">Приложение же в свою очередь фильтрует и отображает(в зависимости от выбранного месяца) в таблице занесённых вами пользователей, а так же производит расчёты сколько каждый дачник должен вам заплатить.</h3>

<h4 align="center">По всем вопросам сюда <a href="https://vk.com/linsaym">https://vk.com/linsaym </a></h4>

## Для запуска сервера

Убедитесь что у вас установленны "<a href="">Open Server</a>" и "<a href="https://getcomposer.org/">Composer</a>"

####

В настройках <a href="">Open Server</a> выберите версию PHP - 8.1, базу данных 'MySQL 8.0-Win10' и "Apache 2.4-PHP
8.0-8.1+Nginx 1.23"

####

Запустите Open Server (флажок в трее должен быть зелёным)

##

Далее, клонируйте репозиторий c API себе:

```sh
git clone https://github.com/Linsaym/PumpHouse_API
```
Переименуйте файл <strong>.env.example</strong> в <strong>.env</strong> и настройте файл под себя.
####
Обязательно установите <strong>DB_DATABASE=pumphouse</strong> и <strong>APP_URL=http://build</strong>
####
И последовательно выполните следующие команды:

```sh
composer install
```

```sh
php artisan key:generate
```

```sh
php artisan migrate
```

```sh
php artisan serve
```

Либо вы можете использовать уже предложенную базу
данных <a href='https://github.com/Linsaym/PumpHouse_API/blob/master/pumphouse.sql'>pumphouse.sql</a> и не выполнять
команду "php artisan migrate"

Теперь для запуска приложения достаточно взять папку build из этого репозитория и поместить её в папку domains в Open
Server. И можно будет пользоваться приложением :)

####

P.s. Не забудьте запустить сервер командой 'php artisan serve' перед открытием приложения

####

P.s.s. Чтобы найти папку domains нажимаем пкм по флажку в трее, и далее "Папка с проектами". После опять нажимаем пкм по
флажку, перезагружаем Open Server, нажимаем "Мои проекты", выбираем build и наслаждаемся

## Для Fronted разработки

<p>Первым делом установите <a href="https://nodejs.org/en">Node.js</a></p>
Далее, клонируйте репозиторий себе:

```sh
git clone https://github.com/Linsaym/PumpHouse_app.git
```

И последовательно выполните следующие команды:

```sh
npm install
```

```sh
npm run dev
```

## Билд проекта

```sh
npm run build
```
