---
title: 'Schnellstart: Datenstreaming mit Azure Event Hubs mithilfe des Kafka-Protokolls'
description: 'Schnellstart: Dieser Artikel enthält Informationen zum Streamen in Azure Event Hubs mit dem Kafka-Protokoll und APIs.'
ms.topic: quickstart
ms.date: 05/10/2021
ms.openlocfilehash: b88515ee0d1bc46eb41585d05f61a2a246cfab44
ms.sourcegitcommit: d90cb315dd90af66a247ac91d982ec50dde1c45f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/04/2021
ms.locfileid: "113288390"
---
# <a name="quickstart-data-streaming-with-event-hubs-using-the-kafka-protocol"></a>Schnellstart: Datenstreaming mit Event Hubs mithilfe des Kafka-Protokolls
In dieser Schnellstartanleitung erfahren Sie, wie Daten an Event Hubs gestreamt werden, ohne dass Sie Protokollclients ändern oder eigene Cluster betreiben müssen. Sie lernen, wie Sie über Ihre Producer und Consumer mit Event Hubs kommunizieren – mit nur einer Konfigurationsänderung in Ihren Anwendungen. 

> [!NOTE]
> Dieses Beispiel ist auf [GitHub](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/quickstart/java) verfügbar.

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung benötigen Sie Folgendes:

* Lesen Sie den Artikel [Event Hubs für Apache Kafka](event-hubs-for-kafka-ecosystem-overview.md).
* Ein Azure-Abonnement. Wenn Sie keins besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) erstellen, bevor Sie beginnen.
* [Java Development Kit (JDK) 1.7 oder höher](/azure/developer/java/fundamentals/java-support-on-azure)
* Ein binäres Maven-Archiv ([Download](https://maven.apache.org/download.cgi)/[Installationsanleitung](https://maven.apache.org/install.html))
* [Git-Client](https://www.git-scm.com/)


## <a name="create-an-event-hubs-namespace"></a>Erstellen eines Event Hubs-Namespace
Wenn Sie einen Event Hubs-Namespace erstellen, wird der Kafka-Endpunkt für den Namespace automatisch aktiviert. Sie können Ereignisse von Ihren Anwendungen streamen, die das Kafka-Protokoll in Event Hubs verwenden. Befolgen Sie die Schritt-für-Schritt-Anleitung unter [Erstellen eines Event Hubs mithilfe des Azure-Portals](event-hubs-create.md), um einen Event Hubs-Namespace zu erstellen. Wenn Sie einen dedizierten Cluster verwenden, finden Sie weitere Informationen unter [Erstellen eines Namespace und eines Event Hubs in einem dedizierten Cluster](event-hubs-dedicated-cluster-create-portal.md#create-a-namespace-and-event-hub-within-a-cluster).

> [!NOTE]
> Event Hubs für Kafka wird im **Basic-Tarif** nicht unterstützt.

## <a name="send-and-receive-messages-with-kafka-in-event-hubs"></a>Senden und Empfangen von Nachrichten mit Kafka in Event Hubs

1. Klonen Sie das [Azure Event Hubs-Repository für Kafka](https://github.com/Azure/azure-event-hubs-for-kafka).

2. Navigieren Sie zu `azure-event-hubs-for-kafka/quickstart/java/producer`.

3. Aktualisieren Sie die Konfigurationsdetails für die Producer in `src/main/resources/producer.config` wie folgt:

    **TLS/SSL:**

    ```xml
    bootstrap.servers=NAMESPACENAME.servicebus.windows.net:9093
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
    ```
    
    > [!IMPORTANT]
    > Ersetzen Sie `{YOUR.EVENTHUBS.CONNECTION.STRING}` durch die Verbindungszeichenfolge für Ihren Event Hubs-Namespace. Anweisungen zum Abrufen der Verbindungszeichenfolge finden Sie unter [Abrufen einer Event Hubs-Verbindungszeichenfolge](event-hubs-get-connection-string.md). Hier sehen Sie eine Beispielkonfiguration: `sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="Endpoint=sb://mynamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=XXXXXXXXXXXXXXXX";`

    **OAuth:**

    ```xml
    bootstrap.servers=NAMESPACENAME.servicebus.windows.net:9093
    security.protocol=SASL_SSL
    sasl.mechanism=OAUTHBEARER
    sasl.jaas.config=org.apache.kafka.common.security.oauthbearer.OAuthBearerLoginModule required;
    sasl.login.callback.handler.class=CustomAuthenticateCallbackHandler;
    ```    

    Den Quellcode für die Beispielhandlerklasse „CustomAuthenticateCallbackHandler“ auf GitHub finden Sie [hier](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/oauth/java/appsecret/producer/src/main/java).
4. Führen Sie den Producer-Code aus, und streamen Sie Ereignisse an Event Hubs:
   
    ```shell
    mvn clean package
    mvn exec:java -Dexec.mainClass="TestProducer"                                    
    ```
    
5. Navigieren Sie zu `azure-event-hubs-for-kafka/quickstart/java/consumer`.

6. Aktualisieren Sie die Konfigurationsdetails für die Consumer in `src/main/resources/consumer.config` wie folgt:
   
    **TLS/SSL:**

    ```xml
    bootstrap.servers=NAMESPACENAME.servicebus.windows.net:9093
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
    ```

    > [!IMPORTANT]
    > Ersetzen Sie `{YOUR.EVENTHUBS.CONNECTION.STRING}` durch die Verbindungszeichenfolge für Ihren Event Hubs-Namespace. Anweisungen zum Abrufen der Verbindungszeichenfolge finden Sie unter [Abrufen einer Event Hubs-Verbindungszeichenfolge](event-hubs-get-connection-string.md). Hier sehen Sie eine Beispielkonfiguration: `sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="Endpoint=sb://mynamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=XXXXXXXXXXXXXXXX";`

    **OAuth:**

    ```xml
    bootstrap.servers=NAMESPACENAME.servicebus.windows.net:9093
    security.protocol=SASL_SSL
    sasl.mechanism=OAUTHBEARER
    sasl.jaas.config=org.apache.kafka.common.security.oauthbearer.OAuthBearerLoginModule required;
    sasl.login.callback.handler.class=CustomAuthenticateCallbackHandler;
    ``` 

    Den Quellcode für die Beispielhandlerklasse „CustomAuthenticateCallbackHandler“ auf GitHub finden Sie [hier](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/oauth/java/appsecret/consumer/src/main/java).

    Alle OAuth-Beispiele für Event Hubs für Kafka finden Sie [hier](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/oauth).
7. Führen Sie den Consumercode und die Prozessereignisse über Event Hubs unter Verwendung Ihrer Kafka-Clients aus:

    ```java
    mvn clean package
    mvn exec:java -Dexec.mainClass="TestConsumer"                                    
    ```

Wenn Ihr Kafka-Cluster in Event Hubs über Ereignisse verfügt, werden diese nun allmählich vom Consumer empfangen.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie erfahren, wie Daten an Event Hubs gestreamt werden, ohne dass Sie Protokollclients ändern oder eigene Cluster ausführen müssen. Weitere Informationen finden Sie unter [Apache Kafka-Entwicklerleitfaden für Azure Event Hubs](apache-kafka-developer-guide.md).