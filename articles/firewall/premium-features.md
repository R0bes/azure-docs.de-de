---
title: Azure Firewall Premium-Features
description: Azure Firewall Premium ist ein verwalteter, cloudbasierter Netzwerksicherheitsdienst, der Ihre Azure Virtual Network-Ressourcen schützt.
author: vhorne
ms.service: firewall
services: firewall
ms.topic: conceptual
ms.date: 08/10/2021
ms.author: victorh
ms.custom: references_regions
ms.openlocfilehash: 2a76ee1c5009091247d9629bdbb0edc0b8c8a217
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122339594"
---
# <a name="azure-firewall-premium-features"></a>Azure Firewall Premium-Features

:::image type="content" source="media/premium-features/icsa-cert-firewall-small.png" alt-text="ICSA-Zertifizierungslogo" border="false"::::::image type="content" source="media/premium-features/pci-logo.png" alt-text="PCI-Zertifizierungslogo" border="false":::


 Azure Firewall Premium ist eine Firewall der nächsten Generation mit Funktionen, die für streng vertrauliche und regulierte Umgebungen erforderlich sind.

:::image type="content" source="media/premium-features/premium-overview.png" alt-text="Azure Firewall Premium: Übersichtsdiagramm":::

Azure Firewall Premium verwendet die Firewallrichtlinie, eine globale Ressource, die zur zentralen Verwaltung Ihrer Firewalls mit Azure Firewall Manager verwendet werden kann. Ab dieser Version sind alle neuen Features nur noch über die Firewallrichtlinie konfigurierbar. Firewallregeln (klassisch) werden weiterhin unterstützt und können zur Konfiguration bestehender Standardfirewallfeatures verwendet werden.  Die Firewallrichtlinie kann unabhängig oder mit Azure Firewall Manager verwaltet werden. Eine Firewallrichtlinie, die mit einer einzelnen Firewall verbunden ist, wird nicht berechnet.

Azure Firewall Premium umfasst die folgenden Features:

- **TLS-Inspektion** – Entschlüsselt den ausgehenden Datenverkehr, verarbeitet die Daten, verschlüsselt sie dann und sendet sie an das Ziel.
- **IDPS** – Ein System zur Erkennung und Verhinderung von Eindringversuchen in Netzwerke (IDPS) ermöglicht es Ihnen, Netzwerkaktivitäten auf schädliche Aktivitäten zu überwachen, Informationen über diese Aktivitäten zu protokollieren, sie zu melden und optional zu versuchen, sie zu blockieren.
- **URL-Filterung** – Erweitert die FQDN-Filterfunktion von Azure Firewall, um eine gesamte URL zu berücksichtigen. Beispiel: `www.contoso.com/a/c` anstelle von `www.contoso.com`.
- **Webkategorien** – Administratoren können den Benutzerzugriff auf Websitekategorien wie Gaming- oder Social Media-Websites zulassen oder verweigern.


## <a name="tls-inspection"></a>TLS-Überprüfung

Azure Firewall Premium terminiert ausgehende und Ost-West-TLS-Verbindungen. Die eingehende TLS-Inspektion wird mit [Azure Application Gateway](../web-application-firewall/ag/ag-overview.md) unterstützt und ermöglicht eine End-to-End-Verschlüsselung. Azure Firewall übernimmt die erforderlichen Mehrwertsicherheitsfunktionen und verschlüsselt den Datenverkehr erneut, der an das ursprüngliche Ziel gesendet wird.

> [!TIP]
> TLS 1.0 und 1.1 sind veraltet und werden nicht mehr unterstützt. Bei den Versionen TLS 1.0 und 1.1 von TLS/Secure Sockets Layer (SSL) wurde ein Sicherheitsrisiko festgestellt. Sie funktionieren aus Gründen der Abwärtskompatibilität zwar noch, werden jedoch nicht empfohlen. Migrieren Sie so schnell wie möglich zu TLS 1.2.

Weitere Informationen zu den Anforderungen für das Zertifikat von Zwischenzertifizierungsstellen für Azure Firewall Premium finden Sie unter [Zertifikate von Azure Firewall Premium](premium-certificates.md).

## <a name="idps"></a>IDPS

