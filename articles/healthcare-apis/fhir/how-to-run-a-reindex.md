---
title: Ausführen eines Neuindizierungsauftrags in Azure API for FHIR
description: In diesem Artikel wird beschrieben, wie Sie einen Neuindizierungsauftrag ausführen, um Such- oder Sortierparameter zu indizieren, die noch nicht in Ihrer Datenbank indiziert wurden.
author: ginalee-dotcom
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 4/23/2021
ms.author: cavoeg
ms.openlocfilehash: 905d9381ac93e38575e9d0ff5c6f5571122b2990
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110476645"
---
# <a name="running-a-reindex-job"></a>Ausführen eines Neuindizierungsauftrags

Es gibt Szenarien, in denen Sie möglicherweise Such- oder Sortierparameter in der Azure API for FHIR haben, die noch nicht indiziert wurden. Dies ist besonders relevant, wenn Sie Eigene Suchparameter definieren. Bis der Suchparameter indiziert ist, kann er nicht in der Suche verwendet werden. Dieser Artikel enthält eine Übersicht über das Ausführen eines Neuindizierungsauftrags zum Indizieren von Such- oder Sortierparametern, die noch nicht in Ihrer Datenbank indiziert wurden.

> [!Warning]
> Es ist wichtig, dass Sie diesen gesamten Artikel lesen, bevor Sie beginnen. Ein Neuindizierungsauftrag kann sehr leistungsintensiv sein. Dieser Artikel enthält Optionen zum Drosseln und Steuern des Neuindizierungsauftrags.

## <a name="how-to-run-a-reindex-job"></a>Ausführen eines Auftrags zum Neuindizieren 

Verwenden Sie das folgende Codebeispiel, um einen Neuindizierungsauftrag zu starten:

```json
POST {{FHIR URL}}/$reindex 

{ 

“resourceType”: “Parameters”,  

“parameter”: [] 

}
 ```

Wenn die Anforderung erfolgreich ist, wird der Status **201 Erstellt** zurückgegeben. Das Ergebnis dieser Meldung sieht wie folgt aus:

```json
HTTP/1.1 201 Created 
Content-Location: https://{{FHIR URL}}/_operations/reindex/560c7c61-2c70-4c54-b86d-c53a9d29495e 

{
  "resourceType": "Parameters",
  "id": "560c7c61-2c70-4c54-b86d-c53a9d29495e",
  "meta": {
    "versionId": "\"4c0049cd-0000-0100-0000-607dc5a90000\""
  },
  "parameter": [
    {
      "name": "id",
      "valueString": "560c7c61-2c70-4c54-b86d-c53a9d29495e"
    },
    {
      "name": "queuedTime",
      "valueDateTime": "2021-04-19T18:02:17.0118558+00:00"
    },
    {
      "name": "totalResourcesToReindex",
      "valueDecimal": 0.0
    },
    {
      "name": "resourcesSuccessfullyReindexed",
      "valueDecimal": 0.0
    },
    {
      "name": "progress",
      "valueDecimal": 0.0
    },
    {
      "name": "status",
      "valueString": "Queued"
    },
    {
      "name": "maximumConcurrency",
      "valueDecimal": 1.0
    },
    {
      "name": "resources",
      "valueString": ""
    },
    {
      "name": "searchParams",
      "valueString": ""
    }
  ]
}
```

> [!NOTE]
> Um den Status eines Neuindizierungsauftrags zu überprüfen oder abzubrechen, benötigen Sie die Neuindizierungs-ID. Dies ist die ID der resultierenden Parameters-Ressource. Im obigen Beispiel wäre die ID für den Neuindizierungsauftrag `560c7c61-2c70-4c54-b86d-c53a9d29495e` .

 ## <a name="how-to-check-the-status-of-a-reindex-job"></a>Überprüfen des Status eines Neuindizierungsauftrags

Nachdem Sie einen Neuindizierungsauftrag gestartet haben, können Sie den Status des Auftrags wie folgt überprüfen:

`GET {{FHIR URL}}/_operations/reindex/{{reindexJobId}`

Der Status des Auftragsergebnisses für die Neuindizierung ist unten dargestellt:

```json
{

  "resourceType": "Parameters",
  "id": "b65fd841-1c62-47c6-898f-c9016ced8f77",
  "meta": {

    "versionId": "\"1800f05f-0000-0100-0000-607a1a7c0000\""
  },
  "parameter": [

    {

      "name": "id",
      "valueString": "b65fd841-1c62-47c6-898f-c9016ced8f77"
    },
    {

      "name": "startTime",
      "valueDateTime": "2021-04-16T23:11:35.4223217+00:00"
    },
    {

      "name": "queuedTime",
      "valueDateTime": "2021-04-16T23:11:29.0288163+00:00"
    },
    {

      "name": "totalResourcesToReindex",
      "valueDecimal": 262544.0
    },
    {

      "name": "resourcesSuccessfullyReindexed",
      "valueDecimal": 5754.0
    },
    {

      "name": "progress",
      "valueDecimal": 2.0
    },
    {

      "name": "status",
      "valueString": "Running"
    },
    {

      "name": "maximumConcurrency",
      "valueDecimal": 1.0
    },
    {

      "name": "resources",
      "valueString": 
      "{LIST OF IMPACTED RESOURCES}"
    },
    {
```

