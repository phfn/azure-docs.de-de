---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/14/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 7116a9b4f585dcfcaa62c447be57376ac0f5c62b
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110074517"
---
|Name<br /><sub>(Azure-Portal)</sub> |BESCHREIBUNG |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Azure Media Services-Aufträge mit HTTPS-Eingaben sollten Eingabe-URIs auf zulässige URI-Muster begrenzen](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe9914afe-31cd-4b8a-92fa-c887f847d477) |Beschränken Sie HTTPS-Eingaben, die von Media Services-Aufträgen verwendet werden, auf bekannte Endpunkte. Eingaben von HTTPS-Endpunkten können vollständig deaktiviert werden, indem eine leere Liste mit zulässigen Auftragseingabemustern festgelegt wird. Wenn in Auftragseingaben ein „baseUri“ angegeben wird, werden die Muster mit diesem Wert abgeglichen. Wenn „baseUri“ nicht festgelegt ist, wird das Muster mit der „files“-Eigenschaft abgeglichen. |Verweigern, deaktiviert |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Media%20Services/Jobs_RestrictHttpInputs.json) |