Ein System zur Erkennung und Verhinderung von Eindringversuchen in Netzwerke (IDPS) ermöglicht es Ihnen, Ihr Netzwerk auf schädliche Aktivitäten zu überwachen, Informationen über diese Aktivitäten zu protokollieren, sie zu melden und optional zu versuchen, sie zu blockieren. 

Azure Firewall Premium bietet signaturbasiertes IDPS, um eine schnelle Erkennung von Angriffen zu ermöglichen, indem nach bestimmten Mustern gesucht wird, z. B. nach Bytesequenzen im Netzwerkdatenverkehr oder nach bekannten schädlichen Anweisungssequenzen, die von Malware verwendet werden. Die IDPS-Signaturen gelten sowohl für Datenverkehr auf Anwendungsebene als auch für Datenverkehr auf Netzwerkebene (Ebenen 4 bis 7), sie werden vollständig verwaltet und kontinuierlich aktualisiert. IDPS kann auf eingehenden Datenverkehr, Datenverkehr zwischen Spokes (Ost-West) und ausgehenden Datenverkehr angewendet werden.

Die Signaturen/Regelsätze in Azure Firewall bieten Folgendes:
- Schwerpunkt auf der Erstellung von Fingerabdrücken von tatsächlicher Schadsoftware, Command-and-Control, Exploitkits und schädlichen Aktivitäten in der Praxis, die herkömmliche Präventionsmethoden übersehen
- Über 55.000 Regeln in mehr als 50 Kategorien.
    - Zu diesen Kategorien gehören unter anderem Schadsoftware, Command-and-Control, DoS-Angriffe, Botnets, Informationsereignisse, Exploits, Sicherheitsrisiken, SCADA-Netzwerkprotokolle und Exploitkitaktivitäten.
- Jeden Tag werden zwischen 20 und mehr als 40 neue Regeln veröffentlicht.
- Niedrige False-Positive-Bewertung dank Verwendung einer modernen Sandbox für Schadsoftware und einer globalen Feedbackschleife für das Sensornetzwerk

Mit IDPS können Sie Angriffe auf alle Ports und Protokolle für nicht verschlüsselten Datenverkehr erkennen. Wenn jedoch HTTPS-Datenverkehr überprüft werden muss, kann Azure Firewall seine TLS-Inspektionsfunktion nutzen, um den Datenverkehr zu entschlüsseln und schädliche Aktivitäten besser zu erkennen.  

Mit der IDPS-Umgehungsliste können Sie den Datenverkehr zu den in der Umgehungsliste angegebenen IP-Adressen, Bereichen und Subnetzen nicht filtern.

Sie können auch Signaturregeln verwenden, wenn der IDPS-Modus auf **Warnung** festgelegt ist, Sie aber bestimmte Signaturen, einschließlich des zugehörigen Datenverkehrs, blockieren möchten. In diesem Fall können Sie neue Signaturregeln hinzufügen, indem Sie den Modus für die TLS-Inspektion auf **Ablehnen** festlegen.


## <a name="url-filtering"></a>URL-Filterung

Die URL-Filterung erweitert die FQDN-Filterfunktion von Azure Firewall, um eine gesamte URL zu berücksichtigen. Beispiel: `www.contoso.com/a/c` anstelle von `www.contoso.com`.  

Die URL-Filterung kann sowohl auf HTTP- als auch auf HTTPS-Datenverkehr angewendet werden. Wenn der HTTPS-Datenverkehr überprüft wird, kann Azure Firewall Premium die Funktion „TLS-Inspektion“ nutzen, um den Datenverkehr zu entschlüsseln und die Ziel-URL zu extrahieren, um zu überprüfen, ob der Zugriff gestattet ist. Die TLS-Inspektion erfordert eine Aktivierung auf der Anwendungsregelebene. Sobald diese Funktion aktiviert ist, können Sie URLs für die Filterung mit HTTPS verwenden. 

## <a name="web-categories"></a>Webkategorien

Mithilfe von Webkategorien können Administratoren den Benutzerzugriff auf Websitekategorien, z. B. Glücksspiel- oder Social Media-Websites, zulassen oder verweigern. Webkategorien sind auch in Azure Firewall Standard enthalten, aber in Azure Firewall Premium sind sie noch weiter optimiert. Im Gegensatz zu den Funktionen für Webkategorien in der Standard-SKU, bei denen der Abgleich der Kategorie anhand des FQDN erfolgt, werden die Kategorien bei der Premium-SKU anhand der gesamten URL abgeglichen (sowohl HTTP- als auch HTTPS-Datenverkehr). 