Die folgenden Informationen werden im Ergebnis des Neuindizierungsauftrags angezeigt:

* **totalResourcesToReindex:** Enthält die Gesamtzahl der Ressourcen, die im Rahmen des Auftrags neu indiziert werden.

* **resourcesSuccessfullyReindexed:** Die Summe, die bereits erfolgreich neu indiziert wurde.

* **progress:** Auftrag in Prozent neu indizieren abgeschlossen. Entspricht resourcesSuccessfullyReindexed/totalResourcesToReindex x 100.

* **status:** Gibt an, ob der Auftrag für die Neuindizierung in die Warteschlange gestellt, ausgeführt, abgeschlossen, fehlgeschlagen oder abgebrochen wurde.

* **resources:** Hier werden alle Ressourcentypen aufgeführt, die vom Neuindizierungsauftrag in Frage stehen.

## <a name="delete-a-reindex-job"></a>Löschen eines Neuindizierungsauftrags

Wenn Sie einen Auftrag zum erneuten Indizieren abbrechen müssen, verwenden Sie einen Löschaufruf, und geben Sie die Auftrags-ID für die Neuindizierung an:

`Delete {{FHIR URL}}/_operations/reindex/{{reindexJobId}`

## <a name="performance-considerations"></a>Überlegungen zur Leistung

Ein Neuindizierungsauftrag kann sehr leistungsintensiv sein. Wir haben einige Drosselungssteuerelemente implementiert, mit deren Hilfe Sie verwalten können, wie ein Neuindizierungsauftrag in Ihrer Datenbank ausgeführt wird.

> [!NOTE]
> Es ist nicht ungewöhnlich, dass große Datasets einen Auftrag für die Neuindizierung tagelang ausführen. Für eine Datenbank mit 30.000.000 Millionen Ressourcen haben wir festgestellt, dass es bei 100.000 RUs 4 bis 5 Tage gelangt hat, die gesamte Datenbank neu zu indizieren.

Im Folgenden finden Sie eine Tabelle mit den verfügbaren Parametern, Standardwerten und empfohlenen Bereichen. Sie können diese Parameter verwenden, um entweder den Prozess zu beschleunigen (mehr Compute zu verwenden) oder den Prozess zu verlangsamen (weniger Compute zu verwenden). Beispielsweise können Sie den Auftrag für die Neuindizierung mit einer geringen Datenverkehrszeit ausführen und Ihre Computeleistung erhöhen, um dies schneller zu erledigen. Stattdessen können Sie die Einstellungen verwenden, um eine sehr geringe Computenutzung sicherzustellen und diese tagelang im Hintergrund ausführen zu lassen. 

| **Parameter**                     | **Beschreibung**              | **Standard**        | **Empfohlener Bereich**           |
| --------------------------------- | ---------------------------- | ------------------ | ------------------------------- |
| QueryDelayIntervalInMilliseconds  | Dies ist die Verzögerung zwischen jedem Batch von Ressourcen, der während des Neuindizierungsauftrags startet. | 500 MS (,5 Sekunden) | 50 bis 5000: 50 beschleunigt den Neuindizierungsauftrag, und 5000 verlangsamt ihn vom Standardwert. |
| MaximumResourcesPerQuery  | Dies ist die maximale Anzahl von Ressourcen, die im Batch der Ressourcen enthalten sind, die neu indiziert werden sollen.  | 100 | 1-500 |
| MaximumConcurreny  | Dies ist die Anzahl von Batches, die gleichzeitig ausgeführt werden.  | 1 | 1–5 |
| targetDataStoreUsagePercentrage | Dadurch können Sie angeben, welcher Prozentsatz Ihres Datenspeichers für den Neuindizierungsauftrag verwendet werden soll. Beispielsweise können Sie 50 % angeben, und dies würde sicherstellen, dass der Neuindizierungsauftrag höchstens 50 % der verfügbaren RUs auf Cosmos DB verwendet.  | Kein vorhanden, was bedeutet, dass bis zu 100 % verwendet werden können. | 1-100 |

Wenn Sie einen der oben genannten Parameter verwenden möchten, können Sie diese beim Starten des Auftrags für die Neuindizierung an die Ressource Parameter übergeben.

```json
{
  "resourceType": "Parameters",
  "parameter": [
    {
      "name": "maximumConcurrency",
      "valueInteger": "3"
    },
    {
      "name": "targetDataStoreUsagePercentage",
      "valueInteger": "20"
    },
    {
      "name": "queryDelayIntervalInMilliseconds",
      "valueInteger": "1000"
    },
    {
      "name": "maximumNumberOfResourcesPerQuery",
      "valueInteger": "1"
    }
  ]
}
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie gelernt, wie Sie einen Neuindizierungsauftrag starten. Informationen zum Definieren neuer Suchparameter, die den Auftrag für die Neuindizierung erfordern, finden Sie unter 

>[!div class="nextstepaction"]
>[Definieren von benutzerdefinierten Suchparametern](how-to-do-custom-search.md)

         
     