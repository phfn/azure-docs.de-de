---
title: Erstellen einer benutzerdefinierten Klassifizierung und Klassifizierungsregel (Vorschau)
description: Hier erfahren Sie, wie Sie benutzerdefinierte Klassifizierungen erstellen können, um Datentypen in ihrem Datenbestand zu definieren, die für Ihre Organisation in Azure Purview eindeutig sind.
author: animukherjee
ms.author: anmuk
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 3/24/2021
ms.openlocfilehash: 7d6baee49250509e50cdeeea8cf8ca6cec5b362d
ms.sourcegitcommit: 3f684a803cd0ccd6f0fb1b87744644a45ace750d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2021
ms.locfileid: "106222153"
---
# <a name="custom-classifications-in-azure-purview"></a>Benutzerdefinierte Klassifizierungen in Azure Purview

In diesem Artikel ist beschrieben, wie Sie benutzerdefinierte Klassifizierungen erstellen können, um Datentypen in ihrem Datenbestand zu definieren, die für Ihre Organisation eindeutig sind. Außerdem wird die Erstellung von benutzerdefinierten Klassifizierungsregeln beschrieben, mit denen Sie bestimmte Daten im gesamten Datenbestand finden können.

## <a name="default-classifications"></a>Standardklassifizierungen

Der Azure Purview Data Catalog bietet einen umfangreichen Satz von Standardklassifizierungen, die typische persönliche Datentypen darstellen, die es möglicherweise in ihrem Datenbestand gibt.

:::image type="content" source="media/create-a-custom-classification-and-classification-rule/classification.png" alt-text="Klassifizierung auswählen" border="true":::

Sie haben auch die Möglichkeit, benutzerdefinierte Klassifizierungen zu erstellen, wenn keine der Standardklassifizierungen Ihren Anforderungen entspricht.

## <a name="steps-to-create-a-custom-classification"></a>Schritte zum Erstellen einer benutzerdefinierten Klassifizierung

Führen Sie die folgenden Schritte aus, um eine benutzerdefinierten Klassifizierungsregel zu erstellen:

1. Wählen Sie in Ihrem Katalog die Option **Verwaltungscenter** im linken Menü aus.

2. Wählen Sie **Klassifizierungen** unter **Metadatenverwaltung** aus.

3. Wählen Sie **+ Neu** aus.

   :::image type="content" source="media/create-a-custom-classification-and-classification-rule/new-classification.png" alt-text="Neue Klassifizierung" border="true":::

Der Bereich **Neue Klassifizierung hinzufügen** wird geöffnet, in dem Sie ihrer Klassifizierung einen Namen und eine Beschreibung geben können. Es empfiehlt sich, eine auf Namespaces basierende Konvention zu verwenden, z. B. `your company name.classification name`.

Die Microsoft-Systemklassifizierungen sind unter dem reservierten `MICROSOFT.`-Namespace gruppiert. Ein Beispiel ist **MICROSOFT.GOVERNMENT.US.SOCIAL\_SECURITY\_NUMBER**.

Der Name einer Klassifizierung muss mit einem Buchstaben beginnen, auf den eine Folge von Buchstaben, Ziffern und Punkten (.) oder Unterstrichen folgt. Leerzeichen sind nicht zulässig. Während Sie tippen, generiert die Benutzeroberfläche automatisch einen Anzeigenamen. Dieser Anzeigename ist der Namen, den Benutzer sehen, wenn Sie ihn auf ein Asset (Objekt) im Katalog anwenden.

Damit der Name möglichst kurz bleibt, erstellt das System den Anzeigenamen gemäß der folgenden Logik:

- Alle Segmente mit Ausnahme der letzten beiden Segmente des Namespace werden gekürzt.

- Die Groß-/Kleinschreibung wird so angepasst, dass der erste Buchstabe jedes Worts groß geschrieben ist. Alle anderen Buchstaben werden in Kleinbuchstaben konvertiert.

- Jeder Unterstrich (\_) wird durch ein Leerzeichen ersetzt.

Beispiel: Wenn Sie Ihrer Klassifizierung den Namen **CONTOSO.HR.EMPLOYEE\_ID** gegeben haben, wird der Anzeigename im System als **Hr.Employee ID** gespeichert.

:::image type="content" source="media/create-a-custom-classification-and-classification-rule/contoso-hr-employee-id.png" alt-text="Contoso.hr.employee_id" border="true":::

