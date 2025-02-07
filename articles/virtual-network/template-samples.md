---
title: Azure Resource Manager-Vorlagenbeispiele für virtuelle Netzwerke | Microsoft-Dokumentation
description: Hier finden Sie Informationen zu verschiedenen verfügbaren Azure Resource Manager-Vorlagen, mit denen Sie virtuelle Azure-Netzwerke bereitstellen können.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: mtillman
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 04/22/2019
ms.author: kumud
ms.openlocfilehash: d565e1b6991534a19e9ac9a92d9fe6aed62d520b
ms.sourcegitcommit: 1b19b8d303b3abe4d4d08bfde0fee441159771e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/11/2021
ms.locfileid: "109751507"
---
# <a name="azure-resource-manager-template-samples-for-virtual-network"></a>Azure Resource Manager-Vorlagenbeispiele für virtuelle Netzwerke

Die folgende Tabelle enthält Links zu Azure Resource Manager-Vorlagenbeispielen. Sie können Vorlagen über das [Portal](../azure-resource-manager/templates/deploy-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json), mithilfe der [Azure-Befehlszeilenschnittstelle](../azure-resource-manager/templates/deploy-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) oder mithilfe von [PowerShell](../azure-resource-manager/templates/deploy-powershell.md?toc=%2fazure%2fvirtual-network%2ftoc.json) bereitstellen. Informationen zum Erstellen eigener Vorlagen finden Sie unter [Erstellen und Bereitstellen Ihrer ersten Azure Resource Manager-Vorlage](../azure-resource-manager/templates/quickstart-create-templates-use-the-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sowie unter [Verstehen der Struktur und Syntax von Azure Resource Manager-Vorlagen](../azure-resource-manager/templates/template-syntax.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Informationen zur JSON-Syntax und zu den Eigenschaften, die in Vorlagen verwendet werden sollen, finden Sie unter [Microsoft.Network resource types](/azure/templates/microsoft.network/allversions) (Microsoft.Network-Ressourcentypen).

| Aufgabe | BESCHREIBUNG |
|----|----|
|[Erstellen eines virtuellen Netzwerks mit zwei Subnetzen](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.network/vnet-two-subnets/)| Erstellt ein virtuelles Netzwerk mit zwei Subnetzen.|
|[Weiterleiten von Datenverkehr über ein virtuelles Netzwerkgerät](https://github.com/Azure/azure-quickstart-templates/tree/master/201-userdefined-routes-appliance)| Erstellt ein virtuelles Netzwerk mit drei Subnetzen. Stellt in jedem der Subnetze einen virtuellen Computer bereit. Erstellt eine Routingtabelle mit Routen, um Datenverkehr aus einem Subnetz über den virtuellen Computer im dritten Subnetz an ein anderes Subnetz weiterzuleiten. Ordnet die Routingtabelle einem der Subnetze zu.|
|[Erstellen eines VNET-Dienstendpunkts für Azure Storage](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.network/vnet-2subnets-service-endpoints-storage-integration)|Erstellt ein neues virtuelles Netzwerk mit zwei Subnetzen und jeweils eine Netzwerkschnittstelle in den Subnetzen. Aktiviert einen Dienstendpunkt für Azure Storage für eines der Subnetze und bindet ein neues Speicherkonto an dieses Subnetz.|
|[Verbinden zweier virtueller Netzwerke](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vnet-to-vnet-peering)| Erstellt zwei virtuelle Netzwerke und ein Peering zwischen ihnen.|
|[Erstellen eines virtuellen Computers mit mehreren IP-Adressen](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.compute/vm-multiple-ipconfig)| Erstellt einen virtuellen Windows- oder Linux-Computer mit mehreren IP-Adressen.|
|[Konfigurieren eines virtuellen IPv4 + IPv6 Dual-Stack-Netzwerks](https://github.com/Azure/azure-quickstart-templates/tree/master/demos/ipv6-in-vnet)|Stellt ein virtuelles Dual-Stack-Netzwerk (IPv4 + IPv6) mit zwei virtuellen Computern und einem Azure Load Balancer Basic mit öffentlichen IPv4- und IPv6-Adressen bereit. |