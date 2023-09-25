### Создайте хук для интеграции {#create-hook}

Чтобы привязывать Merge Requests из {{ GL }} к задачам в {{ tracker-name }}, создайте хук:

1. Если у вас еще нет OAuth-токена для работы с {{ tracker-name }} API, [получите его](../../tracker/concepts/access.md#section_about_OAuth).
1. Проверьте наличие доступа к API с помощью [запроса информации о текущем пользователе](../../tracker/get-user-info.md).
1. Откройте инстанс {{ GL }} и перейдите к настройкам хука:
    * Для включения интеграции на весь [инстанс {{ GL }}](../../managed-gitlab/concepts/index.md) (требуются права администратора {{ GL }}):
        1. В панели слева нажмите на раскрывающийся список **Your work** и выберите пункт **Admin Area**.
        1. Перейдите в раздел **System Hooks**.
    * Для включения интеграции на отдельный проект:
        1. Перейдите в необходимый проект.
        1. В панели слева нажмите кнопку **Settings** и выберите пункт **Webhooks**.
1. Укажите параметры хука:
    * **URL** — `https://api.tracker.yandex.net/v2/system/gitlab/receive?comments=true&<тип_организации>=<идентификатор_организации>`.

        Где:

        * `comments=true` — включает автоматическое создание комментариев в задаче со ссылкой и информацией о Merge Requests. Отключите, если комментирование не нужно. Параметр доступен только для {{ mgl-name }}.
        * `<тип_организации>` — может принимать следующие значения:

            * `x_cloud_org_id` — если тип вашей организации {{ org-full-name }}.
            * `x_org_id` — если тип вашей организации или {{ ya-360 }}, или одновременно {{ ya-360 }} и {{ org-full-name }}.

        * `<идентификатор-организации>` — идентификатор организации на [странице организаций {{ tracker-name }}]({{ link-tracker }}admin/orgs).

    * **Secret token** — OAuth-токен робота, от имени которого будут добавляться связи, в формате `OAuth <токен>`.
    * В блоке **Trigger** выключите все опции, кроме **Merge request events**.
    * В блоке **SSL verification** включите опцию **Enable SSL verification**.
1. Нажмите кнопку **Add system hook** (**Add webhook** для отдельного проекта).

На странице появится блок **System Hooks** (**Project Hooks** для отдельного проекта), в котором отобразится созданный хук и его параметры.