Wählen Sie **OK** aus. Danach wird ihre neue Klassifizierung zu Ihrer Klassifizierungsliste hinzugefügt.

:::image type="content" source="media/create-a-custom-classification-and-classification-rule/custom-classification.png" alt-text="Benutzerdefinierte Klassifizierung " border="true":::

Wird die Klassifizierung in der Liste ausgewählt, wird die Seite mit den Klassifizierungsdetails geöffnet. Auf dieser Seite finden Sie alle Details zur Klassifizierung.

Zu diesen Details gehören die Anzahl der Instanzen, die vorhanden sind, der formale Name, zugeordnete Klassifizierungsregeln (sofern vorhanden) und der Besitzername.

:::image type="content" source="media/create-a-custom-classification-and-classification-rule/select-classification.png" alt-text="Klassifizierung auswählen" border="true":::

## <a name="custom-classification-rules"></a>Benutzerdefinierte Klassifizierungsregeln

Der Katalogdienst stellt eine Reihe von Standardklassifizierungsregeln bereit, die vom Scanner (Überprüfung) verwendet werden, um bestimmte Datentypen automatisch zu erkennen. Sie können auch eigene benutzerdefinierte Klassifizierungsregeln hinzufügen, um andere Datentypen zu erkennen, die Sie möglicherweise in Ihrem gesamten Datenbestand finden möchten. Diese Funktionalität kann sehr leistungsstark sein, wenn Sie versuchen, Daten in ihrem Datenbestand zu finden.

Nehmen Sie beispielsweise an, dass ein Unternehmen namens Contoso Personal-IDs verwendet, die im gesamten Unternehmen mit dem Wort \"Employee\" gefolgt von einer GUID standardisiert sind, um EMPLOYEE{GUID} zu erstellen. Eine Instanz einer Mitarbeiter-ID sieht zum Beispiel wie `EMPLOYEE9c55c474-9996-420c-a285-0d0fc23f1f55` aus.

Contoso kann das Überprüfungssystem durch Erstellen einer benutzerdefinierten Klassifizierungsregel so konfigurieren, dass Instanzen dieser IDs gefunden werden. Contoso kann einen regulären Ausdruck bereitstellen, der dem Datenmuster entspricht, in diesem Fall `\^Employee\[A-Za-z0-9\]{8}-\[A-Za-z0-9\]{4}-\[A-Za-z0-9\]{4}-\[A-Za-z0-9\]{4}-\[A-Za-z0-9\]{12}\$`. Zusätzlich kann Contoso, wenn sich die Daten normalerweise in einer Spalte befinden, deren Name bekannt ist (z. B. „Employee\_ID“ oder „EmployeeID“), einen regulären Ausdruck für das Spaltenmuster hinzufügen, um die Überprüfung noch präziser zu gestalten. Ein Beispiel für einen regulären Ausdruck ist „Employee\_ID\|EmployeeID“.

Das Überprüfungssystem kann dann diese Regel verwenden, um die tatsächlichen Daten in der Spalte und den Spaltennamen auszuwerten, um zu versuchen, jede Instanz zu erkennen, in der das Muster für die Personal-ID (Employee-ID) zu finden ist.

## <a name="steps-to-create-a-custom-classification-rule"></a>Schritte zum Erstellen einer benutzerdefinierten Klassifizierungsregel

So erstellen Sie eine benutzerdefinierte Klassifizierungsregel:

1. Erstellen Sie eine benutzerdefinierte Klassifizierung, indem Sie die Anweisungen im vorherigen Abschnitt befolgen. Sie fügen diese benutzerdefinierte Klassifizierung in der Klassifizierungsregelkonfiguration hinzu, sodass das System die Klassifizierung anwendet, wenn es eine Entsprechung in der Spalte findet.

2. Wählen Sie das **Verwaltungscenter**-Symbol aus.

3. Wählen Sie den Abschnitt **Klassifizierungsregeln** aus.

   :::image type="content" source="media/create-a-custom-classification-and-classification-rule/classificationrules.png" alt-text="Kachel mit Klassifizierungsregeln" border="true":::

4. Klicken Sie auf **Neu**.

   :::image type="content" source="media/create-a-custom-classification-and-classification-rule/newclassificationrule.png" alt-text="Neue Klassifizierungsregel hinzufügen" border="true":::

