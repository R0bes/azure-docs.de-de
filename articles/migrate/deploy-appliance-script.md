---
title: Einrichten einer Azure Migrate-Appliance mit einem Skript
description: Erfahren Sie, wie Sie eine Azure Migrate-Appliance mit einem Skript einrichten.
ms.topic: how-to
author: Vikram1988
ms.author: vibansa
ms.manager: abhemraj
ms.date: 03/18/2021
ms.openlocfilehash: bcd455590e804ad337f25afbd38d729f03ef3dc4
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122355647"
---
# <a name="set-up-an-appliance-with-a-script"></a>Einrichten einer Appliance mit einem Skript

Befolgen Sie die Anleitungen in diesem Artikel, um eine [Azure Migrate-Appliance](./migrate-appliance-architecture.md) mithilfe eines PowerShell-Skripts für Folgendes bereitzustellen:
- Ermittlung, Bewertung und Replikation ohne Agent von Servern, die in einer VMware-Umgebung ausgeführt werden
- Ermittlung und Bewertung von Servern, die in einer Hyper-V-Umgebung ausgeführt werden.

Sie können die Appliance für Server mit VMware und Hyper-V mithilfe eines Skripts bereitstellen oder eine Vorlage (OVA/VHD) nutzen, die Sie aus dem Azure-Portal herunterladen. Die Verwendung eines Skripts ist sinnvoll, wenn Sie mit der heruntergeladenen Vorlage keine Appliance erstellen können.

- Wenn Sie eine Vorlage verwenden möchten, befolgen Sie die Anleitungen in den Tutorials für [VMware](./tutorial-discover-vmware.md) und [Hyper-V](./tutorial-discover-hyper-v.md).
- Für die Einrichtung einer Appliance für physische Server kann ausschließlich ein Skript verwendet werden. Folgen Sie den Anweisungen in [diesem Artikel](how-to-set-up-appliance-physical.md).
- Das Einrichten einer Appliance in einer Azure Government Cloud ist nur mit einem Skript möglich. Folgen Sie den Anweisungen in [diesem Artikel](deploy-appliance-script-government.md).

## <a name="prerequisites"></a>Voraussetzungen

Sie können das Skript verwenden, um die Azure Migrate-Appliance auf einem vorhandenen Server in Ihrer VMware- oder Hyper-V-Umgebung bereitzustellen.

- Der Server, der die Appliance hostet, muss die folgenden Hardware- und Betriebssystemanforderungen erfüllen:

Szenario | Requirements (Anforderungen)
--- | ---
VMware | Windows Server 2016 mit 32 GB Arbeitsspeicher, acht vCPUs, ungefähr 80 GB Speicherplatz auf dem Datenträger.
Hyper-V | Windows Server 2016 mit 16 GB Arbeitsspeicher, acht vCPUs, ungefähr 80 GB Speicherplatz auf dem Datenträger.

