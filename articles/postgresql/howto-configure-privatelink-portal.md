---
title: Private Link – Azure-Portal – Azure Database for PostgreSQL – Einzelserver
description: Erfahren Sie, wie Sie einen privaten Link für die Azure Database for PostgreSQL konfigurieren können – Einzelserver des Azure-Portals
author: mksuni
ms.author: sumuth
ms.service: postgresql
ms.topic: how-to
ms.date: 01/09/2020
ms.openlocfilehash: 75defcf88128e1b100e403d74f76577525f0bdeb
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105642132"
---
# <a name="create-and-manage-private-link-for-azure-database-for-postgresql---single-server-using-portal"></a>Erstellen und Verwalten von Private Link for Azure Database for PostgreSQL – Einzelserver im Portal

Ein privater Endpunkt ist der grundlegende Baustein für Private Link in Azure. Mit ihm können Azure-Ressourcen wie virtuelle Computer (VMs) privat mit Private Link-Ressourcen kommunizieren.  In diesem Artikel erfahren Sie, wie Sie einen virtuellen Computer in einem Azure Virtual Network und einen Azure Database for PostgreSQL-Einzelserver mit einem privaten Azure-Endpunkt über das Azure-Portal erstellen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

> [!NOTE]
> Das Feature „Private Link“ ist nur für Azure Database for PostgreSQL-Server in den Tarifen „Universell“ oder „Arbeitsspeicheroptimiert“ verfügbar. Stellen Sie sicher, dass für den Datenbankserver einer dieser Tarife festgelegt wird.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure
Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="create-an-azure-vm"></a>Erstellen einer Azure-VM

In diesem Abschnitt erstellen Sie ein virtuelles Netzwerk und das Subnetz zum Hosten des virtuellen Computers, der für den Zugriff auf Ihre Private Link-Ressource verwendet wird (ein PostgreSQL-Server in Azure).

### <a name="create-the-virtual-network"></a>Erstellen des virtuellen Netzwerks
In diesem Abschnitt erstellen Sie ein virtuelles Netzwerk und das Subnetz zum Hosten des virtuellen Computers, der für den Zugriff auf Ihre Private Link-Ressource verwendet wird.

1. Wählen Sie oben links auf dem Bildschirm **Ressource erstellen** > **Netzwerk** > **Virtuelles Netzwerk** aus.
2. Geben Sie in **Virtuelles Netzwerk erstellen** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | Name | Geben Sie *MyVirtualNetwork* ein. |
    | Adressraum | Geben Sie *10.1.0.0/16* ein. |
    | Subscription | Wählen Sie Ihr Abonnement aus.|
    | Resource group | Wählen Sie **Neue erstellen** aus, geben Sie *myResourceGroup* ein, und wählen Sie **OK** aus. |
    | Standort | Wählen Sie **Europa, Westen** aus.|
    | Subnetzname | Geben Sie *mySubnet* ein. |
    | Subnetzadressbereich | Geben Sie *10.1.0.0/24* ein. |
    |||
3. Übernehmen Sie die übrigen Standardeinstellungen, und wählen Sie **Erstellen** aus.

### <a name="create-virtual-machine"></a>Erstellen eines virtuellen Computers

1. Wählen Sie oben links auf dem Bildschirm im Azure-Portal die Option **Ressource erstellen** > **Compute** > **Virtueller Computer** aus.

2. Geben Sie in **Virtuellen Computer erstellen – Grundlagen** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | **PROJEKTDETAILS** | |
    | Subscription | Wählen Sie Ihr Abonnement aus. |
    | Resource group | Wählen Sie **myResourceGroup** aus. Diese haben Sie im vorherigen Abschnitt erstellt.  |
    | **INSTANZDETAILS** |  |
    | Name des virtuellen Computers | Geben Sie *myVm* ein. |
    | Region | Wählen Sie **Europa, Westen** aus. |
    | Verfügbarkeitsoptionen | Übernehmen Sie den Standardwert **Keine Infrastrukturredundanz erforderlich**. |
    | Image | Wählen Sie **Windows Server 2019 Datacenter** aus. |
    | Size | Übernehmen Sie den Standardwert **Standard DS1 v2**. |
    | **ADMINISTRATORKONTO** |  |
    | Username | Geben Sie einen Benutzernamen Ihrer Wahl ein. |
    | Kennwort | Geben Sie das gewünschte Kennwort ein. Das Kennwort muss mindestens zwölf Zeichen lang sein und die [definierten Anforderungen an die Komplexität](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) erfüllen.|
    | Kennwort bestätigen | Geben Sie das Kennwort erneut ein. |
    | **REGELN FÜR EINGEHENDE PORTS** |  |
    | Öffentliche Eingangsports | Übernehmen Sie den Standardwert **Keine**. |
    | **SPAREN SIE GELD** |  |
    | Windows-Lizenz bereits vorhanden? | Übernehmen Sie den Standardwert **Nein**. |
    |||

