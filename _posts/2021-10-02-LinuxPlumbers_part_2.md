---
layout: post
title: Linux Plumbers Conference 2021 [part 2]
---
Продолжим рассматривать материалы конференции Linux Plumbers 2021, но на этот раз не будет привязываться к конкретному треку. Прежним останется только повышенный интерес к докладам, связанным с информационной безопасностью.   

##### Compiler Features for Kernel Security

[Доклад](https://linuxplumbersconf.org/event/11/contributions/1026/) сразу пропускает общеизвестные опции компилятора, такие как стековые канарейки и позиционно-независимое исполнение. 

Рассматривались следующие возможности компиляторов:

- реализация отдельных "канареек" для каждого потока ядра, флаги:
  ```
  -mstack-protector-guard=sysreg
  -mstack-protector-guard-reg=sp_el0
  -mstack-protector-guard-offset=0
  ```
  *поддерживаемые в компиляторах Clang и GCC платформы для указанных выше флагов: x86, powerpc, arm64, riscv(только GCC);*

- очистка регистров при выходе из функций (добавилась в GCC 11, рассматривал ее в первой части обзора конференции), флаг:

```
-fzero-call-used-regs
```

*сделал [pull request](https://github.com/a13xp0p0v/linux-kernel-defence-map/pull/4) в [карту средств защиты ядра Linux](https://github.com/a13xp0p0v/linux-kernel-defence-map) про CONFIG_ZERO_CALL_USED_REGS ядра, добавляющий сборку ядра с данной опцией*;

- автоматическая инициализация стековых (локальных) переменных (есть в Clang, реализуется в GCC 12), флаг:

```
-ftrivial-auto-var-init={zero,pattern}
```

- проверка выхода за границы массивов, флаги:

```makefile
-Warray-bounds
-Wzero-length-bounds
-Wzero-length-array //предупреждение об использовании массива нулевого размера, если он перекрывается другим массивом
-fsanitize=bounds //проверка границ массивов
-fsanitize=bounds-strict //проверка границ массивов
```

- защита от переполнения типа int, флаги:

```
-fsanitize=signed-integer-overflow //есть в GCC и Clang
-fsanitize=unsigned-integer-overflow // есть только в Clang
```

- флаги для Control Flow Integrity:

```
-flto //Link Time Optimization
-flto=thin //Link Time Optimization
-mbranch-protection=pac-ret[+leaf] //backward edge Control Flow Integrity
-fsanitize=shadow-call-stack //backward edge Control Flow Integrity
-fcf-protection=branch //forward edge Control Flow Integrity
-mbranch-protection=bti //forward edge Control Flow Integrity
-fsanitize=cfi //forward edge Control Flow Integrity
```

- меры защиты от Spectre v1

```
-mspeculative-load-hardening 
```

- рандомизация структур в программах на языке C: 

```
__attribute__((randomize_layout))
```

.

##### A bug is NOT a bug is NOT a bug: Differences in bug classes, bug tracking and bug impact

[Доклад](https://linuxplumbersconf.org/event/11/contributions/1080/) про системы выявления и  ведения записей об уязвимостях ядра Linux.

Рассмотрены уязвимости обнаруженные:

-  пользователями ядра;
- автоматизированными системами тестирования;
- компиляторами;
- фазерами;
- и статическими анализаторами,

а также системы интеграции всех этих данных.

##### Measuring Code Review in the Linux Kernel Development Process

[Доклад](https://linuxplumbersconf.org/event/11/contributions/905/) про вклад разработчиков в процесс разработки ядра Linux. Intel, Google, IBM - компании с наибольшим количество сотрудников, участвующих в разработке ядра. drivers/hid/hid-ids.h - самый популярный файл, больше всего коммитов. 7.94% трафика обсуждения патчей формируют боты.

##### Detecting semantic bugs in the Linux kernel using differential fuzzing

[Доклад](https://linuxplumbersconf.org/event/11/contributions/1033/) про использование дифференциального фаззинга для обнаружения семантических ошибок средством syz-verifier. Суть метода в проведения фаззинга системных вызовов нескольких версий ядра Linux syzkaller'ом на одинаковых данных. При выявлении различий в выходных данных на разных версиях ядра, считается что хотя бы в одной из них имеется ошибка. Дальше дело за *code review*.

##### Firmware and Bootloader Logging

[Доклад](https://linuxplumbersconf.org/event/11/contributions/1119/) про логирование работы загрузчика GRUB2 и про возможность распространения данного подхода на прошивки и предзагрузчики.

##### Traceability and code coverage: what we have in Linux and how it contributes to safety

[Доклад](https://linuxplumbersconf.org/event/11/contributions/1075/) про тестирование и покрытие кода ядра Linux в рамках проекта [Kernel CI](https://kernelci.org/) (все тесты также open-sources, как я понял). Для сбора покрытия используется [lcov](https://github.com/linux-test-project/lcov). Представлены скрипты и конфигурационные файлы для сбора покрытия, задан вопрос в массы насколько тема покрытия кода ядра актуальна и нужна, и какой процент покрытия реально достижим (90% мне кажется очень круто, цифра 70 выглядит более реалистичной).

##### Guider: Linux Tracing using Python

[Доклад](https://linuxplumbersconf.org/event/11/contributions/914/) про инструмент [Guider](https://github.com/iipeace/guider), который предоставляет удобный графический интерфейс для различных функции трассировки Linux с использованием ftrace, ptrace и procfs. Написан на python, работает Linux и Android, ограниченно на Windows и MacOS. Функции:

- мониторинг:
  - ресурсов;
  - задач;
  - функций (C, C++, Rust, Go, Java, Node.js, Python ...);
  - системных вызовов;
  - файлов;
- профилирование:
  - ресурсов (CPU, Memory, Network, Storage);
  - задач;
- трассировка:
- визуализация ресурсов, шедуллера, стека вызовов;



 

