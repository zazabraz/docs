# Обработка секретов, попавших в открытый доступ

В {{ yandex-cloud }} используются следующие секреты:

* [IAM-токен](../concepts/authorization/iam-token.md);
* [OAuth-токен](../concepts/authorization/oauth-token.md);
* [авторизованные ключи](../concepts/authorization/key.md);
* [JWT](iam-token/create-for-sa.md#via-jwt);
* [статические ключи](../concepts/authorization/access-key.md);
* [API-ключи](../concepts/authorization/api-key.md);
* cookie в браузере.

Чтобы обеспечить безопасность ваших данных и инфраструктуры, внимательно следите за использованием секретов. Если секреты были скомпрометированы, исключите использование этих секретов.


## Отозвать и перевыпустить секреты {#revoke-credentials}

Не все секреты можно отозвать, например IAM-токен или JWT. Если скомпрометирован такой секрет, выполните одно из следующих действий:

* Отзовите права у аккаунта, которому принадлежит секрет.
* Удалите сервисный аккаунт или исключите из организации пользовательский аккаунт.
* Удалите ключ, с помощью которого создан секрет. Например, если скомпрометирован JWT или IAM-токен, который создан с помощью JWT, удалите авторизованный ключ сервисного аккаунта.

{{ yandex-cloud }} позволяет выпускать несколько авторизованных, статических и API-ключей одновременно. Сначала выпустите новые ключи, предоставьте их соответствующим сервисам и пользователям, а потом отзовите старые.


### IAM-токен {#iam-reissue}

Удалить IAM-токен нельзя. Если вы создадите новый IAM-токен, старый продолжит действовать.

1. Чтобы злоумышленник не смог использовать токен:

    * Для сервисного аккаунта – [перевыпустите](#key-reissue) авторизованный ключ сервисного аккаунта, на который выдан токен, или удалите аккаунт.
    * Для аккаунта на Яндексе или федеративного аккаунта выполните одно из действий:

        * [исключите аккаунт](../../organization/edit-account.md) из всех организаций на время действия токена;
        * отзовите права у аккаунта [в облаках](users/delete) и [организациях](../../organization/roles.md#revoke) на время, пока действует токен.

            {% note info %}

            IAM-токен действует не больше 12 часов. Подробнее о [времени жизни IAM-токена](../concepts/authorization/iam-token.md#lifetime).

            {% endnote %}

2. Создайте новый IAM-токен:

    * [Инструкция для аккаунта на Яндексе](iam-token/create.md);
    * [Инструкция для сервисного аккаунта](iam-token/create-for-sa.md);
    * [Инструкция для федеративного аккаунта](iam-token/create-for-federation.md).


### OAuth-токен {#oauth-reissue}

OAuth-токен можно отозвать. При этом IAM-токен, для получения которого используется OAuth-токен, продолжит действовать.

Чтобы злоумышленник не смог использовать токен:

1. [Отзовите OAuth-токен](https://yandex.ru/dev/id/doc/dg/oauth/reference/token-invalidate.html).

1. Отзовите права у аккаунта, которому принадлежит OAuth-токен, одним из способов:

    * [исключите аккаунт](../../organization/edit-account.md) из всех организаций на время, пока действует IAM-токен;
    * отзовите права у аккаунта [в облаках](users/delete) и [организациях](../../organization/roles.md#revoke) на время, пока действует IAM-токен.

        {% note info %}

        IAM-токен действует не больше 12 часов. Подробнее о [времени жизни IAM-токена](../concepts/authorization/iam-token.md#lifetime).

        {% endnote %}

1. [Получите новый OAuth-токен]({{ link-cloud-oauth }}).


### Авторизованный ключ {#key-reissue}

Если вам нужно максимально быстро пресечь угрозы со стороны скомпрометированного ключа, [удалите](sa/delete.md) сервисный аккаунт.

Если для вас важнее непрерывность процесса, в котором участвует сервисный аккаунт, перевыпустите авторизованные ключи:

1. [Создайте новый авторизованный ключ](authorized-key/create.md) для сервисного аккаунта.
1. Предоставьте новый авторизованный ключ сервисам и пользователям, которые его используют.
1. [Получите IAM-токен](../../iam/operations/iam-token/create-for-sa.md) для нового авторизованного ключа.
1. [Удалите старый авторизованный ключ](./authorized-key/delete.md).

Когда вы удалите авторизованный ключ, соответствующий ему IAM-токен перестанет действовать. Чтобы исключить угрозы со стороны скомпрометированного ключа, больше делать ничего не требуется.


### JWT {#jwt-reissue}

Выполните действия, описанные в разделе [Авторизованный ключ](#key-reissue).


### Статический ключ {#access-key-reissue}

1. [Создайте новый статический ключ](sa/create-access-key.md) для сервисного аккаунта.
1. Предоставьте новый статический ключ сервисам и пользователям, которые его используют.
1. [Удалите старый статический ключ](./sa/delete-access-key.md).


### API-ключ {#api-key-reissue}

1. [Создайте новый API-ключ](api-key/create.md) для сервисного аккаунта.
1. Предоставьте новый API-ключ сервисам и пользователям, которые его используют.
1. [Удалите старый API-ключ](./api-key/delete.md).


### Прекратить действие cookie {#cookie-invalidation}

1. [Измените пароль](https://yandex.ru/support/id/profile.html) Яндекс ID. 
1. [Авторизуйтесь в Яндекс ID](https://passport.yandex.ru/) с новым паролем.


## Поиск несанкционированных действий и ресурсов {#searching-unauthorized-access}

После того как отзовете и перевыпустите секреты, проанализируйте доступ к вашим ресурсам {{ yandex-cloud }}.

1. [Изучите записи](../../logging/operations/read-logs.md) {{ cloud-logging-name }}.
1. Выполните [поиск событий в бакете](../../audit-trails/tutorials/search-bucket.md) и [поиск событий в лог-группе](../../audit-trails/tutorials/search-cloud-logging.md) {{ at-name }}.
1. Убедитесь, что все события, в том числе связанные с утечкой секретов, соответствуют ожиданиям.

{% note tip %}

Вы можете настроить [экспорт аудитных логов в SIEM](../../audit-trails/concepts/export-siem.md).

{% endnote %}


## Удалить несанкционированные ресурсы {#delete-unauthorized-resources}

1. Проверьте, не появились ли в {{ yandex-cloud }} ресурсы, которые вы не создавали, например виртуальные машины, хранилища данных, базы данных, функции, API-шлюзы и т.д.
1. Удалите несанкционированные ресурсы, прежде всего те, которые могут обрабатывать данные или другим образом компрометировать вашу информацию.


## Обратиться в службу технической поддержки {#support}

Сообщите в [службу технической поддержки]({{ link-console-support }}) об инциденте. Эта информация поможет усовершенствовать защиту секретов в будущих релизах облака.

Подробнее о [порядке оказания технической поддержки](../../support/overview.md).


## Рекомендации по построению безопасной инфраструктуры {#recommendations}

1. Отделите секреты от кода.

    Не используйте секреты в исходном коде. Вместе с ним они могут попасть в публичные репозитории, например на GitHub, и стать уязвимыми.

1. Обеспечьте [управление секретами в облаке](../../security/domains/encryption.md#upravlenie-sekretami).
1. Обеспечьте [cбор, мониторинг и анализ аудитных логов](../../security/domains/audit-logs.md).