1. Klicken Sie auf **Weiter: Datenträger**.

1. Übernehmen Sie unter **Virtuellen Computer erstellen – Datenträger** die Standardwerte, und wählen Sie **Weiter: Netzwerk**.

1. Wählen Sie in **Virtuellen Computer erstellen – Netzwerk** diese Informationen aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | Virtuelles Netzwerk | Übernehmen Sie den Standardwert **MyVirtualNetwork**.  |
    | Adressraum | Übernehmen Sie den Standardwert **10.1.0.0/24**.|
    | Subnet | Übernehmen Sie den Standardwert **mySubnet (10.1.0.0/24)** .|
    | Öffentliche IP-Adresse | Übernehmen Sie den Standardwert **(neu) myVm-ip**. |
    | Öffentliche Eingangsports | Wählen Sie **Ausgewählte Ports zulassen** aus. |
    | Eingangsports auswählen | Wählen Sie **HTTP** und **RDP** aus.|
    |||


1. Klicken Sie auf **Überprüfen + erstellen**. Sie werden zur Seite **Überprüfen und erstellen** weitergeleitet, auf der Azure Ihre Konfiguration überprüft.

1. Wenn die Meldung **Überprüfung erfolgreich** angezeigt wird, wählen Sie **Erstellen** aus.

> [!NOTE]
> In einigen Fällen befinden sich Azure Database for PostgreSQL und das VNET-Subnetz in unterschiedlichen Abonnements. In diesen Fällen müssen Sie folgende Konfigurationen sicherstellen:
> - Stellen Sie sicher, dass für beide Abonnements der Ressourcenanbieter **Microsoft.DBforPostgreSQL** registriert ist. Weitere Informationen finden Sie unter [Azure-Ressourcenanbieter und -typen][resource-manager-portal].

## <a name="create-an-azure-database-for-postgresql-single-server"></a>Erstellen einer Einzelservers für Azure Database for PostgreSQL

In diesem Abschnitt erstellen Sie einen Azure Database for PostgreSQL-Server in Azure. 

1. Wählen Sie links oben auf dem Bildschirm im Azure-Portal die Option **Ressource erstellen** > **Datenbanken** > **Azure Database for PostgreSQL** aus.

1. Wählen Sie in **Azure Database for PostgreSQL-Bereitstellungsoption** die Option **Einzelserver** aus, und geben Sie folgende Informationen an:

    | Einstellung | Wert |
    | ------- | ----- |
    | **Projektdetails** | |
    | Subscription | Wählen Sie Ihr Abonnement aus. |
    | Resource group | Wählen Sie **myResourceGroup** aus. Diese haben Sie im vorherigen Abschnitt erstellt.|
    | **Serverdetails** |  |
    |Servername  | Geben Sie *myserver* ein. Wenn dieser Name vergeben ist, erstellen Sie einen eindeutigen Namen.|
    | Administratorbenutzername| Geben Sie einen Administratornamen Ihrer Wahl ein. |
    | Kennwort | Geben Sie das gewünschte Kennwort ein. Das Kennwort muss mindestens acht Zeichen lang sein und die festgelegten Anforderungen erfüllen. |
    | Standort | Wählen Sie eine Azure-Region aus, in der sich Ihr PostgreSQL-Server befinden soll. |
    |Version  | Wählen Sie die gewünschte Datenbankversion des PostgreSQL-Servers aus.|
    | Compute + Speicher| Wählen Sie den für den Server erforderlichen Tarif basierend auf der Workload aus. |
    |||
 
7. Klicken Sie auf **OK**. 
8. Klicken Sie auf **Überprüfen + erstellen**. Sie werden zur Seite **Überprüfen und erstellen** weitergeleitet, auf der Azure Ihre Konfiguration überprüft. 
9. Wenn die Meldung „Überprüfung erfolgreich“ angezeigt wird, wählen Sie **Erstellen** aus. 
10. Wenn die Meldung „Überprüfung erfolgreich“ angezeigt wird, wählen Sie „Erstellen“ aus. 

