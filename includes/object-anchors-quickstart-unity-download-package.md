---
author: craigktreasure
ms.service: azure-object-anchors
ms.topic: include
ms.date: 02/18/2021
ms.author: crtreasu
ms.openlocfilehash: ada83d6263ef033208200d810c53c5f045acc9a7
ms.sourcegitcommit: ed7376d919a66edcba3566efdee4bc3351c57eda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2021
ms.locfileid: "105105422"
---
Der nächste Schritt besteht darin, das Azure Object Anchors-Paket für Unity herunterzuladen.

# <a name="download-with-web-browser"></a>[Download über Webbrowser](#tab/unity-package-web-ui)

Suchen Sie [hier](https://aka.ms/aoa/unity-sdk/package) nach dem Azure Object Anchors-Paket für Unity. Wählen Sie die gewünschte Version aus, und laden Sie das Paket über die Schaltfläche **Download** herunter.

# <a name="download-with-npm"></a>[Download mit npm](#tab/unity-package-npm)

Für diesen Schritt ist erforderlich, dass <a href="https://www.npmjs.com/get-npm" target="_blank">npm</a> installiert und verfügbar ist.

Führen Sie den folgenden Befehl aus. Ersetzen Sie dabei `<version_number>` durch die Version der Azure Object Anchors-Instanz, die Sie herunterladen möchten:

```bash
npm pack com.microsoft.azure.object-anchors.runtime@<version_number> --registry https://pkgs.dev.azure.com/aipmr/MixedReality-Unity-Packages/_packaging/Unity-packages/npm/registry/
```

> [!NOTE]
> Führen Sie zum Auflisten der verfügbaren Versionen des Azure Object Anchors-Pakets den folgenden Befehl aus:
>
> ```bash
> npm view com.microsoft.azure.object-anchors.runtime --registry https://pkgs.dev.azure.com/aipmr/MixedReality-Unity-Packages/_packaging/Unity-packages/npm/registry/ versions
> ```

Das Azure Object Anchors-Paket wird in den Ordner heruntergeladen, in dem Sie den Befehl ausgeführt haben.

# <a name="install-with-mixed-reality-feature-tool-beta"></a>[Installation mit dem Mixed Reality-Featuretool (Betaversion)](#tab/unity-package-mixed-reality-feature-tool)

Fahren Sie mit dem nächsten Schritt fort. Das <a a href="/windows/mixed-reality/develop/unity/welcome-to-mr-feature-tool" target="_blank">Mixed Reality-Featuretool</a> verwenden Sie in einem späteren Schritt.

---