5. Das Dialogfeld **Neue Klassifizierungsregel** wird geöffnet. Füllen Sie die Felder aus, und entscheiden Sie, ob Sie eine Regel vom Typ **Regulärer Ausdruck** oder eine Regel vom Typ **Wörterbuch** erstellen möchten.

   |Feld     |BESCHREIBUNG  |
   |---------|---------|
   |Name   |    Erforderlich. Es sind maximal 100 Zeichen zulässig.    |
   |BESCHREIBUNG      |Optional. Es sind maximal 256 Zeichen zulässig.    |
   |Klassifizierungsname    | Erforderlich. Wählen Sie den Namen der Klassifizierung in der Dropdownliste aus, um den Scanner anzuweisen, die Klassifizierung anzuwenden, wenn eine Entsprechung gefunden wurde.        |
   |State   |  Erforderlich. Es gibt Optionen „Aktiviert“ und „Deaktiviert“. „Aktiviert“ ist die Standardeinstellung.    |

   :::image type="content" source="media/create-a-custom-classification-and-classification-rule/create-new-classification-rule.png" alt-text="Neue Klassifizierungsregel erstellen" border="true":::

### <a name="creating-a-regular-expression-rule"></a>Erstellen einer Regel vom Typ „Regulärer Ausdruck“

1. Wenn Sie eine Regel vom Typ „Regulärer Ausdruck“ erstellen, wird der folgende Bildschirm angezeigt. Sie können Sie optional eine Datei zum **Generieren vorgeschlagener RegEx-Muster** für Ihre Regel hochladen.

   :::image type="content" source="media/create-a-custom-classification-and-classification-rule/create-new-regex-rule.png" alt-text="Erstellen einer neuen RegEx-Regel" border="true":::

1. Wenn Sie sich für die Generierung eines empfohlenen RegEx-Musters entscheiden, wählen Sie nach dem Hochladen einer Datei eines der vorgeschlagenen Muster aus, und wählen Sie **Add to Patterns** (Zu Mustern hinzufügen), um die vorgeschlagenen Daten- und Spaltenmuster zu verwenden. Sie können die vorgeschlagenen Muster optimieren oder eigene Muster eingeben, ohne eine Datei hochzuladen.

   :::image type="content" source="media/create-a-custom-classification-and-classification-rule/suggested-regex.png" alt-text="Generieren vorgeschlagener regulärer Ausdrücke" border="true":::

   |Feld     |BESCHREIBUNG  |
   |---------|---------|
   |Datenmuster    |Optional. Ein regulärer Ausdruck, der den Daten entspricht, die im Datenfeld gespeichert sind. Der Grenzwert ist sehr groß. Im vorherigen Beispiel bewirkt das Datenmuster, dass auf eine Personal-ID geprüft wird, die förmlich dem Wort `Employee{GUID}` entspricht.  |
   |Spaltenmuster    |Optional. Ein regulärer Ausdruck, der den Spaltennamen entspricht, die Sie abgleichen möchten. Der Grenzwert ist sehr groß. |

1. Unter **Datenmuster** können zwei Schwellenwerte festgelegt werden:

   - **Schwellenwert für Datenunterschiede** (Distinct match threshold): Die Gesamtanzahl der unterschiedlichen Datenwerte, die in einer Spalte gefunden werden müssen, bevor der Scanner das Datenmuster für die Spalte ausführt. Der vorgeschlagene Wert ist „8“. Dieser Wert kann im Bereich von 2 bis 32 manuell angepasst werden. Das System benötigt diesen Wert, um sicherzustellen, dass die Spalte genügend Daten enthält, damit der Scanner sie genau klassifizieren kann. Beispielsweise wird eine Spalte, die mehrere Zeilen enthält, die alle den Wert 1 enthalten, nicht klassifiziert. Spalten, in denen eine Zeile einen Wert enthält und die restlichen Zeilen NULL-Werte enthalten, werden ebenfalls nicht klassifiziert. Wenn Sie mehrere Muster angeben, gilt dieser Wert für jedes dieser Muster.

   - **Schwellenwert für Mindestübereinstimmung**: Mit dieser Einstellung können Sie den Mindestprozentsatz für die individuellen Datenwertübereinstimmungen in einer Spalte festlegen, die vom Scanner gefunden werden müssen, damit die Klassifizierung angewendet wird. Der vorgeschlagene Wert ist „60 %“. Sie müssen mit dieser Einstellung vorsichtig sein. Wenn Sie die Schwelle unter 60 % verringern, kann es sein, dass Sie falsch positive Klassifizierungen in Ihren Katalog einbringen. Wenn Sie mehrere Datenmuster angeben, ist diese Einstellung deaktiviert, und der Wert ist auf „60 %“ fixiert.

