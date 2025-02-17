---
title: 'Schnellstart: Erstellen eines virtuellen Azure Confidential Computing-Computers im Azure-Portal'
description: Machen Sie sich als Einstieg in Ihre Bereitstellungen damit vertraut, wie Sie im Azure-Portal schnell einen virtuellen Confidential Computing-Computer erstellen können.
author: JBCook
ms.service: virtual-machines
ms.subservice: workloads
ms.workload: infrastructure
ms.topic: quickstart
ms.date: 06/13/2021
ms.author: JenCook
ms.openlocfilehash: 8fb93b7697e2dd9077995572fc91b6e82a7d8512
ms.sourcegitcommit: 98308c4b775a049a4a035ccf60c8b163f86f04ca
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/30/2021
ms.locfileid: "113107216"
---
# <a name="quickstart-deploy-an-azure-confidential-computing-vm-in-the-azure-portal"></a>Schnellstart: Bereitstellen eines virtuellen Azure Confidential Computing-Computers im Azure-Portal

Steigen Sie in Azure Confidential Computing ein, indem Sie das Azure-Portal zum Erstellen eines virtuellen Computers (VM) verwenden, der auf Intel SGX basiert. Anschließend können Sie Enclave-Anwendungen ausführen.

Wir empfehlen Ihnen dieses Tutorial, wenn Sie an der Bereitstellung eines virtuellen Confidential Computing-Computers mit benutzerdefinierter Konfiguration interessiert sind. Andernfalls empfehlen wir Ihnen, den Artikel zum Thema [Virtueller Confidential Computing-Computer: Bereitstellungsschritte für den kommerziellen Microsoft-Marketplace](quick-create-marketplace.md) zu lesen.


## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, [erstellen Sie ein Konto](https://azure.microsoft.com/pricing/purchase-options/pay-as-you-go/), bevor Sie beginnen.

> [!NOTE]
> Mit kostenlosen Testkonten ist kein Zugriff auf die virtuellen Computer möglich, die in diesem Tutorial verwendet werden. Führen Sie ein Upgrade auf ein Abonnement mit nutzungsbasierter Zahlung durch.


## <a name="sign-in-to-azure"></a>Anmelden bei Azure

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. Wählen Sie oben die Option **Ressource erstellen** aus.

1. Wählen Sie im Bereich **Marketplace** links die Option **Compute** aus.

1. Wählen Sie **Virtueller Computer** aus.

    ![Bereitstellen einer VM](media/quick-create-portal/compute-virtual-machine.png)

1. Wählen Sie auf der Startseite des virtuellen Computers die Option **Erstellen** aus.


## <a name="configure-a-confidential-computing-virtual-machine"></a>Konfigurieren eines virtuellen Confidential Computing-Computers

1. Wählen Sie auf der Registerkarte **Grundlagen** Ihr **Abonnement** und Ihre **Ressourcengruppe** aus.

1. Geben Sie im Feld **Name des virtuellen Computers** einen Namen für Ihre neue VM ein.

1. Geben Sie folgende Werte ein bzw. wählen diese aus:

   * **Region**: Wählen Sie die für Sie geeignete Azure-Region aus.

        > [!NOTE]
        > Virtuelle Confidential Computing-Computer werden nur auf spezieller Hardware ausgeführt, die in bestimmten Regionen verfügbar ist. Die neuesten verfügbaren Regionen für VMs der DCsv2-Serie sind unter [Verfügbare Regionen](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines) angegeben.

1. Konfigurieren Sie das Betriebssystemimage, das Sie für Ihren virtuellen Computer verwenden möchten.

    * **Image auswählen**: Wählen Sie für dieses Tutorial „Ubuntu 18.04 LTS“ aus. Sie können auch „Windows Server 2019“, „Windows Server 2016“ oder „Ubuntu 16.04 LTS“ auswählen. Wenn Sie sich für eines dieser Images entscheiden, werden Sie in diesem Tutorial entsprechend umgeleitet.
    
    * **Image für Gen 2 wechseln**: Virtuelle Confidential Computing-Computer werden nur unter Images vom Typ [Generation 2](../virtual-machines/generation-2.md) ausgeführt. Stellen Sie sicher, dass das ausgewählte Image den Typ „Generation 2“ aufweist. Klicken Sie oben auf die Registerkarte **Erweitert**, auf der Sie den virtuellen Computer konfigurieren. Scrollen Sie nach unten bis zum Abschnitt „VM-Generation“. Wählen Sie „Gen 2“ aus, und wechseln Sie anschließend zurück zur Registerkarte **Grundlagen**.
    

        ![Erweitert (Registerkarte)](media/quick-create-portal/advanced-tab-virtual-machine.png)


        ![VM-Generation](media/quick-create-portal/gen2-virtual-machine.png)

    * **Zur grundlegenden Konfiguration wechseln**: Wechseln Sie zurück zur Registerkarte **Grundlagen**, indem Sie oben die Navigationselemente verwenden.

