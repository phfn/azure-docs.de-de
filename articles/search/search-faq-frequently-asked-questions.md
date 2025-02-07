---
title: Häufig gestellte Fragen (FAQ)
titleSuffix: Azure Cognitive Search
description: Hier erhalten Sie Antworten auf häufig gestellte Fragen zum Dienst für die kognitive Azure-Suche, einem in der Cloud gehosteten Suchdienst für Microsoft Azure.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 04/10/2020
ms.openlocfilehash: 9d6acdcf9487b2d1a5964d4ec686cd23666275b0
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "88923091"
---
# <a name="azure-cognitive-search---frequently-asked-questions-faq"></a>Kognitive Azure-Suche – Häufig gestellte Fragen (FAQ)

 Hier finden Sie Antworten auf häufig gestellte Fragen zu Konzepten, Code und Szenarien der kognitiven Azure-Suche.

## <a name="platform"></a>Plattform

### <a name="how-is-azure-cognitive-search-different-from-full-text-search-in-my-dbms"></a>Wie unterscheidet sich die kognitive Azure-Suche von der Volltextsuche in meinem DBMS?

Die kognitive Azure-Suche unterstützt mehrere Datenquellen, die [linguistische Analyse für viele Sprachen](/rest/api/searchservice/language-support), die [benutzerdefinierte Analyse für interessante und ungewöhnliche Dateneingaben](/rest/api/searchservice/custom-analyzers-in-azure-search), Suchrangsteuerungen mithilfe von [Bewertungsprofilen](/rest/api/searchservice/add-scoring-profiles-to-a-search-index) und Benutzeroberflächenfunktionen wie automatische Vervollständigungen, Treffermarkierung und Facettennavigation. Darüber hinaus sind weitere Features vorhanden, z.B. Synonyme und umfassende Abfragesyntax, aber dies sind normalerweise keine außergewöhnlichen Features.

### <a name="can-i-pause-azure-cognitive-search-service-and-stop-billing"></a>Kann ich den Dienst für die kognitive Azure-Suche anhalten und so die Abrechnung von Gebühren stoppen?

Sie können den Dienst nicht anhalten. Compute- und Speicherressourcen werden beim Erstellen des Diensts für die exklusive Nutzung durch Sie zugeordnet. Es ist nicht möglich, diese Ressourcen je nach Bedarf freizugeben und dann erneut zu nutzen.

## <a name="indexing-operations"></a>Indizierungsvorgänge

### <a name="move-backup-and-restore-indexes-or-index-snapshots"></a>Können Indizes oder Indexmomentaufnahmen verschoben, gesichert und wiederhergestellt werden?

Während der Entwicklungsphase möchten Sie den Index möglicherweise zwischen den Suchdiensten verschieben. Beispielsweise möchten Sie einen Tarif „Basic“ oder „Free“ verwenden, um ihren Index zu entwickeln, und ihn dann für den Einsatz in der Produktionsumgebung in den Tarif „Standard“ oder einen höheren Tarif verschieben. 

Oder Sie möchten eine Indexmomentaufnahme in Dateien sichern, die später zum Wiederherzustellen verwendet werden können. 

