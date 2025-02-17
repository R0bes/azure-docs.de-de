---
title: Azure Virtual Desktop PowerShell – Azure
description: Informationen zum Behandeln von Problemen mit PowerShell beim Einrichten einer Azure Virtual Desktop-Umgebung.
author: Heidilohr
ms.topic: troubleshooting
ms.date: 06/05/2020
ms.author: helohr
ms.custom: devx-track-azurepowershell
manager: femila
ms.openlocfilehash: aae1f58eb9fc5e006254f6fdfd620c057e6d6fc3
ms.sourcegitcommit: 9339c4d47a4c7eb3621b5a31384bb0f504951712
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2021
ms.locfileid: "113762518"
---
# <a name="azure-virtual-desktop-powershell"></a>Azure Virtual Desktop PowerShell

>[!IMPORTANT]
>Dieser Inhalt gilt für Azure Virtual Desktop mit Azure Virtual Desktop-Objekten für Azure Resource Manager. Wenn Sie Azure Virtual Desktop (klassisch) ohne Azure Resource Manager-Objekte verwenden, finden Sie weitere Informationen in [diesem Artikel](./virtual-desktop-fall-2019/troubleshoot-powershell-2019.md).

Verwenden Sie diesen Artikel, um Fehler und Probleme bei der Verwendung von PowerShell mit Azure Virtual Desktop zu beheben. Weitere Informationen zu Remote Desktop Services PowerShell finden Sie unter [Azure Virtual Desktop PowerShell](/powershell/windows-virtual-desktop/overview).

## <a name="provide-feedback"></a>Feedback geben

