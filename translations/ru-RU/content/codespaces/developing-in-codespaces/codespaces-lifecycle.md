---
title: Жизненный цикл Codespaces
intro: Вы можете разрабатывать данные в среде {% variables.product.prodname_github_codespaces %} и поддерживать данные на протяжении всего жизненного цикла пространства кода.
versions:
  fpt: '*'
  ghec: '*'
type: overview
topics:
- Codespaces
- Developer
product: '{% data reusables.gated-features.codespaces %}'
ms.openlocfilehash: 08fd96d66aaf5017ff9305d7b33d80a875cb5ab5
ms.sourcegitcommit: 6bc8b888e02cc31ec01464186ed4530889cf2408
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/20/2022
ms.locfileid: "148101495"
---
## Сведения о жизненном цикле пространства кода

Жизненный цикл пространства кода начинается при его создании и заканчивается при его удалении. Вы можете отключиться и переподключиться к активному пространству кода, не затрагивая выполняемые процессы. Вы можете остановить и перезапустить пространство кода без потери изменений, внесенных в проект.

## Создание codespace

Когда вы собираетесь работать над проектом, вы можете создать новое пространство кода или открыть существующее. Может потребоваться создать новое пространство кода из ветви проекта при каждой разработке в {% данных variables.product.prodname_github_codespaces %} или сохранить длительное пространство кода для функции. Дополнительные сведения см. в статье [Создание кодового пространства](/codespaces/developing-in-codespaces/creating-a-codespace).

{% data reusables.codespaces.max-number-codespaces %} Аналогичным образом, если вы достигнете максимального количества активных сред codespace и попытаетесь запустить еще одну среду, вам будет предложено остановить одну из активных сред codespace.

Если вы решили создавать новое пространство кода каждый раз при работе над проектом, следует регулярно отправлять изменения, чтобы все новые фиксации были включены в {% data variables.product.prodname_dotcom %}. Если вы решили использовать для своего проекта долгосрочное пространство кода, то должны выполнять вытягивание из ветви по умолчанию репозитория каждый раз, когда начинаете работать в пространстве кода, чтобы в вашей среде присутствовали последние фиксации. Этот рабочий процесс очень похож на работу с проектом на локальном компьютере. 

{% data reusables.codespaces.prebuilds-crossreference %}

## Сохранение изменений в пространстве кода

