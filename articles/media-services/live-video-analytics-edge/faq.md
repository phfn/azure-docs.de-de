---
title: Häufig gestellte Fragen zu Azure Live Video Analytics in IoT Edge
description: Dieser Artikel bietet Antworten auf häufig gestellte Fragen zu Azure Live Video Analytics in IoT Edge.
ms.topic: conceptual
ms.date: 12/01/2020
ms.openlocfilehash: ed27b120ff28704aeb3595815544262bab78e95d
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110371450"
---
# <a name="azure-live-video-analytics-on-iot-edge-faq"></a>Häufig gestellte Fragen zu Azure Live Video Analytics in IoT Edge

[!INCLUDE [redirect to Azure Video Analyzer](./includes/redirect-video-analyzer.md)]

Dieser Artikel bietet Antworten auf häufig gestellte Fragen zu Live Video Analytics in Azure IoT Edge.

## <a name="general"></a>Allgemein

**Welche Systemvariablen kann ich in der Definition der Graphtopologie verwenden?**

| Variable   |  BESCHREIBUNG  | 
| --- | --- | 
| [System.DateTime](/dotnet/framework/data/adonet/sql/linq/system-datetime-methods) | Stellt einen Zeitpunkt (in UTC) dar, der üblicherweise als Datum und Uhrzeit im folgenden Format ausgedrückt wird:<br>*jjjjMMttTHHmmssZ* | 
| System.PreciseDateTime | Stellt eine DateTime-Instanz in koordinierter Weltzeit (UTC) in einem ISO8601-konformen Format mit Millisekunden im folgenden Format dar:<br>*jjjjMMttTHHmmss.fffZ* | 
| System.GraphTopologyName   | Stellt eine Mediengraphtopologie dar und enthält die Blaupause eines Graphen. | 
| System.GraphInstanceName |    Stellt eine Mediengraphinstanz dar, enthält die Parameterwerte und verweist auf die Topologie. | 

## <a name="configuration-and-deployment"></a>Konfiguration und Bereitstellung

**Kann ich das Edgemodul für Medien auf einem Windows 10-Gerät bereitstellen?**

Ja. Weitere Informationen finden Sie unter [Linux-Container unter Windows 10](/virtualization/windowscontainers/deploy-containers/linux-containers).

## <a name="capture-from-ip-camera-and-rtsp-settings"></a>Aufzeichnen von IP-Kameras und RTSP-Einstellungen

**Muss ich auf meinem Gerät ein spezielles SDK verwenden, um einen Videostream zu senden?**

Nein, Live Video Analytics in IoT Edge unterstützt das Aufzeichnen von Medien mithilfe des Videostreamingprotokolls RTSP (Real-Time Streaming Protocol) für Videostreams, das von den meisten IP-Kameras unterstützt wird.

**Kann ich Medien mithilfe von RTMP (Real-Time Messaging Protocol) oder dem Smooth Streaming-Protokoll (z. B. von einem Media Services-Liveereignis) an Live Video Analytics in IoT Edge pushen?**

Nein, Live Video Analytics unterstützt für die Aufzeichnung von Videos von IP-Kameras nur RTSP. Alles Kameras, die das RTSP-Streaming über TCP/HTTP unterstützen, sollten funktionieren. 

**Kann ich die URL der RTSP-Quelle in einer Graphinstanz zurücksetzen oder aktualisieren?**

Ja, sofern sich die Graphinstanz in einem *inaktiven* Zustand befindet.  

**Ist ein RTSP-Simulator verfügbar, der bei Tests und während der Entwicklung verwendet werden kann?**

