---
title: 'Azure ExpressRoute: ARP-Tabellen – Problembehandlung'
description: Diese Seite enthält Anweisungen zum Abrufen der ARP-Tabellen (Address Resolution Protocol) für eine ExpressRoute-Verbindung.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: troubleshooting
ms.date: 12/15/2020
ms.author: duau
ms.custom: seodec18
ms.openlocfilehash: 7d8ae2c58979c66ebbbab366d172179bdeee4253
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "97561578"
---
# <a name="getting-arp-tables-in-the-resource-manager-deployment-model"></a>Abrufen von ARP-Tabellen im Resource Manager-Bereitstellungsmodell
> [!div class="op_single_selector"]
> * [PowerShell – Resource Manager](expressroute-troubleshooting-arp-resource-manager.md)
> * [PowerShell – klassisch](expressroute-troubleshooting-arp-classic.md)
> 
> 

In diesem Artikel werden Sie durch die Schritte zum Erlernen der Nutzung der ARP-Tabellen für Ihre ExpressRoute-Verbindung geführt.

> [!IMPORTANT]
> Dieses Dokument soll Ihnen helfen, einfache Probleme zu untersuchen und zu beheben. Es dient nicht als Ersatz für Microsoft Support. Sie müssen ein Supportticket beim [Microsoft-Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) öffnen, wenn Sie Ihr Problem nicht anhand der folgenden Anleitung beheben können.
> 
> 