## <a name="create-a-private-endpoint"></a>Erstellen eines privaten Endpunkts

In diesem Abschnitt erstellen Sie einen PostgreSQL-Server und fügen diesem einen privaten Endpunkt hinzu. 

1. Wählen Sie links oben auf dem Bildschirm im Azure-Portal die Option **Ressource erstellen** > **Netzwerk** > **Private Link** aus.
2. Wählen Sie unter **Private Link-Center – Übersicht** bei der Option **Private Verbindung mit einem Dienst herstellen** den Befehl **Starten** aus.

    :::image type="content" source="media/concepts-data-access-and-security-private-link/privatelink-overview.png" alt-text="Übersicht über Private Link":::

1. Geben Sie unter **Privaten Endpunkt erstellen – Grundlagen** die folgenden Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | **Projektdetails** | |
    | Subscription | Wählen Sie Ihr Abonnement aus. |
    | Resource group | Wählen Sie **myResourceGroup** aus. Diese haben Sie im vorherigen Abschnitt erstellt.|
    | **Instanzendetails** |  |
    | Name | Geben Sie *myPrivateEndpoint* ein. Wenn dieser Name vergeben ist, erstellen Sie einen eindeutigen Namen. |
    |Region|Wählen Sie **Europa, Westen** aus.|
    |||
5. Klicken Sie auf **Weiter: Ressource** aus.
6. Geben Sie unter **Privaten Endpunkt erstellen – Ressource** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    |Verbindungsmethode  | Wählen Sie das Herstellen einer Verbindung mit einer Azure-Ressource im eigenen Verzeichnis aus.|
    | Subscription| Wählen Sie Ihr Abonnement aus. |
    | Ressourcentyp | Wählen Sie **Microsoft.DBforPostgreSQL/servers** aus. |
    | Resource |Wählen Sie *myServer* aus.|
    |Zielunterressource |Wählen Sie *postgresqlServer* aus.|
    |||
7. Klicken Sie auf **Weiter: Konfiguration** aus.
8. Geben Sie unter **Privaten Endpunkt erstellen – Konfiguration** die folgenden Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    |**NETZWERK**| |
    | Virtuelles Netzwerk| Wählen Sie *MyVirtualNetwork* aus. |
    | Subnet | Wählen Sie *mySubnet* aus. |
    |**PRIVATE DNS-INTEGRATION**||
    |Integration in eine private DNS-Zone |Wählen Sie **Ja** aus. |
    |Private DNS-Zone |Wählen Sie *(New)privatelink.postgres.database.azure.com* aus. |
    |||

    > [!Note] 
    > Verwenden Sie die vordefinierte private DNS-Zone für Ihren Dienst, oder geben Sie Ihren bevorzugten DNS-Zonennamen an. Ausführliche Informationen finden Sie unter [DNS-Zonenkonfiguration für Azure-Dienste](../private-link/private-endpoint-dns.md).

1. Klicken Sie auf **Überprüfen + erstellen**. Sie werden zur Seite **Überprüfen und erstellen** weitergeleitet, auf der Azure Ihre Konfiguration überprüft. 
2. Wenn die Meldung **Überprüfung erfolgreich** angezeigt wird, wählen Sie **Erstellen** aus. 

    :::image type="content" source="media/concepts-data-access-and-security-private-link/show-postgres-private-link.png" alt-text="Erstellter Private Link":::

    > [!NOTE] 
    > Der FQDN in der DNS-Einstellung des Kunden wird nicht in die konfigurierte private IP-Adresse aufgelöst. Sie müssen eine DNS-Zone für den konfigurierten FQDN einrichten, wie [hier](../dns/dns-operations-recordsets-portal.md) gezeigt.

## <a name="connect-to-a-vm-using-remote-desktop-rdp"></a>Herstellen einer Verbindung mit einem virtuellen Computer mithilfe von Remotedesktop (RDP)


Stellen Sie nach der Erstellung von **myVm** über das Internet eine Verbindung mit diesem virtuellen Computer her: 

1. Geben Sie in der Suchleiste des Portals *myVm* ein.