Wenn Azure Firewall z. B. eine HTTPS-Anforderung für `www.google.com/news` abfängt, wird die folgende Kategorisierung erwartet: 

- Firewall Standard: Nur der Teil mit dem vollqualifizierten Domänennamen (FQDN) wird untersucht, und `www.google.com` wird als *Suchmaschine* kategorisiert. 

- Firewall Premium: Die gesamte URL wird untersucht, sodass `www.google.com/news` als *News* kategorisiert wird.

Die Kategorien werden basierend auf dem Schweregrad in **Haftung**, **Hohe Bandbreite**, **Geschäftliche Nutzung**, **Produktivitätsverlust**, **Allgemeines Surfen** und **Nicht kategorisiert** unterteilt. Eine ausführliche Beschreibung der Webkategorien finden Sie unter [Azure Firewall-Webkategorien](web-categories.md).

### <a name="web-category-logging"></a>Protokollierung von Webkategorien
Sie können Datenverkehr, der nach **Webkategorien** gefiltert wurde, in den Anwendungsprotokollen anzeigen. Das Feld **Webkategorien** wird nur angezeigt, wenn es in den Firewallrichtlinien-Anwendungsregeln explizit konfiguriert wurde. Wenn Sie beispielsweise keine Regel definiert haben, die *Suchmaschinen* explizit ablehnt, und ein Benutzer anfordert, zu „www.bing.com“ zu wechseln, wird nur eine Standardablehnungsmeldung und keine einer Webkategorien-Meldung angezeigt. Dies liegt daran, dass die Webkategorie nicht explizit konfiguriert wurde.

### <a name="category-exceptions"></a>Kategorieausnahmen

Sie können Ausnahmen für Ihre Webkategorisierungsregeln erstellen. Erstellen Sie innerhalb der Regelsammlungsgruppe eine separate Sammlung mit Zulassungs- oder Verweigerungsregeln mit einer höheren Priorität. Sie können beispielsweise eine Regelsammlung konfigurieren, die `www.linkedin.com` mit Priorität 100 zulässt, und eine Regelsammlung, die **Soziale Netzwerke** mit der Priorität 200 verweigert. Dadurch wird eine Ausnahme für die vordefinierte Webkategorie **Soziale Netzwerke** erstellt.

 ## <a name="supported-regions"></a>Unterstützte Regionen

Azure Firewall mit Premium-Tarif wird aktuell in den folgenden Regionen unterstützt:

- Australien, Mitte (Öffentlich/Australien)
- Australien, Mitte 2 (Öffentlich/Australien)
- Australien, Osten (Öffentlich/Australien)
- Australien, Südosten (Öffentlich/Australien)
- Brasilien, Süden (Öffentlich/Brasilien)
- Brasilien, Südosten (Öffentlich/Brasilien)
- Kanada, Mitte (Öffentlich/Kanada)
- Kanada, Osten (Öffentlich/Kanada)
- Indien, Mitte (Öffentlich/Indien)
- USA, Mitte (Öffentlich/USA)
- USA, Mitte EUAP (Öffentlich/Canary (US))
- Asien, Osten (Öffentlich/Asien-Pazifik)
- USA, Osten (Öffentlich/USA)
- USA, Osten 2 (Öffentlich/USA)
- Frankreich, Mitte (Öffentlich/Frankreich)
- Frankreich, Süden (Öffentlich/Frankreich)
- Deutschland, Westen-Mitte (Öffentlich/Deutschland)
- Japan, Osten (Öffentlich/Japan)
- Japan, Westen (Öffentlich/Japan)
- Zentralkorea (Öffentlich/Korea)
- Südkorea, Süden (Öffentlich/Korea)
- USA, Norden-Mitte (Öffentlich/USA)
- Europa, Norden (Öffentlich/Europa)
- Norwegen, Osten (Öffentlich/Norwegen)
- Südafrika, Norden (Öffentlich/Südafrika)
- USA, Süden-Mitte (Öffentlich/USA)
- Indien, Süden (Öffentlich/Indien)
- Asien, Südosten (Öffentlich/Asien-Pazifik)
- Schweiz, Norden (Öffentlich/Schweiz)
- VAE, Mitte (Öffentlich/VAE)
- VAE, Norden (Öffentlich/VAE)
- Vereinigtes Königreich, Süden (Öffentlich/Vereinigtes Königreich)
- Vereinigtes Königreich, Westen (Öffentlich/Vereinigtes Königreich)
- USA, Westen-Mitte (Öffentlich/USA)
- Europa, Westen (Öffentlich/Europa)
- Indien, Westen (Öffentlich/Indien)
- USA, Westen (Öffentlich/USA)
- USA, Westen 2 (Öffentlich/USA)
- USA, Westen 3 (Öffentlich/USA)


