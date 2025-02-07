---
title: Vergleichsbeispiele für die Migration von Media Services V2 zu V3
description: Hier finden Sie einige Beispiele, die Ihnen helfen, Codeunterschiede zwischen Azure Media Services V2 und V3 zu vergleichen.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: conceptual
ms.workload: media
ms.date: 03/25/2021
ms.author: inhenkel
ms.openlocfilehash: feb2c83ee7edc3ab22b7b8031e6eb07ef65f9908
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105558940"
---
# <a name="media-services-migration-code-sample-comparison"></a>Vergleich von Codebeispielen für die Media Services-Migration

![Logo des Migrationsleitfadens](./media/migration-guide/azure-media-services-logo-migration-guide.svg)

<hr color="#5ea0ef" size="10">

## <a name="compare-the-sdks"></a>Vergleich der SDKs

Sie können einige unserer Codebeispiele verwenden, um die Unterschiede bei den Verfahren zwischen den einzelnen SDKs zu vergleichen.

## <a name="samples-for-comparison"></a>Beispiele für den Vergleich

In der folgenden Tabelle finden Sie Beispiele für den Vergleich zwischen V2 und V3 für gängige Szenarien.

|Szenario|v2-API|v3-API|
|---|---|---|
|Erstellen eines Medienobjekts und Hochladen einer Datei |[Beispiel für v2-.NET](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-aes/blob/master/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs#L113)|[Beispiel für v3-.NET](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#L169)|
|Übermitteln eines Auftrags|[Beispiel für v2-.NET](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-aes/blob/master/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs#L146)|[Beispiel für v3-.NET](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#L298)<br/><br/>Zeigt, wie zuerst eine Transformation erstellt und dann ein Auftrag übermittelt wird.|
|Veröffentlichen eines Medienobjekts mit AES-Verschlüsselung |1. Erstellen Sie `ContentKeyAuthorizationPolicyOption`.<br/>2. Erstellen Sie `ContentKeyAuthorizationPolicy`.<br/>3. Erstellen Sie `AssetDeliveryPolicy`.<br/>4. Erstellen Sie ein `Asset`, und laden Sie Inhalte hoch, ODER übermitteln Sie einen `Job`, und verwenden Sie `OutputAsset`.<br/>5. Ordnen Sie `AssetDeliveryPolicy` `Asset` zu.<br/>6. Erstellen Sie `ContentKey`.<br/>7. Fügen Sie `ContentKey` an `Asset` an.<br/>8. Erstellen Sie `AccessPolicy`.<br/>9. Erstellen Sie `Locator`.<br/><br/>[Beispiel für v2-.NET](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-aes/blob/master/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs#L64)|1. Erstellen Sie `ContentKeyPolicy`.<br/>2. Erstellen Sie `Asset`.<br/>3. Laden Sie Inhalte hoch, oder verwenden Sie `Asset` als `JobOutput`.<br/>4. Erstellen Sie `StreamingLocator`.<br/><br/>[Beispiel für v3-.NET](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES/Program.cs#L105)|
|Abrufen von Auftragsdetails und Verwalten von Aufträgen |[Verwalten von Aufträgen mit v2](../previous/media-services-dotnet-manage-entities.md#get-a-job-reference) |[Verwalten von Aufträgen mit v3](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#L546)|
