---
title: 'Einbinden einer Azure-NFS-Dateifreigabe (Vorschau): Azure Files'
description: Erfahren Sie, wie Sie eine NFS (Network File System)-Freigabe einbinden.
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 07/01/2021
ms.author: rogarana
ms.subservice: files
ms.custom: references_regions
ms.openlocfilehash: 8f3565f05fc04a74e761b1070f0374677703d225
ms.sourcegitcommit: f4e04fe2dfc869b2553f557709afaf057dcccb0b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2021
ms.locfileid: "113225251"
---
# <a name="how-to-mount-an-nfs-file-share-preview"></a>Einbinden einer NFS-Dateifreigabe (Vorschau)

[Azure Files](storage-files-introduction.md) ist das benutzerfreundliche Clouddateisystem von Microsoft. Azure-Dateifreigaben können in Linux-Distributionen mit dem SMB- oder NFS-Protokoll (Server Message Block/Network File System) (Vorschau) eingebunden werden. Dieser Artikel konzentriert sich auf die Einbindung mit NFS. Ausführliche Informationen zum Einbinden mit SMB finden Sie unter [Verwenden von Azure Files mit Linux](storage-how-to-use-files-linux.md). Ausführliche Informationen zu den einzelnen verfügbaren Protokollen finden Sie unter [Azure-Dateifreigabeprotokolle](storage-files-planning.md#available-protocols).

## <a name="limitations"></a>Einschränkungen

[!INCLUDE [files-nfs-limitations](../../../includes/files-nfs-limitations.md)]

### <a name="regional-availability"></a>Regionale Verfügbarkeit

[!INCLUDE [files-nfs-regional-availability](../../../includes/files-nfs-regional-availability.md)]

## <a name="prerequisites"></a>Voraussetzungen

- [Erstellen einer NFS-Freigabe](storage-files-how-to-create-nfs-shares.md)

    > [!IMPORTANT]
    > Der Zugriff auf NFS-Freigaben ist nur über vertrauenswürdige Netzwerke möglich. Verbindungen mit Ihrer NFS-Freigabe müssen von einer der folgenden Quellen stammen:

- Verwenden Sie eine der folgenden Netzwerklösungen:
    - Entweder [Erstellen eines privaten Endpunkts](storage-files-networking-endpoints.md#create-a-private-endpoint) (empfohlen) oder [Einschränken des Zugriffs auf den öffentlichen Endpunkt](storage-files-networking-endpoints.md#restrict-public-endpoint-access)
    - [Konfigurieren eines P2S-VPN (Point-to-Site) unter Linux zur Verwendung mit Azure Files](storage-files-configure-p2s-vpn-linux.md)
    - [Konfigurieren eines Site-to-Site-VPN zur Verwendung mit Azure Files](storage-files-configure-s2s-vpn.md)
    - Konfigurieren von [ExpressRoute](../../expressroute/expressroute-introduction.md)

## <a name="disable-secure-transfer"></a>Deaktivieren der sicheren Übertragung

1. Melden Sie sich beim Azure-Portal an, und greifen Sie auf das Speicherkonto zu, das die erstellte NFS-Freigabe enthält.
1. Wählen Sie **Konfiguration** aus.
1. Wählen Sie für **Sichere Übertragung erforderlich** die Einstellung **Deaktiviert** aus.
1. Wählen Sie **Speichern** aus.

    :::image type="content" source="media/storage-files-how-to-mount-nfs-shares/storage-account-disable-secure-transfer.png" alt-text="Screenshot mit dem Konfigurationsbildschirm des Speicherkontos und der deaktivierten Option „Sichere Übertragung erforderlich“":::

## <a name="mount-an-nfs-share"></a>Einbinden einer NFS-Freigabe

1. Nachdem die Dateifreigabe erstellt wurde, wählen Sie die Freigabe und dann **Verbindungsherstellung über Linux** aus.
1. Geben Sie den zu verwendenden Einbindungspfad ein, und kopieren Sie dann das Skript.
1. Stellen Sie eine Verbindung mit dem Client her, und verwenden Sie das bereitgestellte Einbindungsskript.

    :::image type="content" source="media/storage-files-how-to-create-mount-nfs-shares/mount-nfs-file-share-script.png" alt-text="Screenshot: Blatt zum Verbinden einer Dateifreigabe":::

Sie haben nun Ihre NFS-Dateifreigabe eingebunden.

### <a name="validate-connectivity"></a>Überprüfen der Konnektivität

Wenn bei der Installation ein Fehler aufgetreten ist, wurde Ihr privater Endpunkt möglicherweise nicht ordnungsgemäß eingerichtet, oder es kann darauf nicht zugegriffen werden. Ausführliche Informationen zum Bestätigen der Konnektivität finden Sie im Abschnitt [Überprüfen der Konnektivität](storage-files-networking-endpoints.md#verify-connectivity) des Artikels „Networking Endpoints“ (Endpunkte im Netzwerk).

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Azure Files finden Sie in unserem Artikel [Planung für eine Azure Files-Bereitstellung](storage-files-planning.md).
- Wenn Probleme auftreten, finden Sie unter [Behandeln von Problemen mit Azure NFS-Dateifreigaben](storage-troubleshooting-files-nfs.md) weitere Informationen.