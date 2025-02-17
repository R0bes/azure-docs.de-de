---
title: Herstellen einer Verbindung zu einer Linux-VM in Azure Lab Services | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Remotedesktop für virtuelle Linux-Computer in einem Lab in Azure Lab Services verwenden.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 04a86ba98df3e1600ac95d19e690815515eb4c6d
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122346165"
---
# <a name="connect-to-linux-virtual-machines-in-a-classroom-lab-of-azure-lab-services"></a>Herstellen einer Verbindung zu virtuellen Linux-Computern in einem Unterrichts-Lab in Azure Lab Services
In diesem Artikel erfahren Sie, wie Kursteilnehmer in einem Lab eine Verbindung zu einem virtuellen Linux-Computer (Virtual Machine, VM) herstellen können. Dazu werden folgende Mittel verwendet:
- Ein SSH-Terminal (Secure Shell-Protokoll)
- Die grafische Benutzeroberfläche (Graphical User Interface, GUI) eines Remotedesktops

> [!IMPORTANT] 
> SSH wird automatisch so konfiguriert, dass sowohl die Kursteilnehmer als auch der Dozent ohne zusätzliches Setup eine SSH-Verbindung zu virtuellen Linux-Computer herstellen können. Wenn die Kursteilnehmer die Verbindung jedoch über die GUI eines Remotedesktops herstellen müssen, muss der Dozent möglicherweise zusätzliche Einrichtungsschritte durchführen.  Weitere Informationen finden Sie unter [Aktivieren von Remotedesktop für virtuelle Linux-Computer in einem Lab in Azure Lab Services](how-to-enable-remote-desktop-linux.md).

## <a name="connect-to-the-student-vm-using-ssh"></a>Herstellen einer Verbindung zu einer Kursteilnehmer-VM über SSH

1. Wenn sich ein Kursteilnehmer direkt am Labs-Portal (`https://labs.azure.com`) anmeldet oder einen Registrierungslink (`https://labs.azure.com/register/<registrationCode>`) verwendet, wird eine Kachel für jedes Lab angezeigt, auf das der Kursteilnehmer Zugriff hat. 
   
1. Schalten Sie auf der Kachel die Schaltfläche zum Starten des virtuellen Computers um, um den virtuellen Computer zu starten, wenn er sich im Status „Beendet“ befindet. 

2. Wählen Sie **Verbinden**. Es werden zwei Optionen für die Verbindungsherstellung mit dem virtuellen Computer angezeigt: **SSH** und **RDP**.

    ![Virtueller Computer eines Kursteilnehmers: Verbindungsoptionen](./media/how-to-enable-remote-desktop-linux/student-vm-connect-options.png)

3. Wenn Sie die Option **SSH** auswählen, wird das Dialogfeld **Verbindung mit VM herstellen** angezeigt:  

    ![SSH-Verbindungszeichenfolge](./media/how-to-enable-remote-desktop-linux/ssh-connection-string.png)

4. Klicken Sie neben dem Textfeld auf die Schaltfläche **Kopieren**, um die Informationen zur SSH-Verbindung in die Zwischenablage zu kopieren. 

5. Speichern Sie die SSH-Verbindungsinformationen z. B. in TextPad, damit Sie sie im nächsten Schritt verwenden können.