In der [Azure Virtual Desktop Tech Community](https://techcommunity.microsoft.com/t5/azure-virtual-desktop/bd-p/AzureVirtualDesktopForum) können Sie sich mit dem Produktteam und aktiven Communitymitgliedern über den Azure Virtual Desktop-Dienst austauschen.

## <a name="powershell-commands-used-during-azure-virtual-desktop-setup"></a>Während der Installation von Azure Virtual Desktop verwendete PowerShell-Befehle

Dieser Abschnitt listet PowerShell-Befehle auf, die typischerweise beim Einrichten von Azure Virtual Desktop verwendet werden, und bietet Lösungen für Probleme, die bei der Verwendung dieser Befehle auftreten können.

### <a name="error-new-azroleassignment-the-provided-information-does-not-map-to-an-ad-object-id"></a>Error: New-AzRoleAssignment: Die angegebenen Informationen stimmen mit keiner AD-Objekt-ID überein.

```powershell
New-AzRoleAssignment -SignInName "admins@contoso.com" -RoleDefinitionName "Desktop Virtualization User" -ResourceName "0301HP-DAG" -ResourceGroupName 0301RG -ResourceType 'Microsoft.DesktopVirtualization/applicationGroups'
```

**Ursache:** Der durch den Parameter *-SignInName* angegebene Benutzer kann nicht im Azure Active Directory gefunden werden, das der Azure Virtual Desktop-Umgebung zugeordnet ist.

**Behebung:** Stellen Sie Folgendes sicher.

- Der Benutzer muss mit Azure Active Directory synchronisiert werden.
- Der Benutzer ist nicht mit B2C (Business-to-Consumer) oder B2B (Business-to-Business) Commerce verknüpft.
- Die Azure Virtual Desktop-Umgebung ist an dem richtigen Azure Active Directory zugeordnet.

### <a name="error-new-azroleassignment-the-client-with-object-id-does-not-have-authorization-to-perform-action-over-scope-code-authorizationfailed"></a>Error: New-AzRoleAssignment: „Der Client mit der Objekt-ID hat keine Berechtigung zum Ausführen der Aktion über Bereich (Code: AuthorizationFailed)“

**Ursache 1:** Das verwendete Konto hat keine Besitzerberechtigungen für das Abonnement.

**Behebung 1:** Ein Benutzer mit Besitzerberechtigungen muss die Rollenzuweisung ausführen. Alternativ muss der Benutzer der Rolle „Benutzerzugriffsadministrator“ zugewiesen werden, um einen Benutzer einer Anwendungsgruppe zuzuweisen.

**Ursache 2:** Das verwendete Konto hat Besitzerrechte, ist aber nicht Teil des Azure Active Directory der Umgebung oder verfügt nicht über Berechtigungen zum Abfragen des Azure Active Directory, in dem sich der Benutzer befindet.

**Behebung 2:** Ein Benutzer mit Active Directory-Berechtigungen muss die Rollenzuweisung ausführen.

### <a name="error-new-azwvdhostpool----the-location-is-not-available-for-resource-type"></a>Error: New-AzWvdHostPool: Der Speicherort ist für den Ressourcentyp nicht verfügbar.

```powershell
New-AzWvdHostPool_CreateExpanded: The provided location 'southeastasia' is not available for resource type 'Microsoft.DesktopVirtualization/hostpools'. List of available regions for the resource type is 'eastus,eastus2,westus,westus2,northcentralus,southcentralus,westcentralus,centralus'.
```

Ursache: Azure Virtual Desktop unterstützt die Auswahl des Speicherorts von Hostpools, Anwendungsgruppen und Arbeitsbereichen, um Dienstmetadaten an bestimmten Orten zu speichern. Ihre Optionen sind darauf beschränkt, wo diese Funktion verfügbar ist. Dieser Fehler bedeutet, dass das Feature nicht an dem von Ihnen ausgewählten Speicherort verfügbar ist.

Behebung: In der Fehlermeldung wird eine Liste der unterstützten Regionen veröffentlicht. Verwenden Sie stattdessen eine der unterstützten Regionen.

### <a name="error-new-azwvdapplicationgroup-must-be-in-same-location-as-host-pool"></a>Error: New-AzWvdApplicationGroup muss sich am gleichen Speicherort wie der Hostpool befinden

```powershell
New-AzWvdApplicationGroup_CreateExpanded: ActivityId: e5fe6c1d-5f2c-4db9-817d-e423b8b7d168 Error: ApplicationGroup must be in same location as associated HostPool
```

**Ursache:** Der Speicherort stimmt nicht überein. Alle Hostpools, Anwendungsgruppen und Arbeitsbereiche verfügen über einen Speicherort zum Speichern von Dienstmetadaten. Alle Objekte, die Sie erstellen, die einander zugeordnet sind, müssen sich am gleichen Speicherort befinden. Wenn sich ein Hostpool beispielsweise in `eastus` befindet, müssen Sie auch die Anwendungsgruppen in `eastus`erstellen. Wenn Sie einen Arbeitsbereich erstellen, in dem diese Anwendungsgruppen registriert werden, muss sich dieser Arbeitsbereich ebenfalls in `eastus` befinden.

**Behebung:** Rufen Sie den Speicherort ab, in dem der Hostpool erstellt wurde, und weisen Sie dann die zu erstellende Anwendungsgruppe dem gleichen Speicherort zu.

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über die Problembehandlung von Azure Virtual Desktop und die Eskalationspfade finden Sie unter [Problembehandlung: Übersicht, Feedback und Support](troubleshoot-set-up-overview.md).
- Informationen zur Problembehandlung beim Einrichten einer Azure Virtual Desktop-Umgebung und von Hostpools finden Sie unter [Umgebungs- und Hostpoolerstellung](troubleshoot-set-up-issues.md).
- Informationen zur Problembehandlung bei der Konfiguration einer VM in Azure Virtual Desktop finden Sie unter [Konfiguration von Sitzungshost-VMs](troubleshoot-vm-configuration.md).
- Informationen zur Behebung von Problemen bei Azure Virtual Desktop-Clientverbindungen finden Sie unter [Azure Virtual Desktop – Clientverbindungen](troubleshoot-service-connection.md).
- Informationen zur Behebung von Problemen bei Remotedesktop-Clients finden Sie unter [Problembehandlung für den Remotedesktop-Client](troubleshoot-client.md).
- Weitere Informationen zum Dienst finden Sie unter [Azure Virtual Desktop-Umgebung](environment-setup.md).
- Informationen zur Überwachung von Aktionen finden Sie unter [Überwachen von Vorgängen mit Resource Manager](../azure-resource-manager/management/view-activity-logs.md).
- Weitere Informationen zu Aktionen zum Bestimmen von Fehlern während der Bereitstellung finden Sie unter [Anzeigen von Bereitstellungsvorgängen mit dem Azure-Portal](../azure-resource-manager/templates/deployment-history.md).
