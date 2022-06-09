# Общие вопросы про {{ dns-name }}

#### Почему списывается плата за {{ dns-name }}, если в моем облаке нет DNS-зон? {#dns-fee-without-usage}

[Публичные DNS-запросы](../concepts/dns-zone.md#public-zones) с виртуальных машин к DNS-серверам Яндекса также [тарифицируются](../pricing.md#public-dns-requests).

Поэтому сервис {{ dns-name }} используется даже в том случае, если в вашем облаке нет никаких DNS-зон, кроме сервисных.

Рекомендуем использовать [кеширующие резолверы](../tutorials/local-dns-cache.md), например: `systemd-resolved`, `dnsmasq`, `unbound`. С их помощью можно снизить количество публичных DNS-запросов и, таким образом, уменьшить расходы.

{% include [qa-logs.md](../../_includes/qa-logs.md) %}