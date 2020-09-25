# О чём речь?

Диагностика и отладка ПО на контроллере Wiren Board - процесс, требующий много специфических знаний об устройстве Linux, навыков по обработке текстовых логов и немного магии. Это затрудняет работу для неопытных пользователей и для техподдержки.

Более того, работа системы логгирования в нынешнем виде может повлиять на работоспособность, вплоть до 

Какие **проблемы** выявлены с логами в WB:

* 🌪 логи **неструктурированы** - поиск и фильтрация нужных записей требует усилий \(например, получить записи, относящиеся только к конкретному устройству на шине\);
* 📌 логи **настраиваются только при запуске** - если требуется что-то отладить, нужно лезть в конфиг и перезапускать сервис, так можно потерять момент. А ещё они настраиваются сложно - что-то в конфигах, что-то в аргументах командной строки🤦♂ 
* 🧰 логи **глубоко спрятаны в контроллере** - добываются только через консоль, что затруднительно для пользователя;
* ⚠ запись логов **не сконфигурирована по умолчанию** - не настроена ротация, все записи сыпятся на диск и могут привести к сбоям в работе контроллера;
* 💀 в логах не хранится **информация о падениях** сервисов \(либо её недостаточно для точного определения проблемы\).

Также есть вещи, которые нельзя или очень сложно сделать только с помощью логов:

* ⏱ логи не дают возможности **быстро оценить общую картину** происходящего в контроллере - нужен детальный анализ всех записей;
* 🔕 логи не уведомляют о **важных событиях** в системе.

Для того, чтобы решить или нивелировать эти проблемы, я предлагаю следующий план \(заголовки - ссылки на подробную информацию\):

* [ ](koncepciya/logi-2.0.md)💪 [Логи 2.0](koncepciya/logi-2.0.md)
  * добавление **метаинформации** к записям \(файл и строка, версия ПО, ID сообщения, ID запуска, категория и т.п.\), которые позволят извлечь из логов больше пользы при анализе и фильтрации;
  * **настройка на лету,** без необходимости перезапускать сервис. В том числе разные уровни для отдельных категорий, если нужно разобраться, что с конкретным устройством;
  * добавление в логи **stacktrace** при падении сервиса для post-mortal анализа;
  * вдумчивая настройка **ротации по умолчанию**, чтобы у пользователей не ломались контроллеры из-за забытого debug-вывода в wb-mqtt-serial;
  * **one-click выгрузка логов** для быстрой передачи информации в техподдержку \(вплоть до кнопки "Отправить на форум"\).
*  🎯 [Диагностики](koncepciya/diagnostiki.md)
  * способ быстро получить **общую информацию** о состоянии системы и о проблемах;
* 🔔 [Уведомления в интерфейсе](koncepciya/uvedomleniya.md)
  * способ быстро получить **информацию о событиях**: обновления, нехватка места в памяти, проблемы со связью на шине \(и ссылку на форум/документацию\);
* 👁🗨 [Мониторинг сервисов](koncepciya/monitoring-servisov.md)
  * фронтенд для `systemctl status` на основе диагностик и уведомлений.

На следующих страницах подробней рассказано обо всех идеях и способах их реализации на практике.

