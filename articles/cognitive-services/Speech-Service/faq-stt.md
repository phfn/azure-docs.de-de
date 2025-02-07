---
title: Häufig gestellte Fragen zur Spracherkennung
titleSuffix: Azure Cognitive Services
description: Erhalten Sie Antworten auf häufig gestellte Fragen zum Spracherkennungsdienst.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/01/2021
ms.author: panosper
ms.openlocfilehash: f71fd01d45604dff843ad6eba62561937366a125
ms.sourcegitcommit: 77d7639e83c6d8eb6c2ce805b6130ff9c73e5d29
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2021
ms.locfileid: "106382333"
---
# <a name="speech-to-text-frequently-asked-questions"></a>Häufig gestellte Fragen zur Spracherkennung

Wenn Sie in diesen häufig gestellten Fragen keine Antworten auf Ihre Fragen finden, sehen Sie sich [weitere Supportoptionen](../cognitive-services-support-options.md?context=%2fazure%2fcognitive-services%2fspeech-service%2fcontext%2fcontext%253fcontext%253d%2fazure%2fcognitive-services%2fspeech-service%2fcontext%2fcontext) an.

## <a name="general"></a>Allgemein

**F: Worin besteht der Unterschied zwischen einem Basismodell und einem benutzerdefinierten Spracherkennungsmodell?**

**A:** Ein Basismodell wurde mit Daten von Microsoft trainiert und ist bereits in der Cloud bereitgestellt. Sie können ein benutzerdefiniertes Modell verwenden, um ein Modell optimal an eine bestimmte Umgebung mit speziellen Umweltgeräuschen oder individueller Sprache anzupassen. Für Fabrikhallen, Autos oder laute Straßen wäre ein angepasstes Akustikmodell erforderlich. Für bestimmte Themen wie z. B. Biologie, Physik, Radiologie, Produktnamen und benutzerdefinierte Akronyme würde ein angepasstes Sprachmodell benötigt. Wenn Sie ein benutzerdefiniertes Modell trainieren, sollten Sie mit entsprechendem Text beginnen, um die Erkennung besonderer Begriffe und Phrasen zu verbessern.

**F: Wo fange ich an, wenn ich ein Basismodell verwenden möchte?**

