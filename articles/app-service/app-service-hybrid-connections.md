---
title: Hybridverbindungen
description: Erfahren Sie, wie Sie Hybridverbindungen in Azure App Service für den Zugriff auf Ressourcen in getrennten Netzwerken erstellen und verwenden.
author: ccompy
ms.assetid: 66774bde-13f5-45d0-9a70-4e9536a4f619
ms.topic: article
ms.date: 05/05/2021
ms.author: ccompy
ms.custom: seodec18, fasttrack-edit
ms.openlocfilehash: c8b0377207dc811358db14285a7e287cd7c72525
ms.sourcegitcommit: c385af80989f6555ef3dadc17117a78764f83963
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2021
ms.locfileid: "111412599"
---
# <a name="azure-app-service-hybrid-connections"></a>Azure App Service-Hybridverbindungen

Hybrid Connections ist sowohl ein Dienst in Azure als auch ein Feature in Azure App Service. Als Dienst gehen seine Einsatzmöglichkeiten und Funktionen über die von Azure App Service hinaus. Weitere Informationen zu Hybrid Connections und der Verwendung außerhalb von App Service finden Sie unter [Azure Relay Hybrid Connections][HCService].

In App Service kann mithilfe von Hybridverbindungen auf Anwendungsressourcen in einem beliebigen Netzwerk zugegriffen werden, das ausgehende Aufrufe an Azure über Port 443 ausführen kann. Hybridverbindungen ermöglichen den Zugriff von Ihrer App auf einen TCP-Endpunkt und ermöglichen keine neue Möglichkeit, um auf Ihre App zuzugreifen. Bei der Verwendung in App Service entspricht jede Hybridverbindung einer Kombination aus einem einzelnen TCP-Host und einem Port. Dadurch können Ihre Apps auf Ressourcen unter jedem beliebigen Betriebssystem zugreifen, sofern es sich um einen TCP-Endpunkt handelt. Das Feature Hybrid Connections besitzt keine Informationen über das Anwendungsprotokoll oder den abzurufenden Inhalt und benötigt diese Informationen auch nicht. Es ermöglicht lediglich den Netzwerkzugriff.  

## <a name="how-it-works"></a>Funktionsweise ##
Hybridverbindungen erfordern, dass ein Relay-Agent bereitgestellt wird, über den sie sowohl den gewünschten Endpunkt als auch Azure erreichen können. Der Relay-Agent, Hybridverbindungs-Manager (HCM), ruft ausgehend über Port 443 Azure Relay auf. Von der Web-App-Site aus stellt die App Service-Infrastruktur auch eine Verbindung mit Azure Relay im Auftrag Ihrer Anwendung her. Über die verbundenen Verbindungen kann Ihre App auf den gewünschten Endpunkt zugreifen. Die Verbindung verwendet zum Schutz TLS 1.2 und zur Authentifizierung/Autorisierung SAS-Schlüssel.

:::image type="content" source="media/app-service-hybrid-connections/hybridconn-connectiondiagram.png" alt-text="Allgemeines Flussdiagramm zu Hybridverbindungen":::

Wenn Ihre App eine DNS-Anforderung stellt, die mit einem konfigurierten Hybridverbindungsendpunkt übereinstimmt, wird der ausgehende TCP-Datenverkehr über die Hybridverbindung weitergeleitet.  

> [!NOTE]
> Sie sollten folglich versuchen, stets einen DNS-Namen für Ihre Hybridverbindung zu verwenden. Einige Clientsoftware führen keine DNS-Suche durch, wenn der Endpunkt stattdessen eine IP-Adresse verwendet. 
>

### <a name="app-service-hybrid-connection-benefits"></a>Vorteile der App Service-Hybridverbindungen ###

Die Funktion für Hybridverbindungen bietet eine Reihe von Vorteilen wie etwa Folgende:

- Sie ermöglicht Apps den sicheren Zugriff auf lokale Systeme und Dienste.
- Für das Feature ist kein Endpunkt erforderlich, der über das Internet zugänglich ist.
- Sie lässt sich schnell und einfach einrichten. Keine Gateways erforderlich.
- Jede Hybridverbindung entspricht einer einzelnen Host-Port-Kombination, die auch einen Sicherheitsvorteil darstellt.
- Sie erfordert normalerweise keine Firewalllücken. Die Verbindungen sind alle ausgehend über Standardwebports.
- Da die Funktion auf Netzwerkebene ausgeführt wird, ist sie nicht von der Sprache, die von Ihrer App verwendet wird, und von der Technologie, die vom Endpunkt verwendet wird, abhängig.
- Sie kann verwendet werden, um über eine einzige App Zugriff in mehreren Netzwerken bereitzustellen. 
- Sie wird in den allgemein verfügbaren Versionen von Windows- und Linux-Apps unterstützt. Sie wird für Windows-Container-Apps nicht unterstützt.

### <a name="things-you-cannot-do-with-hybrid-connections"></a>Einschränkungen bei Hybridverbindungen ###

Die folgenden Aktionen sind beispielsweise mit Hybridverbindungen nicht möglich:

- Bereitstellen eines Laufwerks.
- Verwenden von UDP.
- Zugreifen auf TCP-basierte Dienste, die dynamische Ports verwenden, wie der passive Modus für FTP oder der erweiterte passive Modus.
- Unterstützung von LDAP, weil UDP erforderlich sein kann.
- Unterstützung von Active Directory, da der Domänenbeitritt eines App Service-Workers nicht möglich ist. 

## <a name="add-and-create-hybrid-connections-in-your-app"></a>Hinzufügen und Erstellen von Hybridverbindungen in Ihrer App ##

Navigieren Sie zum Erstellen einer Hybridverbindung zum [Azure-Portal][portal], und wählen Sie Ihre App aus. Wählen Sie **Netzwerk** > **Endpunkte der Hybridverbindung konfigurieren** aus. Hier können Sie die Hybridverbindungen anzeigen, die für Ihre App konfiguriert sind.  

:::image type="content" source="media/app-service-hybrid-connections/hybridconn-portal.png" alt-text="Screenshot der Liste der Hybridverbindungen":::

Um eine neue Hybridverbindung hinzuzufügen, wählen Sie **[+] Hybridverbindung hinzufügen** aus.  Es wird eine Liste der Hybridverbindungen angezeigt, die Sie bereits erstellt haben. Um Ihrer App eine oder mehrere dieser Hybridverbindungen hinzuzufügen, markieren Sie diese und wählen anschließend **Ausgewählte Hybridverbindung hinzufügen** aus.

:::image type="content" source="media/app-service-hybrid-connections/hybridconn-addhc.png" alt-text="Screenshot des Portals für Hybridverbindungen":::

Wenn Sie eine neue Hybridverbindung erstellen möchten, wählen Sie **Neue Hybridverbindung erstellen** aus. Geben Sie Folgendes an: 

- Hybridverbindungsname
- Endpunkt-Hostname
- Endpunktport
- Zu verwendender Service Bus-Namespace

:::image type="content" source="media/app-service-hybrid-connections/hybridconn-createhc.png" alt-text="Screenshot des Dialogfelds „Neue Hybridverbindung erstellen“":::

Jede Hybridverbindung ist mit einem Service Bus-Namespace verbunden. Jeder Service Bus-Namespace befindet sich in einer Azure-Region. Es ist wichtig, einen Service Bus-Namespace in derselben Region wie Ihre App zu verwenden, um durch das Netzwerk verursachte Latenzen zu vermeiden.

Wenn Sie Ihre Hybridverbindung aus Ihrer App entfernen möchten, klicken Sie mit der rechten Maustaste darauf, und wählen Sie **Verbindung trennen** aus.  