1. Nun können Sie Ihre Regel überprüfen und **erstellen**.

   :::image type="content" source="media/create-a-custom-classification-and-classification-rule/verify-rule.png" alt-text="Überprüfen der Regel vor der Erstellung" border="true":::

1. Testen Sie die Klassifizierungsregel, bevor Sie den Erstellungsprozess abschließen, um zu überprüfen, ob sie Tags auf Ihre Assets anwendet. Die Klassifizierungen in der Regel werden wie bei einer Überprüfung auf die hochgeladenen Beispieldaten angewendet. Dies bedeutet, dass alle Systemklassifizierungen und Ihre benutzerdefinierte Klassifizierung mit den Daten in der Datei übereinstimmen.

   Eingabedateien können Dateien mit Trennzeichen (CSV, PSV, SSV, TSV), JSON oder XML enthalten. Der Inhalt wird basierend auf der Dateierweiterung der Eingabedatei analysiert. Begrenzungsdaten können eine Dateierweiterung aufweisen, die mit einem der erwähnten Typen übereinstimmt. Beispielsweise können TSV-Daten in einer Datei mit dem Namen MySampleData.csv vorhanden sein. Der durch Trennzeichen getrennte Inhalt muss auch mindestens 3 Spalten aufweisen.

   :::image type="content" source="media/create-a-custom-classification-and-classification-rule/test-rule-screen.png" alt-text="Testen der Regel vor der Erstellung" border="true":::

   :::image type="content" source="media/create-a-custom-classification-and-classification-rule/test-rule-uploaded-file-result-screen.png" alt-text="Angewendete Klassifizierungen nach dem Hochladen einer Testdatei anzeigen" border="true":::

### <a name="creating-a-dictionary-rule"></a>Erstellen einer Wörterbuchregel

1. Wenn Sie eine Wörterbuchregel erstellen, wird der folgende Bildschirm angezeigt. Laden Sie eine Datei hoch, die alle möglichen Werte für die von Ihnen erstellte Klassifizierung in einer einzelnen Spalte enthält.

   :::image type="content" source="media/create-a-custom-classification-and-classification-rule/dictionary-rule.png" alt-text="Erstellen einer Wörterbuchregel" border="true":::

1. Nach Generierung des Wörterbuchs können Sie die Schwellenwerte für Datenunterschiede und Mindestübereinstimmung anpassen und die Regel übermitteln.

- **Schwellenwert für Datenunterschiede** (Distinct match threshold): Die Gesamtanzahl der unterschiedlichen Datenwerte, die in einer Spalte gefunden werden müssen, bevor der Scanner das Datenmuster für die Spalte ausführt. Der unterschiedliche Übereinstimmungsschwellenwert hat nichts mit dem Musterabgleich zu tun, aber er ist eine Voraussetzung für den Musterabgleich. Der vorgeschlagene Wert ist „8“. Dieser Wert kann im Bereich von 2 bis 32 manuell angepasst werden. Das System benötigt diesen Wert, um sicherzustellen, dass die Spalte genügend Daten enthält, damit der Scanner sie genau klassifizieren kann. Beispielsweise wird eine Spalte, die mehrere Zeilen enthält, die alle den Wert 1 enthalten, nicht klassifiziert. Spalten, in denen eine Zeile einen Wert enthält und die restlichen Zeilen NULL-Werte enthalten, werden ebenfalls nicht klassifiziert. Wenn Sie mehrere Muster angeben, gilt dieser Wert für jedes dieser Muster.

   :::image type="content" source="media/create-a-custom-classification-and-classification-rule/dictionary-generated.png" alt-text="Erstellen der Wörterbuchregel mit dem Häkchen für ein generiertes Wörterbuch." border="true":::

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun ihre Klassifizierungsregel erstellt haben, kann sie einem Überprüfungsregelsatz hinzugefügt werden, sodass die Regel vom Scanner (Überprüfung) verwendet wird. Weitere Informationen finden Sie unter [Erstellen eines Überprüfungsregelsatzes](create-a-scan-rule-set.md).