1. Wählen Sie die Schaltfläche **Verbinden** aus. Nach dem Auswählen der Schaltfläche **Verbinden** wird **Verbindung mit virtuellem Computer herstellen** geöffnet.

1. Wählen Sie **RDP-Datei herunterladen** aus. Azure erstellt eine Remotedesktopprotokoll-Datei (*RDP*) und lädt sie auf Ihren Computer herunter.

1. Öffnen Sie die Datei *downloaded.rdp*.

    1. Wenn Sie dazu aufgefordert werden, wählen Sie **Verbinden** aus.

    1. Geben Sie den Benutzernamen und das Kennwort ein, den/das Sie beim Erstellen des virtuellen Computers angegeben haben.

        > [!NOTE]
        > Unter Umständen müssen Sie **Weitere Optionen** > **Anderes Konto verwenden** auswählen, um die Anmeldeinformationen anzugeben, die Sie beim Erstellen des virtuellen Computers eingegeben haben.

1. Klicken Sie auf **OK**.

1. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Wenn Sie eine Zertifikatwarnung erhalten, wählen Sie **Ja** oder **Weiter** aus.

1. Sobald der VM-Desktop angezeigt wird, minimieren Sie ihn, um zu Ihrem lokalen Desktop zurückzukehren.

## <a name="access-the-postgresql-server-privately-from-the-vm"></a>Privates Zugreifen auf den PostgreSQL-Server vom virtuellen Computer

1. Öffnen Sie auf dem Remotedesktop von *myVM* PowerShell.

2. Geben Sie `nslookup mydemopostgresserver.privatelink.postgres.database.azure.com` ein. 

    Sie erhalten eine Meldung wie die folgende:
    ```azurepowershell
    Server:  UnKnown
    Address:  168.63.129.16
    Non-authoritative answer:
    Name:    mydemopostgresserver.privatelink.postgres.database.azure.com
    Address:  10.1.3.4
    ```

3. Testen Sie die Verbindung über den privaten Link für den PostgreSQL-Server mit einem beliebigen verfügbaren Client. Im folgenden Beispiel wird für diesen Vorgang [Azure Data Studio](/sql/azure-data-studio/download) verwendet.

4. Geben Sie in **Neue Verbindung** die folgenden Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | Servertyp| Wählen Sie **PostgreSQL** aus.|
    | Servername| Wählen Sie *mydemopostgresserver.privatelink.postgres.database.azure.com* aus. |
    | Benutzername | Geben Sie einen Benutzernamen als username@servername ein, der während der Erstellung des PostgreSQL-Servers angegeben wird. |
    |Kennwort |Geben Sie ein während der Erstellung des PostgreSQL-Servers angegebenes Kennwort ein. |
    |SSL|Wählen Sie **Erforderlich** aus.|
    ||

5. Wählen Sie „Verbinden“ aus.

6. Durchsuchen Sie Datenbanken im linken Menü.

7. (Optional:) Erstellen Sie die PostgreSQL-Datenbank, oder fragen Sie sie ab.

8. Schließen Sie die Remotedesktopverbindung mit „myVm“.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn Sie den privaten Endpunkt, den PostgreSQL-Server und den virtuellen Computer nicht mehr benötigen, löschen Sie die Ressourcengruppe und alle darin enthaltenen Ressourcen:

1. Geben Sie oben im Portal die Zeichenfolge *myResourceGroup* ins Feld **Suchen** ein, und wählen Sie in den Suchergebnissen  *myResourceGroup* aus.
2. Wählen Sie die Option **Ressourcengruppe löschen**.
3. Geben Sie „myResourceGroup“ für **RESSOURCENGRUPPENNAMEN EINGEBEN** ein, und wählen Sie **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

In dieser Anleitung haben Sie einen virtuellen Computer in einem virtuellen Netzwerk, einen Azure Database for PostgreSQL-Einzelserver und einen privaten Endpunkt für den privaten Zugriff erstellt. Sie haben aus dem Internet eine Verbindung mit einem virtuellen Computer hergestellt und über Private Link sicher mit dem PostgreSQL-Server kommuniziert. Weitere Informationen zu privaten Endpunkten finden Sie unter [Was ist privater Endpunkt in Azure?](../private-link/private-endpoint-overview.md).

<!-- Link references, to text, Within this same GitHub repo. -->
[resource-manager-portal]: ../azure-resource-manager/management/resource-providers-and-types.md