Nachdem Ihrer App eine Hybridverbindung hinzugefügt wurde, können Sie die Details zu dieser anzeigen, indem Sie sie einfach auswählen. 

:::image type="content" source="media/app-service-hybrid-connections/hybridconn-properties.png" alt-text="Screenshot der Details von Hybridverbindungen":::

### <a name="create-a-hybrid-connection-in-the-azure-relay-portal"></a>Erstellen einer Hybridverbindung im Azure Relay-Portal ###

Sie können Hybridverbindungen nicht nur über das Portal Ihrer App erstellen, sondern auch über das Azure Relay-Portal. Damit eine Hybridverbindung von App Service verwendet werden kann, muss sie:

* eine Clientautorisierung erfordern
* ein Metadatenelement namens „endpoint“ aufweisen, das eine Host-Port-Kombination als Wert enthält

## <a name="hybrid-connections-and-app-service-plans"></a>Hybrid Connections und App Service-Pläne ##

App Service-Hybridverbindungen sind nur in den Preis-SKUs Basic, Standard, Premium und Isolated verfügbar. Es gelten Beschränkungen, die am Tarif geknüpft sind.  

| Tarif | Anzahl der im Plan verwendbaren Hybridverbindungen |
|----|----|
| Basic | 5 pro Plan |
| Standard | 25 pro Plan |
| Premium (v1-v3) | 220 pro App |
| Isoliert (v1-v2) | 220 pro App |

Die Benutzeroberfläche des App Service-Plans zeigt auch, wie viele Hybridverbindungen verwendet werden und von welchen Apps.

:::image type="content" source="media/app-service-hybrid-connections/hybridconn-aspproperties.png" alt-text="Screenshot der Eigenschaften eines App Service-Plans":::

Wählen Sie die Hybridverbindung aus, um Details anzuzeigen. Sie können alle Informationen sehen, die auch in der App-Ansicht angezeigt wurden. Außerdem können Sie sehen, wie viele andere Apps im gleichen Plan diese Hybridverbindung verwenden.

Es gilt eine Einschränkung für die Anzahl der Hybridverbindungs-Endpunkte, die in einem App Service-Plan verwendet werden können. Jede verwendete Hybridverbindung kann jedoch von einer beliebigen Anzahl von Apps in diesem Plan verwendet werden. Eine einzelne Hybridverbindung, die in fünf separaten Apps in einem App Service-Plan verwendet wird, zählt z.B. als eine Hybridverbindung.

### <a name="pricing"></a>Preise ###

Zusätzlich zur SKU-Anforderung eines App-Service-Plans fallen zusätzliche Kosten für die Nutzung von Hybridverbindungen an. Für jeden Listener, der von einer Hybridverbindung verwendet wird, fällt eine Gebühr an. Der Listener ist der Hybridverbindungs-Manager. Wenn Sie fünf Hybridverbindungen verwenden, die von zwei Hybridverbindungs-Managern unterstützt werden, würde dies 10 Listener bedeuten. Weitere Informationen finden Sie unter [Service Bus – Preise][sbpricing].

## <a name="hybrid-connection-manager"></a>Hybrid Connection Manager ##

Damit das Feature Hybrid Connections funktioniert, ist ein Relay-Agent in dem Netzwerk erforderlich, das Ihren Hybridverbindungs-Endpunkt hostet. Dieser Relay-Agent wird als Hybrid Connection Manager (HCM) bezeichnet. Zum Herunterladen von HCM wählen Sie in Ihrer App im [Azure-Portal][portal] die Option **Netzwerk** > **Endpunkte der Hybridverbindung konfigurieren** aus.  

Dieses Tool wird unter Windows Server 2012 und höher ausgeführt. HCM wird als Dienst ausgeführt und verbindet ausgehenden Datenverkehr mit Azure Relay über Port 443.  