1. Wählen Sie in der Größenauswahl einen virtuellen Computer mit Confidential Computing-Funktionen aus, indem Sie die Option **Größe ändern** verwenden. Klicken Sie in der VM-Größenauswahl auf **Alle Filter löschen**. Wählen Sie die Option **Filter hinzufügen** und **Familie** als Filtertyp aus. Wählen Sie anschließend nur **Vertrauliche Computeressource** aus.

    ![VMs der DCsv2-Serie](media/quick-create-portal/dcsv2-virtual-machines.png)

    > [!TIP]
    > Es sollten die Größen **DC1s_v2**, **DC2s_v2**, **DC4s_V2** und **DC8_v2** aufgeführt sein. Dies sind die einzigen VM-Größen, für die Confidential Computing derzeit unterstützt wird. [Weitere Informationen](virtual-machine-solutions.md)

1. Geben Sie die folgenden Informationen ein:

   * **Authentifizierungstyp:** Wählen Sie **Öffentlicher SSH-Schlüssel** aus, wenn Sie eine Linux-VM erstellen. 

        > [!NOTE]
        > Für die Authentifizierung können Sie einen öffentlichen SSH-Schlüssel oder ein Kennwort verwenden. SSH ist sicherer. Wie Sie einen SSH-Schlüssel generieren, erfahren Sie unter [Erstellen und Verwenden eines SSH-Schlüsselpaars (öffentlich und privat) für virtuelle Linux-Computer in Azure](../virtual-machines/linux/mac-create-ssh-keys.md).

    * **Benutzername**: Geben Sie den Administratornamen für die VM ein.

    * **Öffentlicher SSH-Schlüssel**: Geben Sie Ihren öffentlichen RSA-Schlüssel ein (falls zutreffend).
    
    * **Kennwort**: Geben Sie Ihr Kennwort für die Authentifizierung ein (falls zutreffend).

    * **Öffentliche Eingangsports**: Wählen Sie die Option **Ausgewählte Ports zulassen** und dann in der Liste **Öffentliche Eingangsports hinzufügen** die Optionen **SSH (22)** und **HTTP (80)** aus. Wählen Sie bei der Bereitstellung einer Windows-VM die Optionen **HTTP (80)** und **RDP (3389)** aus.  

    >[!Note]
    > Das Zulassen von RDP-/SSH-Ports wird für Produktionsbereitstellungen nicht empfohlen.  

     ![Eingehende Ports](media/quick-create-portal/inbound-port-virtual-machine.png)


1. Nehmen Sie Änderungen auf der Registerkarte **Datenträger** vor.

   * Bei Auswahl eines virtuellen Computers vom Typ **DC1s_v2**, **DC2s_v2** oder **DC4s_V2** sollten Sie entweder **SSD Standard** oder **SSD Premium** als Datenträgertyp auswählen. 
   * Wählen Sie für einen virtuellen Computer vom Typ **DC8_v2** die Option **SSD Standard** als Datenträgertyp aus.

1. Nehmen Sie auf den folgenden Registerkarten die gewünschten Änderungen an den Einstellungen vor, oder übernehmen Sie die Standardeinstellungen.

    * **Netzwerk**
    * **Verwaltung**
    * **Gastkonfiguration**
    * **Tags**

1. Klicken Sie auf **Überprüfen + erstellen**.

1. Klicken Sie im Bereich **Bewerten + erstellen** auf **Erstellen**.