Ja, es steht ein [RTSP-Simulator](https://github.com/Azure/live-video-analytics/tree/master/utilities/rtspsim-live555) als Edgemodul zur Verfügung, das Sie in den Schnellstarts und Tutorials zur Unterstützung des Lernprozesses verwenden können. Dieses Modul wird nach bestem Wissen bereitgestellt und ist möglicherweise nicht immer verfügbar. Es wird dringend empfohlen, den Simulator *nicht* länger als einige Stunden zu verwenden. Sie sollten Tests mit Ihrer eigentlichen RTSP-Quelle durchführen, bevor Sie eine Produktionsbereitstellung planen.

**Wird die ONVIF-Ermittlung von IP-Kameras am Edge unterstützt?**

Nein, die ONVIF-Ermittlung (Open Network Video Interface Forum) von Geräten am Edge wird nicht unterstützt.

## <a name="streaming-and-playback"></a>Streaming und Wiedergabe

**Kann ich mithilfe von Streamingtechnologien wie HLS oder DASH Medienobjekte wiedergeben, die am Edge für Azure Media Services aufgezeichnet wurden?**

Ja. Sie können die aufgezeichneten Medienobjekte wie jedes andere Medienobjekt in Azure Media Services streamen. Um den Inhalt zu streamen, müssen Sie einen Streamingendpunkt erstellt haben, der den Status „Wird ausgeführt“ aufweist. Wenn Sie den Standarderstellungsprozess für den Streaminglocator ausführen, erhalten Sie Zugriff auf ein HLS- (HTTP Live Streaming von Apple) oder DASH-Manifest (Dynamic Adaptive Streaming over HTTP, auch als MPEG-DASH bezeichnet), mit dem das Streaming an ein beliebiges unterstütztes Wiedergabeframework möglich ist. Weitere Informationen zum Erstellen und Veröffentlichen von HLS- oder DASH-Manifesten finden Sie unter [Dynamische Paketerstellung](../latest/encode-dynamic-packaging-concept.md).

**Kann ich die Standardschutzfunktionen für Inhalte und die DRM-Features von Media Services für ein archiviertes Medienobjekt verwenden?**

Ja. Der gesamte Standardschutz von Inhalten durch dynamische Verschlüsselung und DRM-Features (Digital Rights Management) ist für die Verwendung bei Medienobjekten verfügbar, die von einem Mediengraphen aufgezeichnet werden.

**Welche Player kann ich verwenden, um Inhalte von aufgezeichneten Medienobjekten anzuzeigen?**

Es werden alle Standardplayer unterstützt, die mit Version 3 oder 4 von HLS konform sind. Außerdem wird jeder Player unterstützt, der das MPEG-DASH-Format wiedergeben kann.

Empfohlene Player für Tests:

* [Azure Media Player](../latest/player-use-azure-media-player-how-to.md)
* [HLS.js](https://hls-js.netlify.app/demo/)
* [Video.js](https://videojs.com/)
* [Dash.js](https://github.com/Dash-Industry-Forum/dash.js/wiki)
* [Shaka Player](https://github.com/google/shaka-player)
* [ExoPlayer](https://github.com/google/ExoPlayer)
* [Natives HTTP Live Streaming (HLS) von Apple](https://developer.apple.com/streaming/)
* Microsoft Edge, Chrome oder Safari mit integriertem HTML5-Videoplayer
* Kommerzielle Player, die die HLS- oder DASH-Wiedergabe unterstützen

**Welche Einschränkungen gelten für das Streaming eines Medienobjekts aus einem Mediendiagramm?**

Beim Streamen von Liveinhalten oder aufgezeichneten Medienobjekten aus einem Mediendiagramm wird dieselbe hochskalierbare Infrastruktur und derselbe Streamingendpunkt verwendet, die Media Services auch für das On-Demand- und Livestreaming von Medien- und Unterhaltungs-, OTT- (Over the Top) und Übertragungskunden unterstützt. Sie können also ganz einfach und schnell Dienste wie Azure CDN (Content Delivery Network), Verizon oder Akamai aktivieren, um Ihre Inhalte je nach Szenario für ein Publikum von wenigen einzelnen bis zu Millionen von Zuschauern bereitzustellen.

Sie können Inhalte mithilfe von Apple HLS oder MPEG-DASH bereitstellen.

## <a name="design-your-ai-model"></a>Entwerfen Ihres KI-Modells 

**Ich habe mehrere KI-Modelle in einem Docker-Container. Wie verwende ich diese mit Live Video Analytics?** 

Die Lösungen unterscheiden sich je nach dem Kommunikationsprotokoll, das vom Rückschlussserver für die Kommunikation mit Live Video Analytics verwendet wird. In den folgenden Abschnitten wird die Funktionsweise der einzelnen Protokolle beschrieben.

*Verwenden des HTTP-Protokolls:*

* Einzelner Container (einzelne lvaExtension):  

   In Ihrem Rückschlussserver können Sie einen einzelnen Port, aber unterschiedliche Endpunkte für verschiedene KI-Modelle verwenden. Beispielsweise können Sie für ein Python-Beispiel verschiedene `route`n pro Modell wie folgt verwenden: 

   ```
   @app.route('/score/face_detection', methods=['POST']) 
   … 
   Your code specific to face detection model… 

   @app.route('/score/vehicle_detection', methods=['POST']) 
   … 
   Your code specific to vehicle detection model 
   … 
   ```

   Legen Sie dann in Ihrer Live Video Analytics-Bereitstellung beim Instanziieren von Diagrammen die URL des Rückschlussservers für jede Instanz wie folgt fest: 

   1\. Instanz: URL des Rückschlussservers = `http://lvaExtension:44000/score/face_detection`<br/>
   2\. Instanz: URL des Rückschlussservers = `http://lvaExtension:44000/score/vehicle_detection`  
   
    > [!NOTE]
    > Alternativ können Sie Ihre KI-Modelle auf unterschiedlichen Ports verfügbar machen und beim Instanziieren von Graphen aufrufen.  

* Mehrere Container: 

   Jeder Container wird mit einem anderen Namen bereitgestellt. Bisher haben wir in der Live Video Analytics-Dokumentation gezeigt, wie Sie eine Erweiterung mit dem Namen *lvaExtension* bereitstellen. Nun können Sie zwei unterschiedliche Container entwickeln, die jeweils dieselbe HTTP-Schnittstelle und somit auch denselben `/score`-Endpunkt aufweisen. Stellen Sie diese beiden Container mit unterschiedlichen Namen bereit, und stellen Sie sicher, dass beide an *anderen Ports* lauschen. 

   Beispielsweise lauscht ein Container mit dem Namen `lvaExtension1` an Port `44000`, der andere Container mit dem Namen `lvaExtension2` an Port `44001`. 

   In Ihrer Live Video Analytics-Topologie instanziieren Sie zwei Graphen mit unterschiedlichen Rückschluss-URLs, wie hier gezeigt: 

   Erste Instanz: URL des Rückschlussservers = `http://lvaExtension1:44001/score`    
   Zweite Instanz: URL des Rückschlussservers = `http://lvaExtension2:44001/score`
   
*Verwenden des gRPC-Protokolls:* 

* Mit dem Live Video Analytics-Modul 1.0 gibt es bei Verwendung eines allgemeinen RPC-Protokolls (gRPC, general-purpose Remote Procedure Call) nur die Möglichkeit, dass der gRPC-Server verschiedene KI-Modelle über verschiedene Ports verfügbar macht. In [diesem Codebeispiel](https://github.com/Azure/live-video-analytics/blob/master/MediaGraph/topologies/grpcExtensionOpenVINO/2.0/topology.json) stellt ein einzelner Port (44000) alle yolo-Modelle zur Verfügung. Theoretisch könnte der yolo-gRPC-Server umgeschrieben werden, um einige Modelle an Port 44000 verfügbar zu machen, andere an Port 45000 usw. 

* Mit dem Live Video Analytics-Modul 2.0 wird dem gRPC-Erweiterungsknoten eine neue Eigenschaft hinzugefügt. Diese **extensionConfiguration** Eigenschaft ist eine optionale Zeichenfolge, die als Teil des gRPC-Vertrags verwendet werden kann. Wenn Sie mehrere KI-Modelle in einem einzelnen Rückschlussserver gepackt haben, müssen Sie nicht für jedes KI-Modell einen Knoten verfügbar machen. Stattdessen können Sie als Erweiterungsanbieter für eine Graphinstanz mithilfe der **extensionConfiguration**-Eigenschaft festlegen, wie die verschiedenen KI-Modelle ausgewählt werden. Während der Ausführung übergibt Live Video Analytics diese Zeichenfolge an den Rückschlussserver, der sie zum Aufrufen des gewünschten KI-Modells verwenden kann. 

**Ich erstelle einen gRPC-Server um ein KI-Modell herum und möchte die Verwendung durch mehrere Kameras/Graphinstanzen unterstützen. Wie sollte ich meinen Server erstellen?** 

 Stellen Sie zunächst sicher, dass der Server mehrere Anforderungen gleichzeitig verarbeiten oder in parallelen Threads arbeiten kann. 

Beispielsweise wurde in einem der [Live Video Analytics-gRPC-Beispiele](https://github.com/Azure/live-video-analytics/blob/master/utilities/video-analysis/notebooks/Yolo/yolov3/yolov3-grpc-icpu-onnx/lvaextension/server/server.py) eine Standardanzahl paralleler Kanäle festgelegt: 

```
server = grpc.server(futures.ThreadPoolExecutor(max_workers=3)) 
```

In der obigen gRPC-Serverinstanziierung kann der Server nur drei Kanäle pro Kamera oder pro Graphtopologieinstanz gleichzeitig öffnen. Sie sollten nicht versuchen, mehr als drei Instanzen mit dem Server zu verbinden. Wenn Sie versuchen, mehr als drei Kanäle zu öffnen, bleiben Anforderungen ausstehend, bis ein vorhandener Kanal getrennt wird.  

Die obige gRPC-Serverimplementierung wird in den Python-Beispielen verwendet. Als Entwickler können Sie einen eigenen Server implementieren oder die vorherige Standardimplementierung verwenden, um die Workeranzahl zu erhöhen. Diese legen Sie auf die für Videofeeds verwendete Anzahl von Kameras fest. 

Wenn Sie mehrere Kameras einrichten und verwenden möchten, können Sie mehrere Graphtopologieinstanzen instanziieren, wobei jede auf denselben oder einen anderen Rückschlussserver zeigt (z. B. auf den im obigen Absatz genannten Server). 

**Ich möchte mehrere Upstreamframes empfangen können, bevor ich eine Rückschlussentscheidung treffe. Wie kann ich dies ermöglichen?** 

Die aktuellen [Standardbeispiele](https://github.com/Azure/live-video-analytics/tree/master/utilities/video-analysis) funktionieren im *zustandslosen* Modus. Sie behalten nicht den Status früherer Aufrufe oder die Aufrufer bei. Dies bedeutet, dass mehrere Topologieinstanzen denselben Rückschlussserver aufrufen können, der Server jedoch nicht unterscheiden kann, wer ihn aufruft oder welcher Zustand je Aufrufer gilt. 

*Verwenden des HTTP-Protokolls:*

Um den Zustand beizubehalten, ruft jeder Aufrufer oder jede Graphtopologieinstanz den Rückschlussserver mit einem für den Aufrufer eindeutigen HTTP-Abfrageparameter auf. Beispielsweise werden die Rückschlussserver-URLs für jede Instanz hier angezeigt:  

1\. Topologieinstanz = `http://lvaExtension:44000/score?id=1`<br/>
2\. Topologieinstanz = `http://lvaExtension:44000/score?id=2`

… 

Auf der Serverseite weiß die Bewertungsroute, wer den Aufruf durchführt. Wenn ID=1, kann der Zustand für diesen Aufrufer oder diese Graphtopologieinstanz getrennt beibehalten werden. Sie können dann die empfangenen Videoframes in einem Puffer aufbewahren. Verwenden Sie z. B. ein Array oder ein Wörterbuch mit einem DateTime-Schlüssel, dessen Wert der Frame ist. Anschließend können Sie den Server für die Verarbeitung (den Rückschluss) definieren, nachdem *x* Frames empfangen wurden. 

*Verwenden des gRPC-Protokolls:* 

Bei einer gRPC-Erweiterung ist jede Sitzung für einen einzelnen Kamerafeed vorgesehen, sodass keine ID bereitgestellt werden muss. Nun können Sie mit der extensionConfiguration-Eigenschaft die Videoframes in einem Puffer speichern und den Verarbeitungsserver (Rückschlussserver) definieren, nachdem *x* Frames empfangen wurden. 

**Führen alle ProcessMediaStreams in einem bestimmten Container dasselbe KI-Modell aus?** 

Nein. Das Starten/Beenden von Aufrufen durch den Endbenutzer bei einer Graphinstanz stellt eine Sitzung dar, oder möglicherweise wird eine Kameraverbindung getrennt bzw. erneut hergestellt. Das Ziel besteht darin, während des Videostreamings durch die Kamera eine Sitzung aufrechtzuerhalten. 

* Wenn zwei Kameras Videodaten zur Verarbeitung senden, werden zwei Sitzungen erstellt. 
* Wenn eine Kamera mit einem Graph verbunden ist, der über zwei gRPC-Erweiterungsknoten verfügt, werden zwei Sitzungen erstellt. 

Jede Sitzung ist eine Vollduplexverbindung zwischen Live Video Analytics und dem gRPC-Server, und jede Sitzung kann ein anderes Modell bzw. eine andere Pipeline aufweisen. 

> [!NOTE]
> Wenn eine Kamera die Verbindung trennt oder wiederherstellt und dabei für einen außerhalb der Toleranzgrenzen liegenden Zeitraum offline geschaltet wird, öffnet Live Video Analytics eine neue Sitzung mit dem gRPC-Server. Der Server muss den Zustand nicht über diese Sitzungen hinweg nachverfolgen. 

In Live Video Analytics wurde außerdem die Unterstützung mehrerer gRPC-Erweiterungen für eine einzelne Kamera in einer Graphinstanz hinzugefügt. Sie können diese gRPC-Erweiterungen verwenden, um die KI-Verarbeitung sequenziell oder parallel bzw. sogar in einer Kombination aus beidem auszuführen. 

> [!NOTE]
> Das parallele Ausführen mehrerer Erweiterungen wirkt sich auf Ihre Hardwareressourcen aus. Behalten Sie dies im Hinterkopf, wenn Sie die Hardware für Ihre Rechenanforderungen auswählen. 

**Was ist die maximale Anzahl gleichzeitiger ProcessMediaStreams?** 

Live Video Analytics wendet auf diese Anzahl keine Einschränkungen an.  

**Wie kann ich entscheiden, ob der Rückschlussserver CPU oder GPU bzw. einen anderen Hardwarebeschleuniger verwenden soll?** 

Ihre Entscheidung hängt von der Komplexität des entwickelten KI-Modells und der gewünschten Verwendung der CPU- und Hardwarebeschleunigung ab. Bei der Entwicklung des KI-Modells können Sie angeben, welche Ressourcen vom Modell verwendet werden sollen und welche Aktionen es durchführen soll. 

**Wie speichere ich nach der Verarbeitung Bilder mit Begrenzungsrahmen?** 

Derzeit stellen wir Begrenzungsrahmenkoordinaten nur als Rückschlussmeldungen bereit. Sie können einen benutzerdefinierten MJPEG-Streamer erstellen, der diese Nachrichten verwenden und die Videoframes mit den Begrenzungsrahmen überlagern kann.  

## <a name="grpc-compatibility"></a>GRPC-Kompatibilität 

**Woher weiß ich, welche Pflichtfelder der Mediendatenstrom-Deskriptor benötigt?** 

Jedes Feld, für das Sie keinen Wert angeben, erhält einen [von gRPC vorgegebenen Standardwert](https://developers.google.com/protocol-buffers/docs/proto3#default).  

Live Video Analytics verwendet Version *proto3* der Protokollpuffersprache. Alle Protokollpufferdaten, die von Live Video Analytics-Verträgen genutzt werden, sind in den [Protokollpufferdateien](https://github.com/Azure/live-video-analytics/tree/master/contracts/grpc) verfügbar. 

**Wie kann ich sicherstellen, dass ich die aktuellen Protokollpufferdateien verwende?** 

Sie können die aktuellen Protokollpufferdateien auf der [Website mit den Vertragsdateien](https://github.com/Azure/live-video-analytics/tree/master/contracts/grpc) abrufen. Wenn wir die Vertragsdateien aktualisieren, werden sie an diesem Ort angezeigt. Obwohl es keinen unmittelbaren Plan zum Aktualisieren der Protokolldateien gibt, suchen Sie nach dem Paketnamen am Anfang der Dateien, um die Version zu ermitteln. sollte aber wie folgt lauten: 

```
microsoft.azure.media.live_video_analytics.extensibility.grpc.v1 
```

Bei allen Updates dieser Dateien wird der „v-Wert“ am Ende des Namens heraufgesetzt. 

> [!NOTE]
> Da in Live Video Analytics die Version „proto3“ der Sprache verwendet wird, sind die Felder optional. Dies sorgt für Ab- und Aufwärtskompatibilität. 

**Welche gRPC-Features sind für die Verwendung mit Live Video Analytics verfügbar? Welche Features sind obligatorisch und welche optional?** 

Sie können alle serverseitigen gRPC-Funktionen verwenden, wenn der Protokollpuffervertrag (Protobuf) erfüllt ist. 

## <a name="monitoring-and-metrics"></a>Überwachung und Metriken

**Kann ich das Mediendiagramm am Edge mithilfe von Azure Event Grid überwachen?**

Ja. Sie können die Prometheus-Metriken nutzen und in Event Grid veröffentlichen. 

**Kann ich Azure Monitor verwenden, um Integrität, Metriken und Leistung meiner Mediendiagramme in der Cloud oder am Edge anzuzeigen?**

Ja, diese Vorgehensweise wird unterstützt. Weitere Informationen finden Sie in der [Übersicht über Azure Monitor-Metriken](../../azure-monitor/essentials/data-platform-metrics.md).

**Gibt es Tools, die das Überwachen des IoT Edge-Moduls für Media Services vereinfachen?**

Visual Studio Code unterstützt die Erweiterung „Azure IoT Tools“, mit der Sie die Endpunkte des LVAEdge-Moduls problemlos überwachen können. Sie können dieses Tool verwenden, um schnell mit der Überwachung des integrierten IoT Hub-Endpunkts auf „Ereignisse“ zu beginnen und die Rückschlussmeldungen anzuzeigen, die vom Edgegerät an die Cloud weitergeleitet werden. 

Darüber hinaus können Sie mit dieser Erweiterung den Modulzwilling für das LVAEdge-Modul und damit die Einstellungen des Mediengraph bearbeiten.

Weitere Informationen finden Sie im Artikel [Überwachung und Protokollierung](monitoring-logging.md).

## <a name="billing-and-availability"></a>Abrechnung und Verfügbarkeit

**Wie wird Live Video Analytics in IoT Edge abgerechnet?**

Ausführliche Informationen zur Abrechnung finden Sie unter [Media Services – Preise](https://azure.microsoft.com/pricing/details/media-services/).

## <a name="next-steps"></a>Nächste Schritte

[Schnellstart: Erste Schritte mit Live Video Analytics in IoT Edge](get-started-detect-motion-emit-events-quickstart.md)