Sie können nach der Installation von HCM die Datei „HybridConnectionManagerUi.exe“ ausführen, um die Benutzeroberfläche für das Tool zu verwenden. Diese Datei befindet sich im Installationsverzeichnis des Hybridverbindungs-Managers. Unter Windows 10 können Sie auch einfach im Suchfeld nach *Hybridverbindungs-Manager-Benutzeroberfläche* suchen.

:::image type="content" source="media/app-service-hybrid-connections/hybridconn-hcm.png" alt-text="Screenshot von Hybridverbindungs-Manager":::

Wenn Sie die Benutzeroberfläche des HCM starten, wird als Erstes eine Tabelle angezeigt, in der alle Hybridverbindungen aufgeführt werden, die mit dieser Instanz des HCM konfiguriert sind. Wenn Sie Änderungen vornehmen möchten, müssen Sie sich zunächst bei Azure authentifizieren. 

So fügen Sie Ihrem HCM eine oder mehrere Hybridverbindungen hinzu:

1. Starten Sie die Benutzeroberfläche des HCM.
2. Wählen Sie **Neue Hybridverbindung hinzufügen** aus.
:::image type="content" source="media/app-service-hybrid-connections/hybridconn-hcmadd.png" alt-text="Screenshot der Konfiguration neuer Hybridverbindungen":::

1. Melden Sie sich mit Ihrem Azure-Konto an, um die für Ihre Abonnements verfügbaren Hybrid Connections abzurufen. Der HCM verwendet Ihr Azure-Konto darüber hinaus nicht. 
1. Wählen Sie ein Abonnement aus.
1. Wählen Sie die Hybridverbindungen aus, die der HCM weiterleiten soll.
:::image type="content" source="media/app-service-hybrid-connections/hybridconn-hcmadded.png" alt-text="Screenshot von Hybridverbindungen":::

1. Wählen Sie **Speichern** aus.

Es werden nun die Hybridverbindungen angezeigt, die Sie hinzugefügt haben. Sie können die konfigurierte Hybridverbindung auch auswählen, um Details anzuzeigen.

:::image type="content" source="media/app-service-hybrid-connections/hybridconn-hcmdetails.png" alt-text="Screenshot von Details einer Hybridverbindung":::

Damit der HCM die bei ihm konfigurierten Hybridverbindungen unterstützen kann, ist Folgendes erforderlich:

- TCP-Zugriff auf Azure über Port 443.
- TCP-Zugriff auf den Hybridverbindungs-Endpunkt
- Möglichkeit für DNS-Suchvorgänge auf dem Endpunkthost und im Service Bus-Namespace

> [!NOTE]
> Azure-Relay nutzt Websockets für die Konnektivität. Diese Funktion ist nur für Windows Server 2012 oder höher verfügbar. Aus diesem Grund wird der HCM von Versionen vor Windows Server 2012 nicht unterstützt.
>

### <a name="redundancy"></a>Redundanz ###

Jeder HCM kann mehrere Hybridverbindungen unterstützen. Darüber hinaus kann eine bestimmte Hybridverbindung durch mehrere HCMs unterstützt werden. Das Standardverhalten ist ein Weiterleiten von Datenverkehr über die konfigurierten HCMs für einen bestimmten Endpunkt. Wenn Sie Hochverfügbarkeit für Hybridverbindungen über Ihr Netzwerk wünschen, führen Sie mehrere HCMs auf unterschiedlichen Computern aus. Der Lastverteilungsalgorithmus, der vom Relaydienst verwendet wird, um Datenverkehr an die HCMs zu verteilen, weist nach dem Zufallsprinzip zu. 

### <a name="manually-add-a-hybrid-connection"></a>Manuelles Hinzufügen einer Hybridverbindung ###

Damit ein Benutzer außerhalb Ihres Abonnements eine HCM-Instanz für eine bestimmte Hybridverbindung hosten kann, geben Sie für diesen die Gateway-Verbindungszeichenfolge für die Hybridverbindung frei. Sie können die Gatewayverbindungszeichenfolge in den Eigenschaften einer Hybridverbindung im [Azure-Portal][portal] anzeigen. Um diese Zeichenfolge zu verwenden, wählen Sie im HCM **Manuell eingeben** aus, und fügen Sie die Gateway-Verbindungszeichenfolge ein.