> [!NOTE]
> Fahren Sie im nächsten Abschnitt mit diesem Tutorial fort, wenn Sie eine Linux-VM bereitgestellt haben. Gehen Sie bei Bereitstellung einer Windows-VM so vor, dass Sie [diese Schritte für die Verbindungsherstellung mit Ihrer Windows-VM ausführen](../virtual-machines/windows/connect-logon.md) und dann das [OE SDK unter Windows installieren](https://github.com/openenclave/openenclave/blob/master/docs/GettingStartedDocs/install_oe_sdk-Windows.md).


## <a name="connect-to-the-linux-vm"></a>Herstellen einer Verbindung mit dem virtuellen Linux-Computer

Wenn Sie bereits über eine BASH-Shell verfügen, verwenden Sie den Befehl **ssh**, um eine Verbindung mit dem virtuellen Azure-Computer herzustellen. Ersetzen Sie im folgenden Befehl den VM-Benutzernamen und die IP-Adresse, um eine Verbindung mit Ihrem virtuellen Linux-Computer herzustellen.

```bash
ssh azureadmin@40.55.55.555
```

Sie finden die öffentliche IP-Adresse Ihrer VM im Azure-Portal im Abschnitt mit der „Übersicht“ für Ihre VM.

:::image type="content" source="media/quick-create-portal/public-ip-virtual-machine.png" alt-text="IP-Adresse im Azure-Portal":::

Wenn Sie mit Windows arbeiten und nicht über eine BASH-Shell verfügen, installieren Sie einen SSH-Client (beispielsweise PuTTY).

1. [Laden Sie PuTTY herunter, und installieren Sie es.](https://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

1. Führen Sie PuTTY aus.

1. Geben Sie im Konfigurationsbildschirm von PuTTY die öffentliche IP-Adresse Ihres virtuellen Computers ein.

1. Wählen Sie **Öffnen** aus, und geben Sie Ihren Benutzernamen und Ihr Kennwort ein.

Weitere Informationen zum Herstellen einer Verbindung mit virtuellen Linux-Computern finden Sie unter [Erstellen einer Linux-VM mit dem Azure-Portal](../virtual-machines/linux/quick-create-portal.md).

> [!NOTE]
> Falls eine PuTTY-Sicherheitswarnung angezeigt wird, dass der Hostschlüssel des Servers nicht in der Registrierung zwischengespeichert wird, wählen Sie eine der folgenden Optionen aus. Wenn Sie diesem Host vertrauen, wählen Sie **Ja** aus, um den Schlüssel dem Cache von PuTTy hinzuzufügen und die Verbindungsherstellung fortzusetzen. Wenn Sie die Verbindung nur einmal herstellen möchten, ohne den Schlüssel dem Cache hinzuzufügen, wählen Sie **Nein** aus. Wenn Sie diesem Host nicht vertrauen, wählen Sie **Abbrechen** aus, um die Verbindungsherstellung abzubrechen.

## <a name="intel-sgx-drivers"></a>Intel SGX-Treiber

> [!NOTE]
> Intel SGX-Treiber sind bereits Teil der Ubuntu & Windows Azure-Katalogimages. Es ist keine spezielle Installation der Treiber erforderlich. Optional können Sie auch die vorhandenen Treiber aktualisieren, die in den Images enthalten sind, indem Sie die [Intel SGX DCAP-Treiberliste](https://01.org/intel-software-guard-extensions/downloads) hinzuziehen.

## <a name="optional-testing-enclave-apps-built-with-open-enclave-sdk-oe-sdk"></a>Optional: Testen von Enclave-Apps, die mit dem Open Enclave SDK (OE SDK) erstellt wurden <a id="Install"></a>

Befolgen Sie die Schritt-für-Schritt-Anleitung für die Installation des [OE SDK](https://github.com/openenclave/openenclave) auf Ihrem virtuellen Computer der DCsv2-Serie, auf dem ein Image vom Typ „Ubuntu 18.04 LTS Gen 2“ ausgeführt wird. 

Wenn Ihr virtueller Computer unter Ubuntu 18.04 LTS Gen 2 ausgeführt wird, gilt die [Installationsanleitung für Ubuntu 18.04](https://github.com/openenclave/openenclave/blob/master/docs/GettingStartedDocs/install_oe_sdk-Ubuntu_18.04.md).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Ressourcengruppe, virtueller Computer und alle zugehörigen Ressourcen nicht mehr benötigt werden, können Sie sie löschen. 

Wählen Sie die Ressourcengruppe für den virtuellen Computer und dann **Löschen** aus. Bestätigen Sie dann den Namen der Ressourcengruppe, um das Löschen der Ressourcen abzuschließen.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen virtuellen Confidential Computing-Computer bereitgestellt und das Open Enclave SDK installiert. Weitere Informationen zu virtuellen Confidential Computing-Computern in Azure finden Sie unter [Lösungen für virtuelle Computer](virtual-machine-solutions.md). 

Machen Sie sich damit vertraut, wie Sie Confidential Computing-Anwendungen entwickeln, indem Sie mit den Beispielen zum Open Enclave SDK auf GitHub fortfahren. 

> [!div class="nextstepaction"]
> [Entwickeln von Open Enclave SDK-Beispielen](https://github.com/openenclave/openenclave/blob/master/samples/README.md)
