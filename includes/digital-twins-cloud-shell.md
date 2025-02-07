---
author: baanders
description: 'Includedatei für Azure Digital Twins: Einrichten von Cloud Shell und IoT-Erweiterung'
ms.service: digital-twins
ms.topic: include
ms.date: 7/17/2020
ms.author: baanders
ms.openlocfilehash: 127ce4aceff536ef8ce4d2a988a26070ef06b6ec
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110078404"
---
Gehen Sie wie folgt vor, um Azure Digital Twins in einem geöffneten [Azure Cloud Shell](https://shell.azure.com)-Fenster zu verwenden: Melden Sie sich an, und legen Sie zunächst den Shellkontext für diese Sitzung auf Ihr Abonnement fest. Führen Sie diese Befehle in Ihrer Cloud Shell aus:

```azurecli-interactive
az login
az account set --subscription "<your-Azure-subscription-ID>"
```
> [!TIP]
> Sie können im obigen Befehl anstelle der ID auch Ihren Abonnementnamen verwenden. 

Wenn Sie dieses Abonnement zum ersten Mal mit Azure Digital Twins nutzen, führen Sie diesen Befehl aus, um sich im Namespace von Azure Digital Twins zu registrieren. (Falls Sie sich nicht sicher sind, ist es in Ordnung, ihn erneut auszuführen, auch wenn Sie ihn irgendwann zuvor schon einmal ausgeführt haben.)

```azurecli-interactive
az provider register --namespace 'Microsoft.DigitalTwins'
```

Als Nächstes fügen Sie die [Microsoft Azure IoT-Erweiterung für Azure CLI](/cli/azure/service-page/azure%20iot?view=azure-cli-latest&preserve-view=true) zu Ihrer Cloud Shell hinzu, um Befehle für die Interaktion mit Azure Digital Twins und anderen IoT-Diensten zu aktivieren. Führen Sie den folgenden Befehl aus, um sicherzustellen, dass Sie über die neueste Version der Erweiterung verfügen:

```azurecli-interactive
az extension add --upgrade --name azure-iot
```

Jetzt können Sie in der Cloud Shell mit Azure Digital Twins arbeiten.

Sie können dies durch Ausführung von `az dt --help` jederzeit überprüfen, um eine Liste der verfügbaren Azure Digital Twins-Befehle oberster Ebene anzuzeigen.