---
title: Metrikdefinitionen für Azure Communication Service
titleSuffix: An Azure Communication Services concept document
description: Dieses Dokument behandelt Definitionen von Metriken, die im Azure-Portal verfügbar sind.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 03/10/2021
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: 129a718175fdda80d4d6852e3d3b4cea609da64d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "103492410"
---
# <a name="metrics-overview"></a>Übersicht der Metriken

Azure Communication Services bietet derzeit Metriken für Chat und SMS. Mit dem [Azure-Metrik-Explorer](../../azure-monitor/essentials/metrics-getting-started.md) können Sie Ihre eigenen Diagramme erstellen, Anomalien in Ihren Metrikwerten untersuchen und Ihren Datenverkehr über die API verstehen, indem Sie die Metrikdaten verwenden, die von Chat- und SMS-Anforderungen ausgegeben werden.

## <a name="where-to-find-metrics"></a>Position von Metriken

Chat- und SMS-Dienste in Azure Communication Services geben Metriken für API-Anforderungen aus. Diese Metriken finden Sie auf dem Blatt „Metriken“ unter Ihrer Communication Services-Ressource. Sie können auch permanente Dashboards mithilfe des Arbeitsmappenblatts unter Ihrer Communication Services-Ressource erstellen.

## <a name="metric-definitions"></a>Metrikdefinitionen

Es gibt zwei Arten von Anforderungen, die in den Communication Services-Metriken dargestellt werden: **Chat-API-Anforderungen** und **SMS-API-Anforderungen**.

Sowohl Chat- als auch SMS-API-Anforderungsmetriken enthalten drei Dimensionen, mit denen Sie Ihre Metrikdaten filtern können. Diese Dimensionen können mithilfe des Aggregationstyps `Count` zusammen aggregiert werden und unterstützen alle Azure Aggregation-Standardzeitreihen, einschließlich `Sum`, `Average`, `Min` und `Max`.

Weitere Informationen zu unterstützten Aggregationstypen und Zeitreihenaggregationen finden Sie unter [Erweiterte Funktionen von Azure-Metrik-Explorer](../../azure-monitor/essentials/metrics-charts.md#aggregation).

- **Operation**: Alle Vorgänge oder Routen, die über das ACS Chat-Gateway aufgerufen werden können.
- **Statuscode**: Die Statuscodeantwort, die nach der Anforderung gesendet wurde.
- **StatusSubClass**: Die nach der Antwort gesendete Statuscodefolge. 


### <a name="chat-api-request-metric-operations"></a>Vorgänge für Chat-API-Anforderungsmetriken

Die folgenden Vorgänge sind für Chat-API-Anforderungsmetriken verfügbar:

| Vorgang/Route    | BESCHREIBUNG                                                                                    |
| -------------------- | ---------------------------------------------------------------------------------------------- |
| GetChatMessage       | Ruft eine Nachricht nach Nachrichten-ID ab. |
| ListChatMessages     | Ruft eine Liste von Chat-Nachrichten aus einem Thread ab. |
| SendChatMessage      | Sendet eine Chat-Nachricht an einen Thread. |
| UpdateChatMessage    | Aktualisiert eine Chat-Nachricht. |
| DeleteChatMessage    | Löscht eine Chat-Nachricht. |
| GetChatThread        | Ruft einen Chatthread ab. |
| ListChatThreads      | Ruft die Liste der Chatthreads eines Benutzers ab. |
| UpdateChatThread     | Aktualisiert die Eigenschaften eines Chatthreads. |
| CreateChatThread     | Erstellt einen Chatthread. |
| DeleteChatThread     | Löscht einen Thread. |
| GetReadReceipts      | Ruft Lesebestätigungen für einen Thread ab. |
| SendReadReceipt      | Sendet im Auftrag eines Benutzers ein Lesebestätigungsereignis an einen Thread. |
| SendTypingIndicator           | Veröffentlicht im Auftrag eines Benutzers ein Eingabeereignis für einen Thread. |
| ListChatThreadParticipants    | Ruft die Mitglieder eines Threads ab. |
| AddChatThreadParticipants     | Fügt Threadmitglieder zu einem Thread hinzu. Wenn bereits Mitglieder vorhanden sind, erfolgt keine Änderung. |
| RemoveChatThreadParticipant   | Entfernt ein Mitglied aus einem Thread. |

:::image type="content" source="./media/chat-metric.png" alt-text="Chat-API-Anforderungsmetrik":::

Wenn eine Anforderung an einen Vorgang gestellt wird, der nicht erkannt wird, erhalten Sie eine Antwort mit dem Wert „Ungültige Route“.

### <a name="sms-api-requests"></a>SMS-API-Anforderungen

Die folgenden Vorgänge sind für SMS-API-Anforderungsmetriken verfügbar:

| Vorgang/Route    | BESCHREIBUNG                                                                                    |
| -------------------- | ---------------------------------------------------------------------------------------------- |
| SMSMessageSent       | Sendet eine SMS-Nachricht. |
| SMSDeliveryReportsReceived     | Ruft SMS-Zustellberichte ab. |
| SMSMessagesReceived      | Ruft SMS-Nachrichten ab. |


:::image type="content" source="./media/sms-metric.png" alt-text="SMS-API-Anforderungsmetrik":::

### <a name="authentication-api-requests"></a>Anforderungen der Authentifizierungs-API

Die folgenden Vorgänge sind für Anforderungsmetriken der Authentifizierungs-API verfügbar:

| Vorgang/Route    | BESCHREIBUNG                                                                                    |
| -------------------- | ---------------------------------------------------------------------------------------------- |
| CreateIdentity       | Erstellt eine Identität, die einen einzelnen Benutzer darstellt. |
| DeleteIdentity       | Löscht eine Identität. |
| CreateToken          | Erstellt ein Zugriffstoken. |
| RevokeToken          | Widerruft alle Zugriffstoken, die vor einem bestimmten Zeitpunkt für eine Identität erstellt wurden. |

:::image type="content" source="./media/acs-auth-metrics.png" alt-text="Authentifizierungsanforderungs-Metrik.":::

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Datenplattformmetriken](../../azure-monitor/essentials/data-platform-metrics.md).
