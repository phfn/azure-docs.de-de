---
title: Verwaltete Identität für Data Factory
description: Erfahren Sie mehr über die verwaltete Identität für Azure Data Factory.
author: nabhishek
ms.service: data-factory
ms.topic: conceptual
ms.date: 03/25/2021
ms.author: abnarain
ms.openlocfilehash: 632c5930fdae7a4a7504d57b9cb8f285efad8d0a
ms.sourcegitcommit: 1fbd591a67e6422edb6de8fc901ac7063172f49e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2021
ms.locfileid: "109481659"
---
# <a name="managed-identity-for-data-factory"></a>Verwaltete Identität für Data Factory

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

In diesem Artikel erfahren Sie, was eine verwaltete Identität für Data Factory (zuvor „Verwaltete Dienstidentität“ genannt) ist und wie diese funktioniert.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="overview"></a>Übersicht

Wenn Sie eine Data Factory erstellen, kann beim Erstellen der Factory auch eine verwaltete Identität erstellt werden. Die verwaltete Identität ist eine verwaltete Anwendung, die in Azure Active Directory registriert ist und diese spezifische Data Factory darstellt.

Die verwaltete Identität für Data Factory profitiert von den folgenden Features:

- [Speichern von Anmeldeinformationen in Azure Key Vault](store-credentials-in-key-vault.md): In diesem Fall wird die verwaltete Identität für Data Factory für die Azure Key Vault-Authentifizierung verwendet.
- Greifen Sie mit verwalteter Identitätsauthentifizierung auf Datenspeicher oder Berechnungen zu, einschließlich Azure Blob Storage, Azure Data Explorer, Azure Data Lake Storage Gen1, Azure Data Lake Storage Gen2, Azure SQL Database, Azure SQL Managed Instance, Azure Synapse Analytics, REST, Databricks-Aktivität, Web-Aktivität und mehr. Überprüfen Sie die Anschluss- und Aktivitätsartikel für Details.

## <a name="generate-managed-identity"></a>Generieren einer verwalteten Identität

Die verwaltete Identität für Data Factory wird wie folgt generiert:

- Beim Erstellen einer Data Factory über das **Azure-Portal oder PowerShell** wird die verwaltete Identität stets automatisch erstellt.
- Wenn Sie eine Data Factory über das **SDK** erstellen, wird die verwaltete Identität nur erstellt, wenn Sie im Factory-Objekt für die Erstellung „Identity = new FactoryIdentity()“ angeben. Sehen Sie sich das Beispiel unter [.NET-Schnellstart – Erstellen einer Data Factory](quickstart-create-data-factory-dot-net.md#create-a-data-factory) an.
- Beim Erstellen einer Data Factory mithilfe der **REST-API** wird die verwaltete Identität nur erstellt, wenn Sie im Anforderungstext den Abschnitt „identity“ angeben. Sehen Sie sich das Beispiel unter [REST-Schnellstart – Erstellen einer Data Factory](quickstart-create-data-factory-rest-api.md#create-a-data-factory) an.

Sollte Ihrer Data Factory nach dem Schritt [Abrufen einer verwalteten Identität](#retrieve-managed-identity) keine verwaltete Identität zugeordnet sein, können Sie diese explizit durch das programmgesteuerte Aktualisieren der Data Factory mit dem Identitätsinitiator erstellen:

- [Generieren der verwalteten Identität mit PowerShell](#generate-managed-identity-using-powershell)
- [Generieren der verwalteten Identität mit der REST-API](#generate-managed-identity-using-rest-api)
- [Generieren der verwalteten Identität mithilfe einer Azure Resource Manager-Vorlage](#generate-managed-identity-using-an-azure-resource-manager-template)
- [Generieren einer verwalteten Identität mit dem SDK](#generate-managed-identity-using-sdk)

>[!NOTE]
>- Die verwaltete Identität kann nicht geändert werden. Die Aktualisierung einer Data Factory, die bereits über eine verwaltete Identität verfügt, hat keine Auswirkungen. Die verwaltete Identität bleibt unverändert.
>- Wenn Sie eine Data Factory, die bereits über eine verwaltete Identität verfügt, aktualisieren, ohne den Parameter „identity“ im Factory-Objekt oder den Abschnitt „identity“ im REST-Anforderungstext anzugeben, erhalten Sie einen Fehler.
>- Wenn Sie eine Data Factory löschen, wird die zugehörige verwaltete Identität ebenfalls gelöscht.

### <a name="generate-managed-identity-using-powershell"></a>Generieren der verwalteten Identität mit PowerShell

Rufen Sie den Befehl **Set-AzDataFactoryV2** auf, woraufhin die „Identity“-Felder neu generiert werden:

```powershell
PS C:\WINDOWS\system32> Set-AzDataFactoryV2 -ResourceGroupName <resourceGroupName> -Name <dataFactoryName> -Location <region>

DataFactoryName   : ADFV2DemoFactory
DataFactoryId     : /subscriptions/<subsID>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/ADFV2DemoFactory
ResourceGroupName : <resourceGroupName>
Location          : East US
Tags              : {}
Identity          : Microsoft.Azure.Management.DataFactory.Models.FactoryIdentity
ProvisioningState : Succeeded
```

### <a name="generate-managed-identity-using-rest-api"></a>Generieren der verwalteten Identität mit der REST-API

Rufen Sie die unten angegebene API mit dem Abschnitt „identity“ im Anforderungstext auf:

```
PATCH https://management.azure.com/subscriptions/<subsID>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/<data factory name>?api-version=2018-06-01
```

**Anforderungstext**: Fügen Sie Folgendes hinzu: „identity": { "type": "SystemAssigned" }.

```json
{
    "name": "<dataFactoryName>",
    "location": "<region>",
    "properties": {},
    "identity": {
        "type": "SystemAssigned"
    }
}
```

**Antwort:** Die verwaltete Identität wird automatisch erstellt, und der Abschnitt „identity“ wird entsprechend aufgefüllt.

```json
{
    "name": "<dataFactoryName>",
    "tags": {},
    "properties": {
        "provisioningState": "Succeeded",
        "loggingStorageAccountKey": "**********",
        "createTime": "2017-09-26T04:10:01.1135678Z",
        "version": "2018-06-01"
    },
    "identity": {
        "type": "SystemAssigned",
        "principalId": "765ad4ab-XXXX-XXXX-XXXX-51ed985819dc",
        "tenantId": "72f988bf-XXXX-XXXX-XXXX-2d7cd011db47"
    },
    "id": "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/ADFV2DemoFactory",
    "type": "Microsoft.DataFactory/factories",
    "location": "<region>"
}
```

### <a name="generate-managed-identity-using-an-azure-resource-manager-template"></a>Generieren der verwalteten Identität mithilfe einer Azure Resource Manager-Vorlage

**Vorlage**: Fügen Sie Folgendes hinzu: "identity": { "type": "SystemAssigned" }.

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "resources": [{
        "name": "<dataFactoryName>",
        "apiVersion": "2018-06-01",
        "type": "Microsoft.DataFactory/factories",
        "location": "<region>",
        "identity": {
            "type": "SystemAssigned"
        }
    }]
}
```

### <a name="generate-managed-identity-using-sdk"></a>Generieren einer verwalteten Identität mit dem SDK

Rufen Sie die Data Factory-Funktion zum Erstellen oder Aktualisieren mit „Identity=new FactoryIdentity()“ auf. Beispielcode unter Verwendung von .NET:

```csharp
Factory dataFactory = new Factory
{
    Location = <region>,
    Identity = new FactoryIdentity()
};
client.Factories.CreateOrUpdate(resourceGroup, dataFactoryName, dataFactory);
```

## <a name="retrieve-managed-identity"></a>Abrufen der verwalteten Identität

Sie können die verwaltete Identität über das Azure-Portal oder programmgesteuert abrufen. Die folgenden Abschnitte enthalten einige Beispiele.

>[!TIP]
> Wenn die verwaltete Identität nicht angezeigt wird, können Sie durch Aktualisieren der Factory die [verwaltete Identität generieren](#generate-managed-identity).

### <a name="retrieve-managed-identity-using-azure-portal"></a>Abrufen der verwalteten Identität über das Azure-Portal

Sie finden die Informationen zur verwalteten Identität im Azure-Portal unter „Ihre Data Factory“ > „Eigenschaften“.

- Objekt-ID der verwalteten Identität
- Mandant der verwalteten Identität

Die Informationen zur verwalteten Identität werden auch angezeigt, wenn Sie einen verknüpften Dienst erstellen, der die Authentifizierung der verwalteten Identität unterstützt, z. B. Azure-Blob, Azure Data Lake Storage, Azure Key Vault usw.

Wenn Sie eine Berechtigung erteilen, wählen Sie in der Azure-Ressource auf der Registerkarte Zugriffskontrolle (IAM) -> Rollenzuweisung hinzufügen -> Zugriff zuweisen zu -> wählen Sie Data Factory unter System zugewiesene verwaltete Identität -> wählen Sie nach Fabrikname; oder im Allgemeinen können Sie die Objekt-ID oder den Datenfabriknamen (als verwalteten Identitätsnamen) verwenden, um diese Identität zu finden. Wenn Sie die Anwendungs-ID der verwalteten Identität abrufen müssen, können Sie PowerShell verwenden.

### <a name="retrieve-managed-identity-using-powershell"></a>Abrufen der verwalteten Identität mit PowerShell

Die Prinzipal-ID und Mandanten-ID der verwalteten Identität werden folgendermaßen zurückgegeben, wenn Sie eine bestimmte Data Factory abrufen: Verwenden Sie die **PrincipalId**, um Zugriff zu gewähren:

```powershell
PS C:\WINDOWS\system32> (Get-AzDataFactoryV2 -ResourceGroupName <resourceGroupName> -Name <dataFactoryName>).Identity

PrincipalId                          TenantId
-----------                          --------
765ad4ab-XXXX-XXXX-XXXX-51ed985819dc 72f988bf-XXXX-XXXX-XXXX-2d7cd011db47
```

Sie können die Anwendungs-ID abrufen, indem Sie die vorstehende Prinzipal-ID kopieren und dann den unten gezeigten Azure Active Directory Befehl mit der Prinzipal-ID als Parameter ausführen.

```powershell
PS C:\WINDOWS\system32> Get-AzADServicePrincipal -ObjectId 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc

ServicePrincipalNames : {76f668b3-XXXX-XXXX-XXXX-1b3348c75e02, https://identity.azure.net/P86P8g6nt1QxfPJx22om8MOooMf/Ag0Qf/nnREppHkU=}
ApplicationId         : 76f668b3-XXXX-XXXX-XXXX-1b3348c75e02
DisplayName           : ADFV2DemoFactory
Id                    : 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc
Type                  : ServicePrincipal
```

### <a name="retrieve-managed-identity-using-rest-api"></a>Abrufen der verwalteten Identität mit der REST-API

Die Prinzipal-ID und Mandanten-ID der verwalteten Identität werden folgendermaßen zurückgegeben, wenn Sie eine bestimmte Data Factory abrufen:

Rufen Sie die folgende API in der Anforderung auf:

```
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DataFactory/factories/{factoryName}?api-version=2018-06-01
```

**Antwort**: Sie erhalten eine Antwort wie im nachfolgenden Beispiel gezeigt. Der Abschnitt für die Identität ist entsprechend ausgefüllt.

```json
{
    "name":"<dataFactoryName>",
    "identity":{
        "type":"SystemAssigned",
        "principalId":"554cff9e-XXXX-XXXX-XXXX-90c7d9ff2ead",
        "tenantId":"72f988bf-XXXX-XXXX-XXXX-2d7cd011db47"
    },
    "id":"/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/<dataFactoryName>",
    "type":"Microsoft.DataFactory/factories",
    "properties":{
        "provisioningState":"Succeeded",
        "createTime":"2020-02-12T02:22:50.2384387Z",
        "version":"2018-06-01",
        "factoryStatistics":{
            "totalResourceCount":0,
            "maxAllowedResourceCount":0,
            "factorySizeInGbUnits":0,
            "maxAllowedFactorySizeInGbUnits":0
        }
    },
    "eTag":"\"03006b40-XXXX-XXXX-XXXX-5e43617a0000\"",
    "location":"<region>",
    "tags":{

    }
}
```

> [!TIP] 
> Fügen Sie zum Abrufen der verwalteten Identität aus einer ARM-Vorlage einen Abschnitt **outputs** im ARM JSON-Code hinzu:

```json
{
    "outputs":{
        "managedIdentityObjectId":{
            "type":"string",
            "value":"[reference(resourceId('Microsoft.DataFactory/factories', parameters('<dataFactoryName>')), '2018-06-01', 'Full').identity.principalId]"
        }
    }
}
```

## <a name="next-steps"></a>Nächste Schritte
Lesen Sie die folgenden Themen, die Ihnen zeigen, wann und wie Sie die verwaltete Identität für Data Factory verwenden können:

- [Speichern von Anmeldeinformationen in Azure Key Vault](store-credentials-in-key-vault.md)
- [Kopieren von Daten von/zu Azure Data Lake Store mit verwalteten Identitäten für die Ressourcenauthentifizierung mit Azure](connector-azure-data-lake-store.md)

Weitere Hintergründe zu verwalteten Identitäten für Azure-Ressourcen (auf denen verwaltete Identitäten für Data Factory basieren) finden Sie unter [Verwaltete Identitäten für Azure-Ressourcen – Übersicht](../active-directory/managed-identities-azure-resources/overview.md).