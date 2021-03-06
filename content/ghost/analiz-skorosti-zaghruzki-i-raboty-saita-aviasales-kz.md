+++
date = "2015-08-10T20:57:03+06:00"
draft = false
title = "Анализ скорости загрузки и работы сайта Aviasales.kz"
slug = "analiz-skorosti-zaghruzki-i-raboty-saita-aviasales-kz"
aliases = [
	"analiz-skorosti-zaghruzki-i-raboty-saita-aviasales-kz"
]
tags = ["Метеор", "Скорость"]
categories = ["PageSpeed"]
author = "Nikolay Noskov"
email = "nvnoskov@gmail.com"
+++
Следующий на очереди Aviasales.kz, с показателем firstPaint в 685мс.

## Основные показатели
![](/images/2015/08/screen_012.png)

## Загрузка документа
Общий объём документа в 589Кб (в 4 раза меньше чем сайт bekair.com), обеспечивает быструю загрузку даже на скорости 1Мбит/сек. Сайт полностью грузится за 3.75 секунды. 
![](/images/2015/08/screen_013.png)


Минусом является тот факт, что проект расположен где то в Великобритании. Вследствие этого увеличивается пинг (~120мс) и, соответственно, время ожидания получения данных (TTFB в среднем около 250мс).



## Блокирующие загрузки
Все файлы в секции HEAD блокируют отображение страницы до момента их полной загрузки.
Крайне необходимо сокращать количество подобных ресурсов.
>Скрипты необходимо выносить в нижнюю часть документа, это никак не сказывается на их функциональности.

Единственным положительным моментов в данной картине является то, что часть этих ресурсов скачивается с [CDN-Yandex](https://tech.yandex.ru/jslibs/). 
Это позволяет выполнять их загрузку параллельно с другими ресурсами, да и средний TTFB в 60мс здесь играет роль.
![](/images/2015/08/screen_014.png)

## Timeline
![](/images/2015/08/screen_015.png)
На данном скриншоте изображены 7 секунд просмотра уже загруженного сайта. Желтые столбики это вызовы функций, зелёные это процессы прорисовки страницы. Частью этих вызовов и причиной постоянной прорисовки страницы является бегущая строка "Сейчас ищут:". Её анимация реализована при помощи функции JQuery Animate. Почему в современных браузерах этого делать не стоит,  очень хорошо описал [Paul Irish](http://paulirish.com/2012/why-moving-elements-with-translate-is-better-than-posabs-topleft/)
 
```JS
var r = function() {
    t.animate({
        left: "-" + e + "px"
    }, 300 * e / 5, function() {
        t.css({
            left: "0px"
        });
        r()
    })
};
r();
```

Большая половина вызовов JS-функций принадлежит [Yandex Webvisor](https://old.metrika.yandex.ru/promo/webvisor/), записывающей действия вашего пользователя на сайте.
Нужно учитывать её наличие, если вы собираетесь добиваться **60FPS**.



## Вывод 
Нужно сокращать количество блокирующих ресурсов в секции HEAD, и если основная аудитория сайта из Казахстана, то переносить его поближе, на хостинговые площадки Казахстана или России.

## Почитать ещё
- [Скорость загрузки и работы сайта- почему она важна](http://blog.vesna.kz/meteor-shop-skorost-pochiemu-ona-vazhna/)
- [Анализ скорости загрузки и работы сайта Aviasales.kz](http://blog.vesna.kz/analiz-skorosti-zaghruzki-i-raboty-saita-aviasales-kz/)