:::image type="content" source="media/app-service-hybrid-connections/hybridconn-manual.png" alt-text="Manuelles Hinzufügen einer Hybridverbindung":::

### <a name="upgrade"></a>Aktualisieren ###

Es sind regelmäßige Updates für den Hybridverbindungs-Manager zum Beheben von Problemen oder Bereitstellen von Verbesserungen verfügbar. Wenn Upgrades veröffentlicht werden, wird ein Popupfenster in der HCM-Benutzeroberfläche angezeigt. Durch Ausführen des Upgrades werden die Änderungen angewendet, und der HCM wird neu gestartet. 

## <a name="adding-a-hybrid-connection-to-your-app-programmatically"></a>Programmgesteuertes Hinzufügen einer Hybridverbindung zu Ihrer App ##

Es gibt Azure CLI-Unterstützung für Hybridverbindungen. Die bereitgestellten Befehle funktionieren sowohl auf der App- als auch auf der App Service-Planebene.  Die Befehle auf App-Ebene lauten:

```azurecli
az webapp hybrid-connection

Group
    az webapp hybrid-connection : Methods that list, add and remove hybrid-connections from webapps.
        This command group is in preview. It may be changed/removed in a future release.
Commands:
    add    : Add a hybrid-connection to a webapp.
    list   : List the hybrid-connections on a webapp.
    remove : Remove a hybrid-connection from a webapp.
```

Mit den App Service Plan-Befehlen können Sie festlegen, welcher Schlüssel von einer bestimmten Hybridverbindung verwendet werden soll. Für jede Hybridverbindung sind zwei Schlüssel festgelegt, ein primärer und ein sekundärer. Sie können die Verwendung des primären oder sekundären Schlüssels mit den folgenden Befehlen auswählen. Dies ermöglicht es Ihnen, die Schlüssel zu wechseln, wenn Sie Ihre Schlüssel in regelmäßigen Abständen erneut generieren möchten. 

```azurecli
az appservice hybrid-connection --help

Group
    az appservice hybrid-connection : A method that sets the key a hybrid-connection uses.
        This command group is in preview. It may be changed/removed in a future release.
Commands:
    set-key : Set the key that all apps in an appservice plan use to connect to the hybrid-
                connections in that appservice plan.
```

## <a name="secure-your-hybrid-connections"></a>Sichern der Hybridverbindungen ##

Eine vorhandene Hybridverbindung kann von jedem Benutzer mit ausreichenden Berechtigungen für das zugrunde liegende Azure Service Bus Relay anderen App Service-Web-Apps hinzugefügt werden. Daher müssen Sie den Zugriff auf das Azure Service Bus Relay blockieren, wenn Sie verhindern möchten, dass dieselbe Hybridverbindung von anderen wiederverwendet wird (wenn z. B. die Zielressource ein Dienst ist, für den keine weiteren Sicherheitsmaßnahmen vorhanden sind, um nicht autorisierten Zugriff zu verhindern).

Jeder Benutzer mit `Reader`-Zugriff auf das Relay kann die Hybridverbindung _anzeigen_, wenn er versucht, diese seiner Web-App im Azure-Portal hinzuzufügen. Er kann sie jedoch nicht _hinzufügen_, da er nicht über die Berechtigungen zum Abrufen der Verbindungszeichenfolge verfügt, die zum Herstellen der Relayverbindung verwendet wird. Damit die Hybridverbindung erfolgreich hinzugefügt werden kann, müssen Sie über die `listKeys`-Berechtigung (`Microsoft.Relay/namespaces/hybridConnections/authorizationRules/listKeys/action`) verfügen. Mit der Rolle `Contributor` oder einer anderen Rolle, die diese Berechtigung für das Relay umfasst, können Benutzer die Hybridverbindung verwenden und sie den eigenen Web-Apps hinzufügen.