[!INCLUDE [updated-for-az](../../includes/hybrid-az-ps.md)]

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Address Resolution Protocol (ARP) und ARP-Tabellen
Address Resolution Protocol (ARP) ist ein in [RFC 826](https://tools.ietf.org/html/rfc826)definiertes Schicht-2-Protokoll. ARP dient zum Zuordnen der Ethernet-Adresse (MAC-Adresse) zu einer IP-Adresse.

Die ARP-Tabelle enthält die folgenden Informationen sowohl für die primäre als auch für die sekundäre Schnittstelle für jeden Peeringtyp:

1. Zuordnung der IP-Adresse der lokalen Routerschnittstelle zur MAC-Adresse
2. Zuordnung der IP-Adresse der ExpressRoute-Routerschnittstelle zur MAC-Adresse
3. Alter der Zuordnung

ARP-Tabellen dienen zum Überprüfen der Layer-2-Konfiguration und Behandeln grundlegender Layer-2-Verbindungsprobleme. 

Beispiel einer ARP-Tabelle: 

```output
Age InterfaceProperty IpAddress  MacAddress    
--- ----------------- ---------  ----------    
 10 On-Prem           10.0.0.1   ffff.eeee.dddd
  0 Microsoft         10.0.0.2   aaaa.bbbb.cccc
```


Der folgende Abschnitt bietet Informationen zum Anzeigen der ARP-Tabellen, die von ExpressRoute-Edge-Routern verwendet werden. 

## <a name="prerequisites-for-learning-arp-tables"></a>Voraussetzungen für das Erlernen von ARP-Tabellen
Vergewissern Sie sich, dass die folgenden Informationen zutreffen, bevor Sie fortfahren:

* Eine gültige ExpressRoute-Verbindung, die mit mindestens einem Peering konfiguriert ist. Die Verbindung muss vom Konnektivitätsanbieter vollständig konfiguriert werden. Von Ihnen oder Ihrem Konnektivitätsanbieter muss mindestens eines der Peerings (Azure privat, Azure öffentlich und Microsoft) für diese Verbindung konfiguriert werden.
* IP-Adressbereiche, die zur Konfiguration der Peerings verwendet werden. Sehen Sie sich die Beispiele für die Zuweisung von IP-Adressen auf der Seite [ExpressRoute-Routinganforderungen](expressroute-routing.md) an, um zu verstehen, wie IP-Adressen den Schnittstellen zugeordnet werden. Auf der Seite [Erstellen und Ändern des Routings für eine ExpressRoute-Verbindung](expressroute-howto-routing-arm.md)erhalten Sie Informationen zur Konfiguration des Peerings.
* Informationen von Ihrem Netzwerkteam/Konnektivitätsanbieter zu den MAC-Adressen von Schnittstellen, die mit diesen IP-Adressen verwendet werden.
* Sie benötigen das neueste PowerShell-Modul für Azure (mindestens Version 1.50).

> [!NOTE]
> Wenn vom Dienstanbieter Layer 3 bereitgestellt wird und die ARP-Tabellen im Portal bzw. in der Ausgabe unten leer sind, aktualisieren Sie die Verbindungskonfiguration über die Schaltfläche „Aktualisieren“ im Portal. Mit diesem Vorgang wird für Ihre Verbindung die richtige Routingkonfiguration angewendet. 
>
>

## <a name="getting-the-arp-tables-for-your-expressroute-circuit"></a>Abrufen der ARP-Tabellen für Ihre ExpressRoute-Verbindung
Dieser Abschnitt bietet Anweisungen zum Anzeigen der ARP-Tabellen pro Peering mithilfe von PowerShell. Von Ihnen oder Ihrem Konnektivitätsanbieter muss das Peering konfiguriert worden sein, ehe Sie fortfahren können. Jede Verbindung hat einen primären und sekundären Pfad. Sie können die ARP-Tabelle für jeden Pfad unabhängig überprüfen.

### <a name="arp-tables-for-azure-private-peering"></a>IP-Adressen für privates Azure-Peering
Das folgende Cmdlet dient zum Abrufen der ARP-Tabellen für privates Azure-Peering.

```azurepowershell
# Required Variables
$RG = "<Your Resource Group Name Here>"
$Name = "<Your ExpressRoute Circuit Name Here>"

# ARP table for Azure private peering - Primary path
Get-AzExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Primary

# ARP table for Azure private peering - Secondary path
Get-AzExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Secondary 
```

Nachstehend sehen Sie die Beispielausgabe für einen der Pfade.

```output
Age InterfaceProperty IpAddress  MacAddress    
--- ----------------- ---------  ----------    
 10 On-Prem           10.0.0.1   ffff.eeee.dddd
  0 Microsoft         10.0.0.2   aaaa.bbbb.cccc
```


### <a name="arp-tables-for-azure-public-peering"></a>ARP-Tabellen für öffentliches Azure-Peering
Das folgende Cmdlet dient zum Abrufen der ARP-Tabellen für öffentliches Azure-Peering.

```azurepowershell
# Required Variables
$RG = "<Your Resource Group Name Here>"
$Name = "<Your ExpressRoute Circuit Name Here>"

# ARP table for Azure public peering - Primary path
Get-AzExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Primary

# ARP table for Azure public peering - Secondary path
Get-AzExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Secondary 
```


Nachstehend sehen Sie die Beispielausgabe für einen der Pfade.

```output
Age InterfaceProperty IpAddress  MacAddress    
--- ----------------- ---------  ----------    
 10 On-Prem           64.0.0.1   ffff.eeee.dddd
  0 Microsoft         64.0.0.2   aaaa.bbbb.cccc
```


### <a name="arp-tables-for-microsoft-peering"></a>ARP-Tabellen für Microsoft-Peering
Das folgende Cmdlet dient zum Abrufen der ARP-Tabellen für Microsoft-Peering.

```azurepowershell
# Required Variables
$RG = "<Your Resource Group Name Here>"
$Name = "<Your ExpressRoute Circuit Name Here>"

# ARP table for Microsoft peering - Primary path
Get-AzExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Primary

# ARP table for Microsoft peering - Secondary path
Get-AzExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Secondary 
```


Nachstehend sehen Sie die Beispielausgabe für einen der Pfade.

```output
Age InterfaceProperty IpAddress  MacAddress    
--- ----------------- ---------  ----------    
 10 On-Prem           65.0.0.1   ffff.eeee.dddd
  0 Microsoft         65.0.0.2   aaaa.bbbb.cccc
```


## <a name="how-to-use-this-information"></a>Verwenden dieser Informationen
Die ARP-Tabelle eines Peerings dient zum Bestimmen und Überprüfen der Layer-2-Konfiguration und -Konnektivität. Dieser Abschnitt enthält eine Übersicht über ARP-Tabellen in verschiedenen Szenarien.

### <a name="arp-table-when-a-circuit-is-in-operational-state-expected-state"></a>ARP-Tabelle einer Verbindung im Betriebszustand (d.h. dem erwarteten Zustand)
* Die ARP-Tabelle enthält einen Eintrag für die lokale Seite mit einer gültigen IP-Adresse und MAC-Adresse. Das gleiche gilt für die Microsoft-Seite. 
* Das letzte Oktett der lokalen IP-Adresse ist immer eine ungerade Zahl.
* Das letzte Oktett der IP-Adresse von Microsoft ist immer eine gerade Zahl.
* Dieselbe MAC-Adresse wird auf Microsoft-Seite für alle drei Peerings (primär/sekundär) angezeigt. 

```output
Age InterfaceProperty IpAddress  MacAddress    
--- ----------------- ---------  ----------    
 10 On-Prem           65.0.0.1   ffff.eeee.dddd
  0 Microsoft         65.0.0.2   aaaa.bbbb.cccc
```

### <a name="arp-table-when-on-premises--connectivity-provider-side-has-problems"></a>ARP-Tabelle, wenn auf der lokalen Seite bzw. beim Konnektivitätsanbieter Probleme auftreten
Wenn ein Problem mit dem lokalen Anbieter oder Konnektivitätsanbieter auftritt, zeigt die ARP-Tabelle eine von zwei Optionen an. Sie sehen entweder die lokale MAC-Adresse unvollständig angezeigt oder nur den Microsoft-Eintrag in der ARP-Tabelle.
  
```output
Age InterfaceProperty IpAddress  MacAddress    
--- ----------------- ---------  ----------   
  0 On-Prem           65.0.0.1   Incomplete
  0 Microsoft         65.0.0.2   aaaa.bbbb.cccc
```
oder
   
```output
Age InterfaceProperty IpAddress  MacAddress    
--- ----------------- ---------  ----------    
  0 Microsoft         65.0.0.2   aaaa.bbbb.cccc
```  

> [!NOTE]
> Öffnen Sie eine Supportanfrage bei Ihrem Konnektivitätsanbieter, um solche Probleme zu beheben. Sehen Sie sich die folgenden Informationen an, wenn für die ARP-Tabelle keine IP-Adressen der Schnittstellen, die MAC-Adressen zugeordnet sind, vorhanden sind:
> 
> 1. Wenn die erste IP-Adresse des /30-Subnetzes, die für den Link zwischen dem MSEE-PR und MSEE zugewiesen ist, für die Schnittstelle des MSEE-PR verwendet wird. Azure verwendet immer die zweite IP-Adresse für MSEEs.
> 2. Überprüfen Sie, ob die VLAN-Tags für den Kunden (C-Tag) und den Dienst (S-Tag) für das MSEE-PR- und das MSEE-Paar übereinstimmen.
> 

### <a name="arp-table-when-microsoft-side-has-problems"></a>ARP-Tabelle, wenn auf der Microsoft-Seite Probleme auftreten
* Wenn auf der Microsoft-Seite Probleme vorliegen, wird für ein Peering keine ARP-Tabelle angezeigt. 
* Öffnen Sie ein Supportticket beim [Microsoft Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Geben Sie an, dass Sie ein Problem mit der Layer-2-Konnektivität haben. 

## <a name="next-steps"></a>Nächste Schritte
* Überprüfen der Layer-3-Konfigurationen Ihrer ExpressRoute-Verbindung
  * Abrufen der Routenübersicht zum Bestimmen des Status von BGP-Sitzungen
  * Abrufen der Routentabelle zum Bestimmen der für ExpressRoute angekündigten Präfixe
* Überprüfen der Datenübertragung anhand der ein- und ausgehenden Bytes
* Öffnen eines Supporttickets beim [Microsoft-Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), wenn weiterhin Probleme auftreten

