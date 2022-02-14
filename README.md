# README #

## Install ##

npm i

npm install capacitor-resources (if not install)


// package.json need

{   
    ...   
    "scripts": {
        ...
        "resources": "capacitor-resources -p android,ios" 
    } 
}

\\



ionic build & npx cap add android & npx cap add ios
(https://www.npmjs.com/package/capacitor-resources)

or

## Билд проекта ##
ionic build 

## Добавление платформы ##
ionic cap add ios 
ionic cap add android

## Запуск студии ##
ionic cap open ios 
ionic cap open android

Переместить google-services.json в andond/app

## Создание иконок ##
Add your icon.png (1024x1024 px) and splash.png (2732x2732 px) in /resources
capacitor-resources
npm run resources