Für all dies können Sie den **index-backup-restore**-Beispielcode in diesem [.NET-Beispielrepository für die kognitive Azure-Suche](https://github.com/Azure-Samples/azure-search-dotnet-samples) verwenden. 

Sie können auch jederzeit mit der REST-API für die kognitive Azure-Suche eine [Indexdefinition abrufen](/rest/api/searchservice/get-index).

Zurzeit gibt es im Azure-Portal keine integrierten Features für Indexextrahierung, Momentaufnahme oder Sicherung/Wiederherstellung. Wir erwägen jedoch, in einem zukünftigen Release Funktionen für Sicherung und Wiederherstellung hinzuzufügen. Wenn Sie Ihre Unterstützung dieses Feature ausdrücken möchten, können Sie auf [User Voice](https://feedback.azure.com/forums/263029-azure-search/suggestions/8021610-backup-snapshot-of-index) Ihre Stimme dafür abgeben.

### <a name="can-i-restore-my-index-or-service-once-it-is-deleted"></a>Kann ich meinen Index oder Dienst wiederherstellen, nachdem er gelöscht wurde?

Nein, wenn Sie einen Index oder Dienst für die kognitive Azure-Suche löschen, kann er nicht wiederhergestellt werden. Wenn Sie einen Dienst für die kognitive Azure-Suche löschen, werden alle Indizes im Dienst dauerhaft gelöscht. Wenn Sie eine Azure-Ressourcengruppe löschen, die einen oder mehrere Dienste für die kognitive Azure-Suche enthält, werden alle Dienste dauerhaft gelöscht.  

Um Ressourcen wie Indizes, Indexer, Datenquellen und Qualifikationsgruppen wiederherzustellen, müssen Sie sie aus dem Code neu erstellen. 

Um einen Index neu zu erstellen, müssen Sie Daten aus externen Quellen neu indizieren. Aus diesem Grund wird empfohlen, eine Masterkopie oder Sicherung der Originaldaten in einem anderen Datenspeicher, wie z.B. Azure SQL-Datenbank oder Cosmos DB, zu speichern.

Alternativ können Sie den **index-backup-restore**-Beispielcode in diesem [.NET-Beispielrepository für die kognitive Azure-Suche](https://github.com/Azure-Samples/azure-search-dotnet-samples) verwenden, um eine Indexdefinition und die Indexmomentaufnahme in einer Reihe von JSON-Dateien zu sichern. Später können Sie das Tool und die Dateien bei Bedarf verwenden, um den Index wiederherzustellen.  

### <a name="can-i-index-from-sql-database-replicas-applies-to-azure-sql-database-indexers"></a>Kann ich die Indizierung von SQL-Datenbankreplikaten aus durchführen (gilt für [Azure SQL-Datenbank-Indexer](./search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md))?

Es gelten keine Einschränkungen für die Verwendung von primären oder sekundären Replikaten als Datenquelle, wenn Sie einen Index völlig neu erstellen. Wenn Sie einen Index mit inkrementellen Updates (basierend auf geänderten Datensätzen) aktualisieren, wird aber das primäre Replikat benötigt. Diese Anforderung stammt aus SQL-Datenbank und garantiert die ausschließliche Änderungsnachverfolgung für primäre Replikate. Wenn Sie versuchen, sekundäre Replikate für eine Workload zur Indexaktualisierung zu verwenden, ist nicht garantiert, dass Sie alle Daten erhalten.

## <a name="search-operations"></a>Suchvorgänge

### <a name="can-i-search-across-multiple-indexes"></a>Kann ich übergreifend mehrere Indizes durchsuchen?

Nein. Dieser Vorgang wird nicht unterstützt. Die Suche ist immer auf einen einzelnen Index beschränkt.

### <a name="can-i-restrict-search-index-access-by-user-identity"></a>Kann ich den Zugriff auf den Suchindex nach Benutzeridentität beschränken?

Sie können [Sicherheitsfilter](./search-security-trimming-for-azure-search.md) mit einem `search.in()`-Filter implementieren. Der Filter lässt sich gut mit [Identitätsverwaltungsdiensten wie Azure Active Directory(AAD)](./search-security-trimming-for-azure-search-with-aad.md) kombinieren, um Suchergebnisse auf der Grundlage definierter Benutzergruppenzugehörigkeit zu trimmen.

### <a name="why-are-there-zero-matches-on-terms-i-know-to-be-valid"></a>Warum ergeben sich keine Übereinstimmungen für Begriffe, von denen ich weiß, dass sie gültig sind?

Häufig ist Benutzern in diesem Zusammenhang nicht bekannt, dass jeder Abfragetyp unterschiedliche Suchverhalten und Grade der linguistischen Analyse unterstützt. Die Volltextsuche, bei der es sich um eine gängige Workload handelt, enthält eine Sprachanalysephase, in der Ausdrücke in ihre Grundformen unterteilt werden. Dieser Aspekt der Analyse von Abfragen bewirkt, dass sich mehr Möglichkeiten für potenzielle Übereinstimmungen ergeben, da der in Token unterteilte Ausdruck mit einer größeren Zahl von Varianten übereinstimmt.

Platzhalter-, Fuzzy- und RegEx-Abfragen werden jedoch nicht wie reguläre Begriffs- oder Ausdruckabfragen analysiert und können zu einem schlechten Abruf führen, wenn die Abfrage nicht mit der analysierten Wortform im Suchindex übereinstimmt. Weitere Informationen zur Abfragenanalyse und anderen Analysen finden Sie unter [Abfragearchitektur](./search-lucene-query-architecture.md).

### <a name="my-wildcard-searches-are-slow"></a>Meine Platzhaltersuchen sind langsam.

Die meisten Platzhaltersuchabfragen wie Präfix-, Fuzzy- und RegEx-Abfragen werden intern mit übereinstimmenden Begriffen im Suchindex umgeschrieben. Diese zusätzliche Verarbeitung aufgrund des Durchsuchens des Suchindexes erhöht die Latenz. Darüber hinaus können weit gefasste Suchabfragen, wie z.B. `a*`, die wahrscheinlich mit vielen Begriffen umgeschrieben werden, sehr langsam sein. Für leistungsstarke Platzhaltersuchen sollten Sie die Definition eines [benutzerdefinierten Analysetools](/rest/api/searchservice/custom-analyzers-in-azure-search) in Betracht ziehen.

### <a name="why-is-the-search-rank-a-constant-or-equal-score-of-10-for-every-hit"></a>Warum ist der Suchrang für jeden Treffer eine Konstante bzw. immer der Wert 1,0?

Standardmäßig werden Suchergebnisse nach den [statistischen Eigenschaften übereinstimmender Ausdrücke](search-lucene-query-architecture.md#stage-4-scoring) bewertet und im Resultset von hoch bis niedrig sortiert. Einige Abfragetypen (Platzhalter, Präfix, regulärer Ausdruck) tragen aber immer eine konstante Bewertungspunktzahl zur Gesamtbewertung des Dokuments bei. Dieses Verhalten ist beabsichtigt. Die kognitive Azure-Suche wendet eine konstante Bewertungspunktzahl an, damit per Abfrageerweiterung ermittelte Übereinstimmungen in die Ergebnisse eingebunden werden können, ohne dass die Rangfolge beeinträchtigt wird.

Beispiel: Angenommen, die Eingabe von „tour*“ für eine Platzhaltersuche in englischem Text führt zu den Übereinstimmungen „tours“, „tourettes“ und „tourmaline“. Aufgrund der Art dieser Ergebnisse lässt sich nicht sicher ableiten, welche Begriffe „wertvoller“ als andere sind. Daher werden Begriffshäufigkeiten beim Bewerten von Ergebnissen in Abfragen vom Typ Platzhalter, Präfix und regulärer Ausdruck ignoriert. Suchergebnisse erhalten basierend auf einer Teileingabe eine konstante Bewertung, um eine Bevorzugung von potenziell unerwarteten Übereinstimmungen zu vermeiden.

## <a name="skillset-operations"></a>Skillsetvorgänge

### <a name="are-there-any-tips-or-tricks-to-reduce-cognitive-services-charges-on-ingestion"></a>Gibt es Tipps oder Tricks zum Verringern der Cognitive Services-Gebühren für die Erfassung?

Verständlicherweise möchten Sie integrierte oder benutzerdefinierte Skills nicht häufiger als unbedingt notwendig ausführen – insbesondere dann nicht, wenn Sie Millionen von Dokumenten verarbeiten müssen. Aus diesem Grund haben wir der Skillsetausführung Funktionen für die „inkrementelle Anreicherung“ hinzugefügt. Im Wesentlichen können Sie einen Cachespeicherort (eine Blobspeicher-Verbindungszeichenfolge) angeben, der zum Speichern der Ausgabe von „Zwischenschritten“ bei der Anreichung verwendet wird.  Auf diese Weise erhalten Sie eine intelligente Anreicherungspipeline und wenden beim Bearbeiten Ihres Skillsets nur Anreicherungen an, die benötigt werden. Gleichzeitig wird so Zeit bei der Indizierung eingespart, und die Pipeline wird effizienter.

Erfahren Sie mehr über die [inkrementelle Anreicherung](cognitive-search-incremental-indexing-conceptual.md).

## <a name="design-patterns"></a>Entwurfsmuster

### <a name="what-is-the-best-approach-for-implementing-localized-search"></a>Was ist der beste Ansatz zum Implementieren der lokalisierten Suche?

Die meisten Kunden wählen keine Sammlung, sondern dedizierte Felder, wenn in demselben Index unterschiedliche Gebietsschemas (Sprachen) unterstützt werden sollen. Gebietsschemaspezifische Felder ermöglichen die Zuweisung eines passenden Analysemoduls. Beispiel: Der Microsoft French Analyzer wird einem Feld zugewiesen, das französische Zeichenfolgen enthält. Hierdurch wird auch die Filterung vereinfacht. Wenn Sie wissen, dass eine Abfrage auf einer französischsprachigen Seite (fr-fr) initiiert wird, können Sie die Suchergebnisse auf dieses Feld beschränken. Oder erstellen Sie ein [Bewertungsprofil](/rest/api/searchservice/add-scoring-profiles-to-a-search-index), um dem Feld eine höhere relative Gewichtung zu verleihen. Für die kognitive Azure-Suche stehen über [50 unterstützte Sprachanalysetools](./search-language-support.md) zur Auswahl.

## <a name="next-steps"></a>Nächste Schritte

Haben Sie eine Frage zu einem fehlenden Feature bzw. einer fehlenden Funktion? Fordern Sie das Feature auf der [User Voice-Website](https://feedback.azure.com/forums/263029-azure-search) an.

## <a name="see-also"></a>Weitere Informationen

 [StackOverflow: Kognitive Azure-Suche](https://stackoverflow.com/questions/tagged/azure-search)   
 [Funktionsweise der Volltextsuche in Azure Cognitive Search](search-lucene-query-architecture.md)  
 [Was ist Azure Cognitive Search?](search-what-is-azure-search.md)