6. Stellen Sie über ein SSH-Terminal (wie [PuTTY](https://www.putty.org/)) eine Verbindung zu Ihrer VM her.

## <a name="connect-to-the-student-vm-using-gui-remote-desktop"></a>Herstellen einer Verbindung zu einer Kursteilnehmer-VM über die GUI eines Remotedesktops
Der Dozent kann VMs so konfigurieren, dass die Kursteilnehmer auch über die GUI eines Remotedesktops eine Verbindung herstellen können.  In diesem Fall müssen Kursteilnehmer bei ihrem Dozenten erfragen, ob sie den **Microsoft-Remotedesktop (RDP)** oder die Clientanwendung **X2Go** für die Verbindung mit ihrer VM verwenden sollen.  Mit beiden Anwendungen können Kursteilnehmer eine Remoteverbindung mit ihrer VM herstellen und den grafischen Linux-Desktop auf ihrem lokalen Computer aufrufen.

> [!WARNING]
>  Es wird empfohlen, eine andere grafische Desktopumgebung als [GNOME](https://www.gnome.org/) zu verwenden.  Sie sollten die Installation von GNOME auf Lab-VMs vermeiden, da GNOME einen Konflikt mit dem Azure Linux-Agent aufweist, der erforderlich ist, damit die VMs in Azure Lab Services ordnungsgemäß funktionieren.  Es wird beispielsweise empfohlen, eine grafische Desktopumgebung wie XFCE zu verwenden.

### <a name="connect-to-the-student-vm-using-microsoft-remote-desktop-rdp"></a>Herstellen einer Verbindung zu einer Kursteilnehmer-VM über den Microsoft-Remotedesktop
Kursteilnehmer können mit dem Microsoft-Remotedesktop eine Verbindung mit ihren Linux-VMs herstellen, nachdem der Dozent das Lab mit RDP- und GUI-Paketen für eine grafische Linux-Desktopumgebung eingerichtet hat (z. B. XFCE, MATE usw.). So wird eine Verbindung hergestellt: 

1. Vergewissern Sie sich auf der Kachel Ihrer VM, dass diese ausgeführt wird, und klicken Sie auf **Verbinden**. Es werden zwei Optionen für die Verbindungsherstellung mit dem virtuellen Computer angezeigt: **SSH** und **RDP**.

    ![Virtueller Computer eines Kursteilnehmers: Verbindungsoptionen](./media/how-to-enable-remote-desktop-linux/student-vm-connect-options.png)
2. Wählen Sie die Option **RDP** aus.  Nachdem die RDP-Datei auf Ihren Computer heruntergeladen wurde, speichern Sie sie auf Ihrer VM.

3. Wenn Sie die Verbindung von einem Windows-Computer aus herstellen, ist der Client des Microsoft-Remotedesktops in der Regel bereits installiert und konfiguriert.  Daher müssen Sie lediglich auf die RDP-Datei klicken, um die Remotesitzung zu öffnen und zu starten.

    Falls Sie die Verbindung von einem Mac oder einem Chromebook aus herstellen, verwenden Sie stattdessen die folgenden Anleitungen:
   - [Herstellen einer Verbindung zu einer VM mithilfe des Remotedesktopprotokolls auf einem Mac](connect-virtual-machine-mac-remote-desktop.md)
   - [Herstellen einer Verbindung zu einer VM mithilfe des Remotedesktopprotokolls auf einem Chromebook](connect-virtual-machine-chromebook-remote-desktop.md)

### <a name="connect-to-the-student-vm-using-x2go"></a>Herstellen einer Verbindung zu einer Kursteilnehmer-VM über X2Go
Kursteilnehmer können mit X2Go eine Verbindung mit ihren Linux-VMs herstellen, nachdem der Dozent das Lab mit X2Go und den GUI-Paketen für eine grafische Linux-Desktopumgebung eingerichtet hat (z. B. XFCE, MATE usw.).

Kursteilnehmer müssen bei ihrem Dozenten erfragen, welche grafische Linux-Desktopumgebung er installiert hat.  Diese Information wird in den nächsten Schritten benötigt, um die Verbindung mithilfe des X2Go-Clients herzustellen.

1. Installieren Sie den [X2Go-Client](https://wiki.x2go.org/doku.php/doc:installation:x2goclient) auf Ihrem lokalen Computer.

1. Befolgen Sie die Anleitung im [ersten Abschnitt](how-to-use-remote-desktop-linux-student.md#connect-to-the-student-vm-using-ssh), um die SSH-Verbindungsinformationen für Ihre VM zu kopieren.  Sie benötigen diese Informationen, um eine Verbindung mithilfe des X2Go-Clients herzustellen.

1. Wenn Sie die SSH-Verbindungsinformationen haben, müssen Sie den X2Go-Client öffnen und auf **Session** > **New Session** (Sitzung > Neue Sitzung) klicken.
   ![Neue Sitzung in X2Go erstellen](./media/how-to-use-classroom-lab/x2go-new-session.png)

1. Geben Sie im Bereich **Session Preferences** (Sitzungseinstellungen) die Werte gemäß den SSH-Verbindungsinformationen ein.  Angenommen, die Verbindungsinformationen sehen in etwa wie folgt aus:

    ```bash
    ssh -p 12345 student@ml-lab-00000000-0000-0000-0000-000000000000.eastus2.cloudapp.azure.com
    ```

    Diesem Beispiel entsprechend müssen Sie die folgenden Werte eingeben:

   - **Session name** (Sitzungsname): Geben Sie einen Namen an, z. B. den Namen Ihrer VM.
   - **Host:** die ID Ihrer VM, beispielsweise **`ml-lab-00000000-0000-0000-0000-000000000000.eastus2.cloudapp.azure.com`**
   - **Login** (Anmeldung): der Benutzername für Ihre VM, beispielsweise **student**
   - **SSH port** (SSH-Port): der eindeutige Port, der Ihrer VM zugewiesen ist, beispielsweise **12345**
   - **Session type** (Sitzungstyp): Wählen Sie die grafische Linux-Desktopumgebung aus, die Ihr Dozenten für Ihre VM konfiguriert hat.  Sie müssen diese Informationen von Ihrem Dozenten erhalten.

    Klicken Sie abschließend auf **OK**, um die Sitzung zu erstellen.

    ![Sitzungseinstellungen in X2Go](./media/how-to-use-classroom-lab/x2go-session-preferences.png)

1.  Klicken Sie im rechten Bereich auf Ihre Sitzung.

    ![Neue Sitzung in X2Go starten](./media/how-to-use-classroom-lab/x2go-start-session.png)

    > [!NOTE] 
    > Wenn Sie mit einer Benachrichtigung wie der folgenden dazu aufgefordert werden, klicken Sie auf **yes** (Ja), um Ihr Kennwort einzugeben: **The authenticity of host '[`00000000-0000-0000-0000-000000000000.eastus2.cloudapp.eastus.cloudapp.azure.com`]:12345' can't be established.  ECDSA key fingerprint is SHA256:00000000000000000000000000000000000000000000.Are you sure you want to continue connecting (yes/no)?** (Die Authentizität von Host '[`00000000-0000-0000-0000-000000000000.eastus2.cloudapp.eastus.cloudapp.azure.com`]:12345' kann nicht bestätigt werden. Der Fingerabdruck des ECDSA-Schlüssels lautet SHA256:00000000000000000000000000000000000000000000. Möchten Sie die Verbindungsherstellung wirklich fortsetzen [Ja/Nein]?)

2. Geben Sie nach entsprechender Aufforderung Ihr Kennwort ein, und klicken Sie auf **OK**.  Sie werden nun remote mit der GUI-Desktopumgebung Ihrer VM verbunden.

## <a name="next-steps"></a>Nächste Schritte
Informationen dazu, wie die Funktion für eine Remotedesktopverbindung für virtuelle Linux-Computer in einem Classroom-Lab aktiviert werden kann, finden Sie unter [Aktivieren von Remotedesktop für virtuelle Linux-Computer](how-to-enable-remote-desktop-linux.md). 