## <a name="troubleshooting"></a>Problembehandlung ##

Der Status „Verbunden“ bedeutet, dass mindestens ein HCM mit dieser Hybridverbindung konfiguriert ist und Azure erreichen kann. Wenn der Status für die Hybridverbindung nicht **Verbunden** ist, wurde die Hybridverbindung nicht auf einem HCM konfiguriert, der auf Azure zugreifen kann. Wenn für Ihren HCM der Status **Nicht verbunden** angezeigt wird, müssen Sie einige Punkte überprüfen:

* Verfügt Ihr Host an Port 443 über ausgehenden Zugriff auf Azure? Sie können dies über Ihren HCM-Host mithilfe des PowerShell-Befehls *Test-NetConnection Destination -P Port* testen. 
* Ist Ihr HCM möglicherweise in einem fehlerhaften Zustand? Versuchen Sie den lokalen Dienst „Azure Hybrid Connection Manager Service“ neu zu starten.

* Haben Sie in Konflikt stehende Software installiert? Hybridverbindungs-Manager kann nicht mit Biztalk-Hybridverbindungs-Manager oder Service Bus für Windows Server gleichzeitig vorhanden sein. Daher sollten bei der Installation von HCM alle Versionen dieser Pakete zuerst entfernt werden.

Gehen Sie wie folgt vor, wenn der Status **Verbunden** lautet, die App den Endpunkt aber nicht erreichen kann:

* Stellen Sie sicher, dass Sie einen DNS-Namen in Ihrer Hybridverbindung verwenden. Wenn Sie eine IP-Adresse verwenden, wird möglicherweise das erforderliche Client-DNS-Lookup nicht durchgeführt. Wenn der Client, der in Ihrer Web-App ausgeführt wird, kein DNS-Lookup durchführt, funktioniert die Hybridverbindung nicht.
* Prüfen Sie, ob der DNS-Name, der in Ihrer Hybridverbindung verwendet wird, vom HCM-Host aufgelöst werden kann. Überprüfen Sie die Auflösung mithilfe von *nslookup EndpointDNSname*, wobei EndpointDNSname eine genaue Entsprechung für die Verwendung in der Definition Ihrer Hybridverbindung ist.
* Testen Sie den Zugriff von Ihrem HCM-Host auf den Endpunkt mithilfe des PowerShell-Befehls *Test-NetConnection EndpointDNSname -P Port*. Wenn Sie den Endpunkt von Ihrem HCM-Host aus nicht erreichen können, überprüfen Sie die Firewalls zwischen den beiden Hosts einschließlich aller hostbasierten Firewalls für den Zielhost.

In App Service kann das Befehlszeilentool **tcpping** über die Konsole „Erweiterte Tools (Kudu)“ aufgerufen werden. Mit diesem Tool können Sie feststellen, ob Sie Zugriff auf einen TCP-Endpunkt haben, jedoch nicht, ob Sie Zugriff auf einen Hybridverbindungsendpunkt haben. Wenn Sie das Tool an der Konsole für einen Hybridverbindungs-Endpunkt verwenden, bestätigen Sie nur, dass eine Host-Port-Kombination verwendet wird.  

Wenn Sie über einen Befehlszeilenclient für Ihren Endpunkt verfügen, können Sie die Konnektivität über die App-Konsole testen. Sie können beispielsweise den Zugriff auf Webserver-Endpunkte mithilfe von cURL testen.


<!--Links-->
[HCService]: /azure/service-bus-relay/relay-hybrid-connections-protocol/
[portal]: https://portal.azure.com/
[oldhc]: /azure/biztalk-services/integration-hybrid-connection-overview/
[sbpricing]: https://azure.microsoft.com/pricing/details/service-bus/
[armclient]: https://github.com/projectkudu/ARMClient/
