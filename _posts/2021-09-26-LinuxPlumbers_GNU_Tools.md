---
layout: post
title: Linux Plumbers Conference 2021 - GNU Tools track
---
Одна из самых моих любимых конференций Linux Plumbers прошла на завершающейся неделе (20-24 сентября). Для меня она всегда проходила виртуально и удаленно, но в этом году такой формат для всех. Детальная программа доступна по [ссылке](https://linuxplumbersconf.org/event/11/timetable/?view=lpc), а я расскажу о том, что заинтересовало меня. Сегодня речь пойдет о докладах ***GNU Tools track***.

##### GCC's -fanalyzer option: what's new in GCC 12

Тема доклада [GCC's -fanalyzer option: what's new in GCC 12](https://linuxplumbersconf.org/event/11/contributions/996/attachments/772/1453/2021-LPC-analyzer-talk.pdf) понятна из названия.
В версии GCC 10 появилась опция -fanalyzer, которая добавила межпроцедурный проход анализа кода.  Данная опция позволяет задействовать более тяжеловесный анализ, который более свойственен серьезным статическим анализаторам.

Анализ выполняется для графов потоков управления и данных. Также используется символьное исполнение. Автор признается в несовершенстве анализатора, но все равно это переход статического анализатора GCC на новый уровень. Опция добавила в GCC 10 - 15, а в GCC 11 - 5 новых типов предупреждений. 

В планах добавление в GCC 12 детектирования переполнения буфера и поддержки работы на коде ядра Linux.

Домашняя страница проекта - <https://gcc.gnu.org/wiki/DavidMalcolm/StaticAnalyzer>.

##### Security improvements in GCC

Следующий интересный доклад [Security improvements in GCC](https://linuxplumbersconf.org/event/11/contributions/1001/)  касается функций безопасности GCC. Рассматриваются две функции из Security Wishlist for GCC, реализация которых в GCC позволяет ему выровняться с Clang'ом.

Первая функция - очистка регистров при выполнении инструкции return, она является мерой противодействия реализации ROP-атак. Данные атаки используют куски готового кода (гаджеты) библиотек и приложений и главной их целью, как правило, является использование системных вызовов, параметры в которые часто передаются через регистры. Каждый гаджет заканчивается инструкцией возврата управления (return, но может быть и переход по метке). Очистка регистров при return'е сделает невозможным передать через них параметры системного вызова и сильно понизит полезность многих гаджетов. Данная функция компилятора уже есть в Clang и была добавлена в GCC 11.

Вторая функция - автоматическая инициализация локальных переменных, расположенных в стеке. Данная функция уже присутствует в компиляторах Microsoft, Clang и призвана защитить от ошибок использования неинициализированных переменных. В GCC ее добавить планируется в версии 12.

#####  gprofng - The next generation GNU profiler

Последний в обзоре данного трека доклад [gprofng - The next generation GNU profiler](https://linuxplumbersconf.org/event/11/contributions/1001/) к сожалению был представлен без размещения презентации на сайте, поэтому все данные из аннотации доклада. Также немного информации и видео доклада представлены [здесь](https://www.phoronix.com/scan.php?page=news_item&px=GNU-Profiler-gprofng).

Gprofng, инструмент профилирования для Linux, базируется на анализаторе производительности Oracle Developer Studio. Он включает в себя несколько инструментов для сбора и анализа данных о производительности.

В отличии от gprof средство gprofng предлагает полную поддержку многопоточности, а также возможность использования при отсутствии исходного кода целевого исполняемого файла, поскольку перекомпилировать или инструментировать код не требуется.

Также есть возможность сравнивать несколько отчетов профилирования.

 