## <a name="known-issues"></a>Bekannte Probleme

Für Azure Firewall Premium sind die folgenden Probleme bekannt:


|Problem  |BESCHREIBUNG  |Minderung  |
|---------|---------|---------|
|ESNI-Unterstützung für FQDN-Auflösung in HTTPS|Verschlüsselte SNI wird im HTTPS-Handshake nicht unterstützt|Derzeit unterstützt nur Firefox ESNI über die benutzerdefinierte Konfiguration. Zur Problemumgehung wird empfohlen, dieses Feature zu deaktivieren.|
|Clientzertifikate (TLS)|Clientzertifikate werden verwendet, um ein gegenseitiges Vertrauen hinsichtlich der Identität zwischen dem Client und dem Server aufzubauen. Clientzertifikate werden während einer TLS-Aushandlung verwendet. Die Azure-Firewall handelt eine Verbindung mit dem Server erneut aus und hat keinen Zugriff auf den privaten Schlüssel der Clientzertifikate.|Keine|
|QUIC/HTTP3|QUIC ist die neue Hauptversion von HTTP. Dabei handelt es sich um ein UDP-basiertes Protokoll über 80 (PLAN) und 443 (SSL). Die FQDN/URL/TLS-Inspektion wird nicht unterstützt.|Konfigurieren Sie die Übergabe von UDP 80/443 als Netzwerkregeln.|
Nicht vertrauenswürdige, vom Kunden signierte Zertifikate|Vom Kunden signierte Zertifikate werden von der Firewall nicht als vertrauenswürdig eingestuft, sobald sie von einem intranetbasierten Webserver empfangen werden.|Es wird bereits nach einer Lösung gesucht.
|Falsche Quell-IP-Adresse in Warnungen mit IDPS für HTTP (ohne TLS-Inspektion).|Wenn HTTP-Nur-Text-Datenverkehr verwendet wird und IDPS eine neue Warnung ausgibt und das Ziel eine öffentliche IP-Adresse ist, ist die angezeigte Quell-IP-Adresse falsch (die interne IP-Adresse wird anstelle der ursprünglichen IP-Adresse angezeigt).|Es wird bereits nach einer Lösung gesucht.|
|Zertifikatverteilung|Nachdem ein Zertifizierungsstellenzertifikat auf die Firewall angewendet wurde, kann es zwischen 5-10 Minuten dauern, bis das Zertifikat wirksam wird.|Es wird bereits nach einer Lösung gesucht.|
|Unterstützung für TLS 1.3|TLS 1.3 wird teilweise unterstützt. Der TLS-Tunnel vom Client zur Firewall basiert auf TLS 1.2 und der von der Firewall zum externen Webserver auf TLS 1.3.|Updates werden derzeit untersucht.|
|Privater KeyVault-Endpunkt|Key Vault unterstützt den Zugriff auf private Endpunkte, um die Netzwerk-Präsenz einzuschränken. Vertrauenswürdige Azure-Dienste können diese Einschränkung umgehen, wenn eine Ausnahme konfiguriert ist, wie in der [KeyvVault-Dokumentation](../key-vault/general/overview-vnet-service-endpoints.md#trusted-services)beschrieben. Die Azure-Firewall ist derzeit nicht als vertrauenswürdiger Dienst aufgeführt und kann nicht auf die Key Vault zugreifen.|Es wird bereits nach einer Lösung gesucht.|

## <a name="next-steps"></a>Nächste Schritte

- [Erfahren Sie mehr über Azure Firewall Premium-Zertifikate](premium-certificates.md)
- [Bereitstellen und Konfigurieren von Azure Firewall Premium](premium-deploy.md)
- [Migrieren zu Azure Firewall Premium](premium-migrate.md)