- Der Server benötigt auch einen externen virtuellen Switch. Er benötigt eine statische oder dynamische IP-Adresse. 
- Überprüfen Sie vor der Bereitstellung der Appliance ihre detaillierten Anforderungen für [VMware](migrate-appliance.md#appliance---vmware) und [Hyper-V](migrate-appliance.md#appliance---hyper-v).
- Wenn Sie das Skript auf einem Server ausführen, auf dem die Azure Migrate-Appliance bereits eingerichtet ist, können Sie die vorhandene Konfiguration bereinigen und eine neue Appliance mit der gewünschten Konfiguration einrichten. Wenn Sie das Skript ausführen, erhalten Sie eine Benachrichtigung wie unten gezeigt:

    ![Einrichten der Appliance mit der gewünschten Konfiguration](./media/deploy-appliance-script/script-reconfigure-appliance.png)

## <a name="set-up-the-appliance-for-vmware"></a>Einrichten der Appliance für VMware

1. Um die Appliance einzurichten, laden Sie die ZIP-Datei namens „AzureMigrateInstaller.zip“ entweder aus dem Portal oder von [hier](https://go.microsoft.com/fwlink/?linkid=2116601) herunter.
1. Extrahieren Sie den Inhalt auf dem Server, auf dem Sie die Appliance bereitstellen möchten.
1. Führen Sie das PowerShell-Skript zum Starten des Appliancekonfigurations-Managers aus.
1. Richten Sie die Appliance ein, und führen die Erstkonfiguration aus.

### <a name="verify-security"></a>Überprüfen der Sicherheit

Vergewissern Sie sich vor der Bereitstellung, dass die gezippte Datei sicher ist.

1. Öffnen Sie auf dem Server, auf den Sie die Datei heruntergeladen haben, ein Administratorbefehlsfenster.
2. Führen Sie den folgenden Befehl aus, um den Hash für die gezippte Datei zu generieren:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Beispielverwendung: ```C:\>CertUtil -HashFile C:\Users\administrator\Desktop\AzureMigrateInstaller.zip SHA256 ```
3.  Überprüfen Sie die aktuelle Applianceversion und die Hashwerte:

    **Download** | **Hashwert**
    --- | ---
    [Aktuelle Version](https://go.microsoft.com/fwlink/?linkid=2116601) | b4668be44c05836bf0f2ac1c8b1f48b7a9538afcf416c5212c7190629e3683b2

> [!NOTE]
> Dasselbe Skript kann verwendet werden, um die VMware-Appliance für die öffentliche Azure- oder Azure Government Cloud einzurichten.

### <a name="run-the-script"></a>Führen Sie das Skript aus.

1. Extrahieren Sie die gezippte Datei in einem Ordner auf dem Server, der die Appliance hostet.  Führen Sie das Skript nicht auf einem Server mit einer vorhandenen Azure Migrate-Appliance aus.
2. Starten Sie PowerShell auf dem oben genannten Server mit Administratorberechtigungen (erhöhten Rechten).
3. Ändern Sie das PowerShell-Verzeichnis in den Ordner, in den die Inhalte der gezippten Datei extrahiert wurden, die Sie heruntergeladen haben.
4. Führen Sie das Skript mit dem Namen **AzureMigrateInstaller.ps1** aus, indem Sie den folgenden Befehl ausführen:

    
    ``` PS C:\Users\administrator\Desktop\AzureMigrateInstaller> .\AzureMigrateInstaller.ps1 ```

5. Treffen Sie eine Auswahl aus den Szenario-, Cloud- und Konnektivitätsoptionen, um eine Appliance mit der gewünschten Konfiguration bereitzustellen. Die unten gezeigte Auswahl richtet beispielsweise eine Appliance für die Ermittlung, Bewertung und Migration von **Servern in Ihrer VMware-Umgebung** für ein Azure Migrate-Projekt mit **Standardkonnektivität _(öffentlicher Endpunkt)_** in der **öffentlichen Azure-Cloud** ein.

    :::image type="content" source="./media/deploy-appliance-script/script-vmware-default-inline.png" alt-text="Screenshot: Einrichten einer VMware-Appliance mit der gewünschten Konfiguration." lightbox="./media/deploy-appliance-script/script-vmware-default-expanded.png":::

6. Das Installationsskript führt folgende Schritte aus:

 - Installation von Agents und einer Webanwendung.
 - Installation von Windows-Rollen, darunter beispielsweise Windows-Aktivierungsdienst, IIS und PowerShell ISE.
 - Download und Installation eines wiederbeschreibbaren IIS-Moduls.
 - Aktualisierung eines Registrierungsschlüssels (HKLM) mit dauerhaften Einstellungsdetails für Azure Migrate.
 - Erstellung der folgenden Dateien in diesem Pfad:
    - **Konfigurationsdateien**: %Programdata%\Microsoft Azure\Config
    - **Protokolldateien**: %Programdata%\Microsoft Azure\Logs

Nach der erfolgreichen Ausführung des Skripts wird der Appliancekonfigurations-Manager automatisch gestartet.

> [!NOTE]
> Bei Problemen können Sie zum Troubleshooting unter „C:\ProgramData\Microsoft Azure\Logs\AzureMigrateScenarioInstaller_<em>Zeitstempel</em>.log“ auf die Skriptprotokolle zugreifen.

### <a name="verify-access"></a>Überprüfen des Zugriffs

Stellen Sie sicher, dass die Appliance eine Verbindung mit Azure-URLs für die [öffentliche](migrate-appliance.md#public-cloud-urls) Cloud herstellen kann.

## <a name="set-up-the-appliance-for-hyper-v"></a>Einrichten der Appliance für Hyper-V

1. Um die Appliance einzurichten, laden Sie die ZIP-Datei namens „AzureMigrateInstaller.zip“ entweder aus dem Portal oder von [hier](https://go.microsoft.com/fwlink/?linkid=2116657) herunter.
1. Extrahieren Sie den Inhalt auf dem Server, auf dem Sie die Appliance bereitstellen möchten.
1. Führen Sie das PowerShell-Skript zum Starten des Appliancekonfigurations-Managers aus.
1. Richten Sie die Appliance ein, und führen die Erstkonfiguration aus.

### <a name="verify-security"></a>Überprüfen der Sicherheit

Vergewissern Sie sich vor der Bereitstellung, dass die gezippte Datei sicher ist.

1. Öffnen Sie auf dem Server, auf den Sie die Datei heruntergeladen haben, ein Administratorbefehlsfenster.
2. Führen Sie den folgenden Befehl aus, um den Hash für die gezippte Datei zu generieren:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Beispielverwendung: ```C:\>CertUtil -HashFile C:\Users\administrator\Desktop\AzureMigrateInstaller.zip SHA256 ```
3.  Überprüfen Sie die aktuelle Applianceversion und die Hashwerte:

    **Download** | **Hashwert**
    --- | ---
    [Aktuelle Version](https://go.microsoft.com/fwlink/?linkid=2116657) | b4668be44c05836bf0f2ac1c8b1f48b7a9538afcf416c5212c7190629e3683b2

> [!NOTE]
> Dasselbe Skript kann verwendet werden, um die Hyper-V-Appliance für die öffentliche Azure- oder Azure Government Cloud einzurichten.

### <a name="run-the-script"></a>Führen Sie das Skript aus.

1. Extrahieren Sie die gezippte Datei in einem Ordner auf dem Server, der die Appliance hostet.  Führen Sie das Skript nicht auf einem Server mit einer vorhandenen Azure Migrate-Appliance aus.
2. Starten Sie PowerShell auf dem oben genannten Server mit Administratorberechtigungen (erhöhten Rechten).
3. Ändern Sie das PowerShell-Verzeichnis in den Ordner, in den die Inhalte der gezippten Datei extrahiert wurden, die Sie heruntergeladen haben.
4. Führen Sie das Skript mit dem Namen **AzureMigrateInstaller.ps1** aus, indem Sie den folgenden Befehl ausführen:

    
    ``` PS C:\Users\administrator\Desktop\AzureMigrateInstaller> .\AzureMigrateInstaller.ps1 ```

5. Treffen Sie eine Auswahl aus den Szenario-, Cloud- und Konnektivitätsoptionen, um eine Appliance mit der gewünschten Konfiguration bereitzustellen. Die unten gezeigte Auswahl richtet beispielsweise eine Appliance für die Ermittlung und Bewertung von **Servern in Ihrer Hyper-V-Umgebung** für ein Azure Migrate-Projekt mit **Standardkonnektivität _(öffentlicher Endpunkt)_** in der **öffentlichen Azure-Cloud** ein.

    :::image type="content" source="./media/deploy-appliance-script/script-hyperv-default-inline.png" alt-text="Screenshot: Einrichten einer Hyper-V-Appliance mit der gewünschten Konfiguration." lightbox="./media/deploy-appliance-script/script-hyperv-default-expanded.png":::

6. Das Installationsskript führt folgende Schritte aus:

    - Installation von Agents und einer Webanwendung.
    - Installation von Windows-Rollen, darunter beispielsweise Windows-Aktivierungsdienst, IIS und PowerShell ISE.
    - Download und Installation eines wiederbeschreibbaren IIS-Moduls.
    - Aktualisierung eines Registrierungsschlüssels (HKLM) mit dauerhaften Einstellungsdetails für Azure Migrate.
    - Erstellung der folgenden Dateien in diesem Pfad:
        - **Konfigurationsdateien**: %Programdata%\Microsoft Azure\Config
        - **Protokolldateien**: %Programdata%\Microsoft Azure\Logs

Nach der erfolgreichen Ausführung des Skripts wird der Appliancekonfigurations-Manager automatisch gestartet.

> [!NOTE]
> Bei Problemen können Sie zum Troubleshooting unter „C:\ProgramData\Microsoft Azure\Logs\AzureMigrateScenarioInstaller_<em>Zeitstempel</em>.log“ auf die Skriptprotokolle zugreifen.

### <a name="verify-access"></a>Überprüfen des Zugriffs

Stellen Sie sicher, dass die Appliance eine Verbindung mit Azure-URLs für die [öffentliche](migrate-appliance.md#public-cloud-urls) Cloud herstellen kann.

## <a name="next-steps"></a>Nächste Schritte

Nach dem Bereitstellen der Appliance müssen Sie die Erstkonfiguration ausführen und die Appliance beim Projekt registrieren.

- Richten Sie die Appliance für [VMware](how-to-set-up-appliance-vmware.md#4-configure-the-appliance) ein.
- Richten Sie die Appliance für [Hyper-V](how-to-set-up-appliance-hyper-v.md#configure-the-appliance) ein.
