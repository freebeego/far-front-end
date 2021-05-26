## Сайт для НКО «ФАР»

#### Технологический стек: HTML, CSS, БЭМ, JavaScript, NUXT(Vue), React, express, mongoDB, webpack, strapi, npm, git, linux, bash

#### Реализованные возможности:
Форма обратной связи (информация отправленная пользователем приходит на корпоративный email организации)
Интегрирован сервис Юmoney для приема пожертвований (однократных и регулярных)
Локализация средствами Nuxt i18n и Strapi
Для реализации админки сайта используется headless CMS Strapi
Написан плагин для управления пунктами меню (сортировка, подключение целевых страниц)
Кастомизирован текстовый редактор для поля с контентом
Практически весь контент на страницах можно изменять
Так же можно создавать шаблонные страницы

https://far.listen-me.ru/
https://far.listen-me.ru/api/admin

https://far.listen-me.ru

requirements:

"engines": {
    "node": ">=10.16.0 <=14.x.x",
    "npm": "^6.0.0"
  }
  
Проект работает в режиме SSR
Запускается: 
npm i
NODE_ENV=production npm run build
npm start / pm2 start
  
