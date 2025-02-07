---
title: Integration von Azure Security Center mit PowerShell
description: In diesem Dokument wird der Vorgang zum Aktivieren von Azure Security Center mithilfe von PowerShell-Cmdlets erläutert.
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: quickstart
ms.date: 04/19/2021
ms.author: memildin
ms.openlocfilehash: f9ab258f59279112d0b90c5d460e6761ac911a2d
ms.sourcegitcommit: 3ed0f0b1b66a741399dc59df2285546c66d1df38
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2021
ms.locfileid: "107713352"
---
# <a name="automate-onboarding-of-azure-security-center-using-powershell"></a>Automatisieren der Integration von Azure Security Center mithilfe von PowerShell

Das Azure Security Center-PowerShell-Modul ermöglicht den programmgesteuerten Schutz Ihrer Azure-Workloads.
Mithilfe von PowerShell können Sie Tasks automatisieren und menschliche Fehler bei manuellen Aufgaben vermeiden. Dies ist vor allem bei umfangreichen Bereitstellungen mit Dutzenden Abonnements und Hunderttausenden Ressourcen praktisch, die alle von Anfang an geschützt werden müssen.

Integrieren Sie Azure Security Center mithilfe von PowerShell, um die Integration und Verwaltung Ihrer Azure-Ressourcen programmgesteuert zu automatisieren und die erforderlichen Sicherheitskontrollen hinzuzufügen.

Dieser Artikel enthält ein PowerShell-Beispielskript, das zur abonnementübergreifenden Einführung von Security Center angepasst und verwendet werden kann. 

In diesem Beispiel aktivieren Sie Security Center für ein Abonnement mit der ID d07c0080-170c-4c24-861d-9c817742786c und wenden die empfohlenen Einstellungen für hohen Schutz an. Dazu aktivieren Sie Azure Defender, der erweiterte Funktionen für Bedrohungsschutz und Erkennung bietet:

1. Aktivieren Sie [Azure Defender](azure-defender.md). 
 
2. Legen Sie den Log Analytics-Arbeitsbereich fest, an den der Log Analytics-Agent die Daten sendet, die auf den mit dem Abonnement verknüpften virtuellen Computern erfasst werden. In diesem Beispiel wird ein vorhandener benutzerdefinierter Arbeitsbereich (myWorkspace) verwendet.

3. Aktivieren Sie die automatische Agent-Bereitstellung von Security Center, bei der auch der [Log Analytics-Agent bereitgestellt](security-center-enable-data-collection.md#auto-provision-mma) wird.

5. Legen Sie den [CISO der Organisation als Sicherheitskontakt für Security Center-Warnungen und relevante Ereignisse](security-center-provide-security-contact-details.md) fest.

6. Weisen Sie die [Standardsicherheitsrichtlinien](tutorial-security-policy.md) von Security Center zu.

## <a name="prerequisites"></a>Voraussetzungen

Diese Schritte sollten vor der Ausführung der Security Center-Cmdlets ausgeführt werden:

1. Führen Sie PowerShell als Administrator aus.

1. Führen Sie die folgenden Befehle in PowerShell aus:
      
    ```Set-ExecutionPolicy -ExecutionPolicy AllSigned```

    ```Install-Module -Name Az.Security -Force```

## <a name="onboard-security-center-using-powershell"></a>Integrieren von Security Center mithilfe von PowerShell

1. Registrieren Sie Ihre Abonnements beim Security Center-Ressourcenanbieter:

    ```Set-AzContext -Subscription "d07c0080-170c-4c24-861d-9c817742786c"```

    ```Register-AzResourceProvider -ProviderNamespace 'Microsoft.Security'```

1. Optional: Legen Sie die Abdeckungsebene (Azure Defender ein/aus) der Abonnements fest. Wenn keine definiert, ist Defender deaktiviert:

    ```Set-AzContext -Subscription "d07c0080-170c-4c24-861d-9c817742786c"```

    ```Set-AzSecurityPricing -Name "default" -PricingTier "Standard"```

1. Konfigurieren Sie einen Log Analytics-Arbeitsbereich, an den die Agents Daten melden. Sie benötigen einen bereits erstellten Log Analytics-Arbeitsbereich, an den die virtuellen Computer im Abonnement Daten melden können. Sie können mehrere Abonnements festlegen, die an den gleichen Arbeitsbereich Daten melden. Wird keine Angabe gemacht, wird der Standardarbeitsbereich verwendet.

    ```Set-AzSecurityWorkspaceSetting -Name "default" -Scope "/subscriptions/d07c0080-170c-4c24-861d-9c817742786c" -WorkspaceId"/subscriptions/d07c0080-170c-4c24-861d-9c817742786c/resourceGroups/myRg/providers/Microsoft.OperationalInsights/workspaces/myWorkspace"```

1. Installation mit automatischer Bereitstellung des Log Analytics-Agents auf Ihren virtuellen Azure-Computern:
    
    ```Set-AzContext -Subscription "d07c0080-170c-4c24-861d-9c817742786c"```
    
    ```Set-AzSecurityAutoProvisioningSetting -Name "default" -EnableAutoProvision```

    > [!NOTE]
    > Sie sollten die automatische Bereitstellung aktivieren, um sicherzustellen, dass Ihre virtuellen Azure-Computer automatisch mit Azure Security Center geschützt werden.
    >

1. Optional: Es wird dringend empfohlen, die [Sicherheitskontaktinformationen](security-center-provide-security-contact-details.md) für die zu integrierenden Abonnements anzugeben. Die Sicherheitskontakte erhalten die von Security Center generierten Warnungen und Benachrichtigungen:

    ```Set-AzSecurityContact -Name "default1" -Email "CISO@my-org.com" -AlertAdmin -NotifyOnAlert```

1. Weisen Sie die Security Center-Standardrichtlinieninitiative zu:

    ```Register-AzResourceProvider -ProviderNamespace 'Microsoft.PolicyInsights'```

    ```$Policy = Get-AzPolicySetDefinition | where {$_.Properties.displayName -EQ 'Azure Security Benchmark'} New-AzPolicyAssignment -Name 'ASC Default <d07c0080-170c-4c24-861d-9c817742786c>' -DisplayName 'Security Center Default <subscription ID>' -PolicySetDefinition $Policy -Scope '/subscriptions/d07c0080-170c-4c24-861d-9c817742786c'```

Sie haben Azure Security Center mithilfe von PowerShell erfolgreich integriert.

Diese PowerShell-Cmdlets können nun mit Automatisierungsskripts verwendet werden, um Abonnements und Ressourcen programmgesteuert zu durchlaufen. Das spart Zeit und trägt dazu bei, menschliche Fehler zu vermeiden. Sie können dieses [Beispielskript](https://github.com/Microsoft/Azure-Security-Center/blob/master/quickstarts/ASC-Samples.ps1) als Referenz verwenden.




## <a name="see-also"></a>Weitere Informationen
Weitere Informationen zum Automatisieren der Integration in Security Center mithilfe von PowerShell finden Sie im folgenden Artikel:

* [Az.Security](/powershell/module/az.security)

Weitere Informationen zu Security Center finden Sie in den folgenden Artikeln:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](tutorial-security-policy.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md) : Hier erfahren Sie, wie Sie Sicherheitswarnungen verwalten und auf diese reagieren.