---
title: 'Schnellstart: Bereitstellen von Azure API for FHIR mithilfe von PowerShell'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie Azure API for FHIR mithilfe von PowerShell bereitstellen.
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: quickstart
ms.date: 10/15/2019
ms.author: cavoeg
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 896330d15301ae4376744e0e4788c2a7b9ce662c
ms.sourcegitcommit: 20acb9ad4700559ca0d98c7c622770a0499dd7ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2021
ms.locfileid: "110700457"
---
# <a name="quickstart-deploy-azure-api-for-fhir-using-powershell"></a>Schnellstart: Bereitstellen von Azure API for FHIR mithilfe von PowerShell

In dieser Schnellstartanleitung erfahren Sie, wie Sie Azure API for FHIR mithilfe von PowerShell bereitstellen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="register-the-azure-api-for-fhir-resource-provider"></a>Registrieren des Azure API for FHIR-Ressourcenanbieters

Falls der Ressourcenanbieter `Microsoft.HealthcareApis` nicht bereits für Ihr Abonnement registriert ist, können Sie ihn wie folgt registrieren:

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.HealthcareApis
```

## <a name="create-azure-resource-group"></a>Erstellen einer Azure-Ressourcengruppe

```azurepowershell-interactive
New-AzResourceGroup -Name "myResourceGroupName" -Location westus2
```

## <a name="deploy-azure-api-for-fhir"></a>Bereitstellen von Azure API for FHIR

```azurepowershell-interactive
New-AzHealthcareApisService -Name nameoffhirservice -ResourceGroupName myResourceGroupName -Location westus2 -Kind fhir-R4
```

> [!NOTE]
> Abhängig von der von Ihnen installierten Version des `Az`-PowerShell-Moduls kann der bereitgestellte FHIR-Server für die Verwendung der [lokalen RBAC](configure-local-rbac.md) konfiguriert werden, und in der Liste der zulässigen Identitätsobjekt-IDs für den bereitgestellten FHIR-Dienst kann der derzeit angemeldete PowerShell-Benutzer festgelegt sein. Es wird empfohlen, künftig zum Zuweisen von Datenebenenrollen die [Azure RBAC zu verwenden](configure-azure-rbac.md). Unter Umständen müssen Sie diese Benutzerobjekt-ID nach der Bereitstellung löschen, um den Azure RBAC-Modus zu aktivieren.


## <a name="fetch-capability-statement"></a>Abrufen der Funktionsbestätigung

Sie können überprüfen, ob das Azure API for FHIR-Konto ausgeführt wird, indem Sie eine FHIR-Funktionsbestätigung abrufen:

```azurepowershell-interactive
$metadata = Invoke-WebRequest -Uri "https://nameoffhirservice.azurehealthcareapis.com/metadata"
$metadata.RawContent
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie diese Anwendung nicht mehr benötigen, löschen Sie die Ressourcengruppe wie folgt:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroupName
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie Azure API for FHIR in Ihrem Abonnement bereitgestellt. Wenn Sie zusätzliche Einstellungen in Azure API for FHIR festlegen möchten, fahren Sie mit der Schrittanleitung für zusätzliche Einstellungen fort. Wenn Sie Azure API for FHIR verwenden möchten, informieren Sie sich ausführlicher über das Registrieren von Anwendungen.

>[!div class="nextstepaction"]
>[Zusätzliche Einstellungen in Azure API for FHIR](azure-api-for-fhir-additional-settings.md)

>[!div class="nextstepaction"]
>[Registrieren der Azure Active Directory-Apps für Azure API for FHIR](fhir-app-registration.md)
