Чтобы удалить ресурсы, созданные с помощью {{ TF }}:
1. Выполните команду:

   ```bash
   terraform destroy
   ```

   {% note alert %}

   {{ TF }} удалит все ресурсы, созданные в текущей конфигурации: кластеры, сети, подсети, виртуальные машины и т. д.

   {% endnote %}

   После выполнения команды в терминал будет выведен список удаляемых ресурсов.

1. Введите слово `yes` и нажмите **Enter**.