**A:** Rufen Sie zunächst einen [Abonnementschlüssel](overview.md#try-the-speech-service-for-free) ab. Informationen zum Ausführen von REST-Aufrufen an die vorab bereitgestellten Basismodelle finden Sie in den [REST-APIs](./overview.md#reference-docs). Wenn Sie WebSockets verwenden möchten, [laden Sie das SDK herunter](speech-sdk.md).

**F: Muss ich immer ein benutzerdefiniertes Sprachmodell erstellen?**

**A:** Nein. Wenn Ihre Anwendung generische, alltägliche Sprache verwendet, müssen Sie ein Modell nicht anpassen. Wenn Ihre Anwendung in einer Umgebung verwendet wird, in der es wenig oder gar keine Hintergrundgeräusche gibt, müssen Sie ein Modell nicht anpassen.

Sie können Basismodelle und angepasste Modelle im Portal bereitstellen und dann Genauigkeitsprüfungen für diese durchführen. Sie können dieses Feature verwenden, um die Genauigkeit eines Basismodells im Vergleich zu einem benutzerdefinierten Modell zu messen.

**F: Wie erfahre ich, wann die Verarbeitung für mein Dataset oder Modell abgeschlossen ist?**

**A:** Derzeit ist der Status des Modells oder Datasets in der Tabelle die einzige Möglichkeit, dies zu erfahren. Wenn die Verarbeitung abgeschlossen ist, lautet der Status **Erfolgreich**.

**F: Kann ich mehrere Modelle erstellen?**

**A:** Die Anzahl der Modelle, die Sie in Ihrer Sammlung haben können, ist nicht begrenzt.

**F: Ich habe festgestellt, dass ich einen Fehler gemacht habe. Wie kann ich meinen Datenimport oder die laufende Modellerstellung abbrechen?**

**A:** Derzeit können Sie einen akustischen oder sprachlichen Anpassungsprozess nicht rückgängig machen. Sie können importierte Daten und Modelle löschen, wenn sie in einem Endzustand sind.

**F: Mit dem ausführlichen Ausgabeformat erhalte ich mehrere Ergebnisse für jede Phrase. Welches sollte ich verwenden?**

**A:** Verwenden Sie immer das erste Ergebnis, selbst wenn ein anderes Ergebnis („N-Best“) einen höheren Konfidenzwert aufweist. Der Speech-Dienst betrachtet das erste Ergebnis als das beste. Dabei kann es sich auch um eine leere Zeichenfolge handeln, wenn keine Sprache erkannt wurde.

Die anderen Ergebnisse sind wahrscheinlich schlechter und verfügen möglicherweise nicht über vollständige Groß- und Kleinschreibung sowie Interpunktion. Diese Ergebnisse sind insbesondere in speziellen Szenarios hilfreich, z. B., wenn Sie Benutzern die Option bereitstellen möchten, Korrekturoptionen aus einer Liste auszuwählen, oder wenn falsch erkannte Befehle verarbeitet werden sollen.

**F: Warum gibt es unterschiedliche Basismodelle?**

**A:** Im Speech-Dienst können Sie aus mehreren Basismodellen auswählen. Jeder Modellname enthält das Datum, an dem es hinzugefügt wurde. Wenn Sie damit beginnen, ein benutzerdefiniertes Modell zu trainieren, verwenden Sie das neueste Modell, um die höchste Genauigkeit zu erzielen. Ältere Basismodelle sind für einige Zeit weiterhin verfügbar, nachdem ein neues Modell veröffentlicht wurde. Sie können das Modell, mit dem Sie bisher gearbeitet haben, weiterhin verwenden, bis es außer Betrieb gesetzt wird (siehe [Lebenszyklus von Modell und Endpunkt](./how-to-custom-speech-model-and-endpoint-lifecycle.md)). Es wird weiterhin empfohlen, zum neuesten Basismodell zu wechseln, um die Genauigkeit zu verbessern.

**F: Kann ich mein vorhandenes Modell aktualisieren (Modellstapel)?**

**A:** Sie können ein vorhandenes Modell nicht aktualisieren. Als Lösung können Sie das alte Dataset mit dem neuen Dataset kombinieren und neu anpassen.

Das alte und das neue Dataset müssen in einer einzigen ZIP-Datei (für akustische Daten) bzw. TXT-Datei (für Sprachdaten) zusammengefasst werden. Wenn die Anpassung abgeschlossen ist, muss das neue, aktualisierte Modell neu bereitgestellt werden, um einen neuen Endpunkt zu erhalten.

**F: Wird meine Bereitstellung automatisch aktualisiert, wenn eine neue Version eines Basismodells verfügbar ist?**

**A:** Bereitstellungen werden NICHT automatisch aktualisiert.

Wenn Sie ein Modell angepasst und bereitgestellt haben, bleibt diese Bereitstellung unverändert. Sie können das bereitgestellte Modell außer Betrieb nehmen, mithilfe der neueren Version des Basismodells neu anpassen und dann wieder bereitstellen, um eine höhere Genauigkeit zu erzielen.

Sowohl Basismodelle als auch benutzerdefinierte Modelle werden nach einiger Zeit außer Betrieb gesetzt (siehe [Lebenszyklus von Modell und Endpunkt](./how-to-custom-speech-model-and-endpoint-lifecycle.md)).

**F: Kann ich mein Modell herunterladen und lokal ausführen?**

**A:** Sie können ein benutzerdefiniertes Modell lokal in einem [Docker-Container](speech-container-howto.md?tabs=cstt) ausführen.

**F: Kann ich meine Datasets, Modelle und Bereitstellungen in eine andere Region oder ein anderes Abonnement kopieren bzw. verschieben?**

**A:** Sie können die [REST-API](https://centralus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/CopyModelToSubscription) verwenden, um ein benutzerdefiniertes Modell in eine andere Region bzw. ein anderes Abonnement zu kopieren. Datasets und Bereitstellungen können nicht kopiert werden. Sie können ein Datasets mithilfe der Modellkopien wieder in ein anderes Abonnement importieren und dort Endpunkte erstellen.

**F: Werden meine Anforderungen protokolliert?**

**A:** Standardmäßig werden Anforderungen nicht protokolliert (weder Audio noch Transkription). Bei Bedarf können Sie die Option *Log content from this endpoint* (Inhalte von diesem Endpunkt protokollieren) verwenden, wenn Sie [einen benutzerdefinierten Endpunkt erstellen](how-to-custom-speech-train-model.md#deploy-a-custom-model). Im [Speech SDK](how-to-use-logging.md) können Sie auch die Audioprotokollierung auf Grundlage von Anforderungen aktivieren, ohne dass Sie einen benutzerdefinierten Endpunkt erstellen müssen. In beiden Fällen werden die Audio- und Erkennungsergebnisse von Anforderungen in einem sicheren Speicher gespeichert. Bei Abonnements mit Speicherressourcen von Microsoft werden die Ergebnisse 30 Tage lang aufbewahrt.

Sie können die protokollierten Dateien auf der Bereitstellungsseite in Speech Studio exportieren, wenn Sie einen benutzerdefinierten Endpunkt mit der Option *Log content from this endpoint* (Inhalte von diesem Endpunkt protokollieren) verwenden. Wenn die Audioprotokollierung über das SDK aktiviert ist, rufen Sie die [API](https://centralus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/GetBaseModelLogs) auf, um auf die Dateien zuzugreifen.

**F: Werden meine Anforderungen gedrosselt?**

**A:** Weitere Informationen finden Sie unter [Kontingente und Grenzwerte für Speech Services](speech-services-quotas-and-limits.md).

**F: Wie werden Zweikanalaudiodaten in Rechnung gestellt?**

**A:** Wenn Sie jeden Kanal separat übermitteln (jeden Kanal in seiner eigenen Datei), wird Ihnen die Dauer jeder Datei in Rechnung gestellt. Wenn Sie in jedem Kanal eine einzelne Datei übermitteln, wird Ihnen die Dauer der einzelnen Datei in Rechnung gestellt. Details zu den Preisen finden Sie auf der [Seite mit der Preisübersicht für Azure Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

> [!IMPORTANT]
> Wenn Sie weitere Datenschutzbedenken im Hinblick auf die Nutzung von Custom Speech Service haben, wenden Sie sich an einen der Supportkanäle.

## <a name="increasing-concurrency"></a>Erhöhen der Parallelität
Weitere Informationen finden Sie unter [Kontingente und Grenzwerte für Speech Services](speech-services-quotas-and-limits.md).


## <a name="importing-data"></a>Importieren von Daten

**F: Wie groß darf das Dataset maximal sein, und was ist der Grund für dieses Limit?**

**A:** Grund für das Limit ist, dass die Größe einer Datei für den HTTP-Upload begrenzt ist. Weitere Informationen zum tatsächlichen Grenzwert finden Sie unter [Kontingente und Grenzwerte für Speech Services](speech-services-quotas-and-limits.md). Sie können Ihre Daten in mehrere Datasets unterteilen und alle zum Trainieren des Modells auswählen.

**F: Kann ich meine Textdateien komprimieren (ZIP), damit ich eine größere Textdatei hochladen kann?**

**A:** Nein. Derzeit sind nur unkomprimierte Textdateien zulässig.

**F: Der Datenbericht besagt, dass fehlerhafte Äußerungen gefunden wurden. Was ist das Problem?**

**A:** Wenn die Äußerungen in einer Datei nicht zu 100% hochgeladen werden können, stellt dies kein Problem dar. Wenn der Großteil der Äußerungen in einem Akustik- oder Sprachdataset (z. B. > 95 %) erfolgreich importiert wird, kann das Dataset verwendet werden. Allerdings sollten Sie herausfinden, warum Fehler bei den Äußerungen aufgetreten sind, und die Probleme beheben. Die meisten Probleme, z.B. Formatierungsfehler, sind einfach zu beheben.

## <a name="creating-an-acoustic-model"></a>Erstellen eines Akustikmodells

**F: Wie viele Akustikdaten benötige ich?**

**A:** Sie sollten zunächst mit 30–60 Minuten Akustikdaten beginnen.

**F: Welche Daten soll ich sammeln?**

**A:** Sammeln Sie Daten, die dem Anwendungsszenario und dem Anwendungsfall möglichst nahe kommen. Die Datensammlung sollte in Bezug auf Geräte, Umgebungen und Sprechertypen mit der Zielanwendung und den Benutzern übereinstimmen. Generell sollten Sie Daten von möglichst vielen Sprechern sammeln.

**F: Wie soll ich akustische Daten sammeln?**

**A:** Sie können eine eigenständige Datensammlungsanwendung erstellen oder eine handelsübliche Audioaufzeichnungssoftware verwenden. Sie können auch eine Version der Anwendung erstellen, die Audiodaten protokolliert und dann diese Daten verwendet.

**F: Muss ich die Anpassungsdaten selbst transkribieren?**

**A:** Ja. Sie können sie selbst transkribieren oder einen professionellen Transkriptionsdienst nutzen. Manche Benutzer bevorzugen professionelle Transkriptionsdienste, während andere Crowdsourcing nutzen oder die Transkriptionen selbst durchführen.

**F: Wie lang dauert das Trainieren eines benutzerdefinierten Modells mit Audiodaten?**

**A:** Das Trainieren eines Modells mit Audiodaten ist ein langwieriger Prozess. Je nach Menge der Daten kann das Erstellen eines benutzerdefinierten Modells mehrere Tage dauern. Wenn das Training nicht innerhalb einer Woche abgeschlossen werden kann, bricht der Dienst den Trainingsprozess möglicherweise ab und meldet das Modell als fehlgeschlagen.

Verwenden Sie eine der [Regionen](custom-speech-overview.md#set-up-your-azure-account), in der dedizierte Hardware für das Training verfügbar ist. Der Speech-Dienst verwendet in diesen Regionen bis zu 20 Stunden Audiodaten für das Training. In anderen Regionen werden nur bis zu 8 Stunden verwendet.

Im Allgemeinen verarbeitet der Dienst ungefähr 10 Stunden an Audiodaten pro Tag in Regionen mit dedizierter Hardware. In anderen Regionen kann täglich nur eine Stunde an Audiodaten verarbeitet werden. Sie können das vollständig trainierte Modell mithilfe der [REST-API](https://centralus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/CopyModelToSubscription) in eine andere Region kopieren. Training, bei dem ausschließlich Text verwendet wird, ist deutlich schneller und dauert in der Regel nur Minuten.

Einige Basismodelle können nicht mit Audiodaten angepasst werden. Für diese verwendet der Dienst nur den Text der Transkription für das Training. Die Audiodaten werden ignoriert. Das Training wird dann schneller abgeschlossen, und die Ergebnisse sind mit den Ergebnissen des Trainings identisch, für das nur Text verwendet wird. Eine Liste mit Basismodellen, die das Training mit Audiodaten unterstützen, finden Sie unter [Sprachunterstützung](language-support.md#speech-to-text).

## <a name="accuracy-testing"></a>Genauigkeitstests

**F: Was ist die Wort-Fehler-Rate (Word Error Rate, WER), und wie wird sie berechnet?**

**A:** Die Wort-Fehler-Rate (WER) ist die Auswertungsmetrik für die Spracherkennung. WER wird berechnet als die Gesamtanzahl von Fehlern, einschließlich Einfügungen, Löschungen und Ersetzungen, dividiert durch die Gesamtzahl der Wörter in der Referenztranskription. Weitere Informationen finden Sie unter [Auswerten der Custom Speech-Genauigkeit](how-to-custom-speech-evaluate-data.md#evaluate-custom-speech-accuracy).

**F: Wie kann ich feststellen, ob die Ergebnisse einer Genauigkeitsprüfung gut sind?**

**A:** Die Ergebnisse stellen einen Vergleich zwischen dem Basismodell und dem von Ihnen angepassten Modell dar. Sie sollten das Basismodell übertreffen, damit sich die Anpassung lohnt.

**F: Wie bestimme ich die Wort-Fehler-Rate (WER) eines Basismodells, damit ich sehen kann, ob es eine Verbesserung gab?**

**A:** Die Offlinetestergebnisse zeigen die Genauigkeit des Basismodells und des benutzerdefinierten Modells sowie die Verbesserung gegenüber dem Basismodell.

## <a name="creating-a-language-model"></a>Erstellen eines Sprachmodells

**F: Wie viele Textdaten muss ich hochladen?**

**A:** Es hängt davon ab, wie stark sich die in der Anwendung verwendeten Vokabeln und Ausdrücke von den Ausgangssprachmodellen unterscheiden. Für alle neuen Wörter ist es hilfreich, so viele Beispiele wie möglich für ihre Verwendung bereitzustellen. Für gängige Ausdrücke, die in Ihrer Anwendung verwendet werden, sind auch Ausdrücke in den Sprachdaten nützlich, da sie das System anweisen, auch auf diese Begriffe zu achten. Üblicherweise sollte das Sprachdataset mindestens 100 und typischerweise mehrere hundert Äußerungen oder mehr umfassen. Auch wenn bestimmte Arten von Abfragen häufiger als andere erwartet werden, können Sie mehrere Kopien der häufigen Abfragen in das Dataset einfügen.

**F: Kann ich nur eine Liste von Wörtern hochladen?**

**A:** Das Hochladen einer Liste von Wörtern fügt die Wörter dem Wortschatz hinzu, trainiert das System aber nicht, wie die Wörter typischerweise verwendet werden. Durch die Bereitstellung vollständiger oder teilweiser Äußerungen (Sätze oder Ausdrücke, die von Benutzern üblicherweise verwendet werden) kann das Sprachmodell die neuen Wörter und deren Verwendung lernen. Das benutzerdefinierte Sprachmodell eignet sich nicht nur gut dazu, neue Wörter dem System hinzuzufügen, sondern auch, die Wahrscheinlichkeit bekannter Wörter für Ihre Anwendung anzupassen. Vollständige Äußerungen helfen dem System, besser zu lernen.

## <a name="tenant-model-custom-speech-with-microsoft-365-data"></a>Mandantenmodell (Custom Speech mit Microsoft 365-Daten)

**F: Welche Informationen sind im Mandantenmodell enthalten, und wie wird es erstellt?**

**A:** Ein Mandantenmodell wird mit E-Mails und Dokumenten der [öffentlichen Gruppe](https://support.microsoft.com/office/learn-about-microsoft-365-groups-b565caa1-5c40-40ef-9915-60fdb2d97fa2) erstellt, die von allen Personen in Ihrer Organisation eingesehen werden können.

**F: Welche Sprachfunktion wird durch das Mandantenmodell verbessert?**

**A:** Wenn das Mandantenmodell aktiviert, erstellt und veröffentlicht ist, wird es verwendet, um die Erkennung für Unternehmensanwendungen zu verbessern, die mithilfe des Speech-Diensts erstellt wurden. Dadurch wird auch ein Azure AD-Token übergeben, das die Mitgliedschaft im Unternehmen angibt.

Die in Microsoft 365 integrierten Sprachfunktionen wie Diktat und PowerPoint-Untertitel werden nicht geändert, wenn Sie ein Mandantenmodell für Ihre Speech-Dienstanwendungen erstellen.

## <a name="next-steps"></a>Nächste Schritte

- [Problembehandlung](troubleshooting.md)
- [Versionshinweise](releasenotes.md)