При подключении к пространству кода через Интернет для веб редактора автоматически включается автосохранение и настраивается сохранение изменений после задержки. При подключении к пространству кода через {% data variables.product.prodname_vscode %}, работающий на вашем компьютере, необходимо самостоятельно включить автосохранение. Дополнительные сведения см. в разделе [Сохранение и автосохранение](https://code.visualstudio.com/docs/editor/codebasics#_save-auto-save) документации по {% data variables.product.prodname_vscode %}.

Если вы хотите сохранить изменения в репозитории Git в файловой системе пространства кода, зафиксируйте их и отправьте в удаленную ветвь.

Если у вас есть несохраненные изменения, перед выходом редактор предложит сохранить их.

## Время ожидания Codespaces

Если вы оставите свое пространство кода работающим без взаимодействия или если выйдете из него, не остановив его явно, после некоторого периода бездействия время ожидания истечет, и пространство кода остановит работу. По умолчанию время ожидания пространства кода истекает через 30 минут бездействия, но вы можете настроить другое время ожидания для вновь создаваемых пространств кода. Дополнительные сведения о настройке времени ожидания по умолчанию для пространств кода см. в разделе [Настройка времени ожидания для {% data variables.product.prodname_github_codespaces %}](/codespaces/customizing-your-codespace/setting-your-timeout-period-for-github-codespaces). Дополнительные сведения об остановке пространства кода см. в разделе [Остановка пространства кода](#stopping-a-codespace).

При истечении времени ожидания пространства кода данные сохраняются с момента последнего сохранения изменений. Дополнительные сведения см. в разделе [Сохранение изменений в пространстве кода](#saving-changes-in-a-codespace).

## Перестроение пространства кода

Вы можете перестроить пространство кода, чтобы восстановить исходное состояние, как если бы вы создали новое пространство кода. В большинстве случаев вместо перестроения пространства кода можно просто создать новое пространство кода. Скорее всего, вы захотите перестроить пространство кода для реализации изменений в контейнере разработки. При перестроении пространства кода все контейнеры Docker, образы, тома и кэши очищаются, а затем пространство кода перестраивается.

Если вам нужно сохранить какие-либо из этих данных, можно создать в нужном расположении в контейнере символьную ссылку на постоянный каталог. Например, в каталоге `.devcontainer` можно создать каталог `config`, который будет сохранен при перестроении. Затем вы можете связать символьной ссылкой каталог `config` и его содержимое как `postCreateCommand` в файле `devcontainer.json`.

```json  
{
    "image": "mcr.microsoft.com/vscode/devcontainers/base:alpine",
    "postCreateCommand": ".devcontainer/postCreate.sh"
}
```

В приведенном ниже примере файла `postCreate.sh` содержимое каталога `config` связано символической ссылкой с домашним каталогом.

```bash
#!/bin/bash
ln -sf $PWD/.devcontainer/config $HOME/config && set +x
```

Дополнительные сведения см. в статье [Общие сведения о контейнерах разработки](/codespaces/setting-up-your-project-for-codespaces/introduction-to-dev-containers#applying-configuration-changes-to-a-codespace).

## Остановка пространства кода

{% данных reusables.codespaces.stopping-a-codespace %} Дополнительные сведения см. в разделе "[Остановка и запуск пространства кода](/codespaces/developing-in-codespaces/stopping-and-starting-a-codespace)".

## Удаление codespace

Вы можете создать пространство кода для конкретной задачи, а затем безопасно удалить его после отправки изменений в удаленную ветвь.

Если вы попытаетесь удалить пространство кода с неотправленными фиксациями git, редактор сообщит вам, что имеются изменения, которые не были отправлены в удаленную ветвь. Вы можете отправить все необходимые изменения, а затем удалить пространство кода или продолжить, чтобы удалить пространство кода и все незафиксированные изменения. Вы также можете экспортировать код в новую ветвь, не создавая новое пространство кода. Дополнительные сведения см. в разделе [Экспорт изменений в ветвь](/codespaces/troubleshooting/exporting-changes-to-a-branch).

Пространства кода, которые были остановлены и остаются неактивными в течение указанного периода времени, будут удалены автоматически. По умолчанию неактивные пространства кода удаляются через 30 дней, но вы можете настроить период хранения пространства кода. Дополнительные сведения см. в статье [Настройка автоматического удаления сред codespace](/codespaces/customizing-your-codespace/configuring-automatic-deletion-of-your-codespaces).

Если вы создаете пространство кода, плата за хранение будет взиматься до тех пор, пока она не будет удалена, независимо от того, активна ли она или остановлена. Дополнительные сведения см. в статье [Сведения о выставлении счетов за {% data variables.product.prodname_github_codespaces %}](/billing/managing-billing-for-github-codespaces/about-billing-for-github-codespaces#billing-for-storage-usage). Удаление пространства кода не уменьшает текущую оплачиваемую сумму для {% данных variables.product.prodname_github_codespaces %}, которая накапливается в течение каждого ежемесячного цикла выставления счетов. Дополнительные сведения см. в разделе [Просмотр данных об использовании {% data variables.product.prodname_github_codespaces %}](/billing/managing-billing-for-github-codespaces/viewing-your-github-codespaces-usage).

Дополнительные сведения об удалении пространства кода см. в разделе [Удаление пространства кода](/codespaces/developing-in-codespaces/deleting-a-codespace).

## Потеря подключения при использовании Codespaces

{% данных variables.product.prodname_github_codespaces %} — это облачная среда разработки и требует подключения к Интернету. Если во время работы в пространстве кода произойдет потеря подключения к Интернету, вы не сможете получить доступ к своему пространству кода. Однако все незафиксированные изменения будут сохранены. Когда подключение к Интернету будет восстановлено, вы сможете подключиться к пространству кода в том же состоянии, в котором оно было оставлено. Если ваше подключение к Интернету нестабильно, следует чаще фиксировать и отправлять изменения.

Если вы знаете, что вы часто работаете в автономном режиме, вы можете использовать `devcontainer.json` файл с [расширением "Контейнеры разработки"](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) для {% данных variables.product.prodname_vscode_shortname %} для сборки и присоединения к локальному контейнеру разработки для репозитория. Дополнительные сведения см. в разделе [Разработка в контейнере](https://code.visualstudio.com/docs/remote/containers) в документации по {% data variables.product.prodname_vscode %}.
