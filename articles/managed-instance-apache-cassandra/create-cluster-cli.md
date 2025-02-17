---
title: 'Schnellstart: Erstellen eines Clusters vom Typ „Azure Managed Instance for Apache Cassandra“ mithilfe der CLI'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe der Azure CLI einen Cluster vom Typ „Azure Managed Instance for Apache Cassandra“ erstellen.
author: TheovanKraay
ms.author: thvankra
ms.service: managed-instance-apache-cassandra
ms.topic: quickstart
ms.date: 03/15/2021
ms.openlocfilehash: 6387b07aa29a836eae79b6090571f143091d2115
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "121749500"
---
# <a name="quickstart-create-an-azure-managed-instance-for-apache-cassandra-cluster-using-azure-cli-preview"></a>Schnellstart: Erstellen eines Clusters vom Typ „Azure Managed Instance for Apache Cassandra“ mithilfe der Azure CLI (Vorschau)

Azure Managed Instance for Apache Cassandra bietet automatisierte Bereitstellungs- und Skalierungsvorgänge für verwaltete Open-Source-basierte Apache Cassandra-Rechenzentren. Dieser Dienst unterstützt Sie dabei, Hybridszenarien zu beschleunigen und die laufende Wartung zu verringern.

> [!IMPORTANT]
> Azure Managed Instance for Apache Cassandra befindet sich derzeit in der Public Preview-Phase.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe der Azure CLI einen Cluster mit Azure Managed Instance for Apache Cassandra erstellen. Außerdem wird gezeigt, wie Sie ein Rechenzentrum erstellen und Knoten im Rechenzentrum hoch- oder herunterskalieren.

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment.md)]

* [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) mit einer Verbindung mit Ihrer selbstgehosteten bzw. lokalen Umgebung. Weitere Informationen zum Herstellen einer Verbindung für lokale Umgebungen mit Azure finden Sie im Artikel [Verbinden eines lokalen Netzwerks mit Azure](/azure/architecture/reference-architectures/hybrid-networking/).

* Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

> [!IMPORTANT]
> Für diesen Artikel ist die Azure CLI-Version 2.17.1 oder höher erforderlich. Wenn Sie Azure Cloud Shell verwenden, ist die neueste Version bereits installiert.

## <a name="create-a-managed-instance-cluster"></a><a id="create-cluster"></a>Erstellen eines Managed Instance-Clusters

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)

1. Legen Sie Ihre Abonnement-ID über die Azure CLI fest:

   ```azurecli-interactive
   az account set -s <Subscription_ID>
   ```

1. Erstellen Sie als Nächstes ein virtuelles Netzwerk mit einem dedizierten Subnetz in Ihrer Ressourcengruppe:

   ```azurecli-interactive
   az network vnet create -n <VNet_Name> -l eastus2 -g <Resource_Group_Name> --subnet-name <Subnet Name>
   ```
    > [!NOTE]
    > Für die Bereitstellung einer Instanz von Azure Managed Instance for Apache Cassandra ist Internetzugriff erforderlich. In Umgebungen, in denen der Internetzugriff eingeschränkt ist, tritt ein Fehler bei der Bereitstellung auf. Stellen Sie sicher, dass der Zugriff im VNet auf die folgenden wichtigen Azure-Dienste, die für die richtige Funktionsweise von Managed Cassandra erforderlich sind, nicht blockiert ist:
    > - Azure Storage
    > - Azure Key Vault
    > - Skalierungsgruppen für virtuelle Azure-Computer
    > - Azure-Überwachung
    > - Azure Active Directory
    > - Azure Security

1. Wenden Sie einige spezielle Berechtigungen auf das virtuelle Netzwerk an, die für die verwaltete Instanz erforderlich sind. Verwenden Sie den Befehl `az role assignment create`, und ersetzen Sie `<subscription ID>`, `<resource group name>` und `<VNet name>` durch die entsprechenden Werte:

   ```azurecli-interactive
   az role assignment create --assignee a232010e-820c-4083-83bb-3ace5fc29d0b --role 4d97b98b-1d4f-4787-a291-c67834d212e7 --scope /subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/<VNet name>
   ```

   > [!NOTE]
   > Die Werte `assignee` und `role` im vorherigen Befehl sind feste Werte. Geben Sie diese Werte daher genau so ein, wie im Befehl angegeben. Andernfalls treten beim Erstellen des Clusters Fehler auf. Treten beim Ausführen dieses Befehls Fehler auf, verfügen Sie möglicherweise nicht über die Berechtigungen zum Ausführen des Befehls. Wenden Sie sich an Ihren Administrator, um die entsprechenden Berechtigungen zu erhalten.

1. Erstellen Sie als Nächstes mithilfe des Befehls [az managed-cassandra cluster create](/cli/azure/managed-cassandra/cluster?view=azure-cli-latest&preserve-view=true#az_managed_cassandra_cluster_create) den Cluster in dem neu erstellten virtuellen Netzwerk. Führen Sie den folgenden Befehl mit dem Wert der Variablen `delegatedManagementSubnetId` aus:

   > [!NOTE]
   > Der Wert der von Ihnen unten angegebenen Variablen `delegatedManagementSubnetId` stimmt exakt mit dem Wert von `--scope` überein, den Sie im obigen Befehl angegeben haben:

   ```azurecli-interactive
   resourceGroupName='<Resource_Group_Name>'
   clusterName='<Cluster_Name>'
   location='eastus2'
   delegatedManagementSubnetId='/subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/<VNet name>/subnets/<subnet name>'
   initialCassandraAdminPassword='myPassword'
    
   az managed-cassandra cluster create \
      --cluster-name $clusterName \
      --resource-group $resourceGroupName \
      --location $location \
      --delegated-management-subnet-id $delegatedManagementSubnetId \
      --initial-cassandra-admin-password $initialCassandraAdminPassword \
      --debug
   ```

1. Erstellen Sie abschließend mithilfe des Befehls [az managed-cassandra datacenter create](/cli/azure/managed-cassandra/datacenter?view=azure-cli-latest&preserve-view=true#az_managed_cassandra_datacenter_create) ein Rechenzentrum für den Cluster mit drei Knoten.

   ```azurecli-interactive
   dataCenterName='dc1'
   dataCenterLocation='eastus2'
    
   az managed-cassandra datacenter create \
      --resource-group $resourceGroupName \
      --cluster-name $clusterName \
      --data-center-name $dataCenterName \
      --data-center-location $dataCenterLocation \
      --delegated-subnet-id $delegatedManagementSubnetId \
      --node-count 3 
   ```

1. Die Knoten des erstellten Rechenzentrums können bei Bedarf mithilfe des Befehls [az managed-cassandra datacenter update](/cli/azure/managed-cassandra/datacenter?view=azure-cli-latest&preserve-view=true#az_managed_cassandra_datacenter_update) hoch- oder herunterskaliert werden. Ändern Sie den Wert des Parameters `node-count` in den gewünschten Wert:

   ```azurecli-interactive
   resourceGroupName='<Resource_Group_Name>'
   clusterName='<Cluster Name>'
   dataCenterName='dc1'
   dataCenterLocation='eastus2'
    
   az managed-cassandra datacenter update \
      --resource-group $resourceGroupName \
      --cluster-name $clusterName \
      --data-center-name $dataCenterName \
      --node-count 9 
   ```

## <a name="connect-to-your-cluster"></a>Herstellen einer Clusterverbindung

Von Azure Managed Instance for Apache Cassandra werden keine Knoten mit öffentlicher IP-Adresse erstellt. Wenn Sie eine Verbindung mit dem neu erstellten Cassandra-Cluster herstellen möchten, müssen Sie eine weitere Ressource innerhalb des virtuellen Netzwerks erstellen. Bei dieser Ressource kann es sich um eine Anwendung handeln oder um einen virtuellen Computer, auf dem das Open-Source-Abfragetool [CQLSH](https://cassandra.apache.org/doc/latest/cassandra/tools/cqlsh.html) von Apache installiert ist. Sie können eine [Resource Manager-Vorlage](https://azure.microsoft.com/resources/templates/vm-simple-linux/) verwenden, um einen virtuellen Ubuntu-Computer bereitzustellen. Stellen Sie nach dessen Bereitstellung eine SSH-Verbindung mit dem Computer her, und installieren Sie CQLSH, wie in den folgenden Befehlen gezeigt:

```bash
# Install default-jre and default-jdk
sudo apt update
sudo apt install openjdk-8-jdk openjdk-8-jre

# Install the Cassandra libraries in order to get CQLSH:
echo "deb http://www.apache.org/dist/cassandra/debian 311x main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list
curl https://downloads.apache.org/cassandra/KEYS | sudo apt-key add -
sudo apt-get update
sudo apt-get install cassandra

# Export the SSL variables:
export SSL_VERSION=TLSv1_2
export SSL_VALIDATE=false

# Connect to CQLSH (replace <IP> with the private IP addresses of the nodes in your Datacenter):
host=("<IP>" "<IP>" "<IP>")
cqlsh $host 9042 -u cassandra -p cassandra --ssl
```

## <a name="troubleshooting"></a>Problembehandlung

Wenn beim Anwenden von Berechtigungen für Ihre Virtual Network-Instanz ein Fehler mit dem Hinweis auftritt, dass *der Benutzer oder Dienstprinzipal in der Graphdatenbank für „e5007d2c-4b13-4a74-9b6a-605d99f03501“ nicht gefunden werden kann*, können Sie die gleiche Berechtigung manuell über das Azure-Portal anwenden. Navigieren Sie zum Anwenden von Berechtigungen über das Portal zum Bereich **Zugriffssteuerung (IAM)** Ihres vorhandenen virtuellen Netzwerks, und fügen Sie der Rolle „Netzwerkadministrator“ eine Rollenzuweisung für „Azure Cosmos DB“ hinzu. Werden bei der Suche nach „Azure Cosmos DB“ zwei Einträge angezeigt, fügen Sie beide Einträge hinzu wie in der folgenden Abbildung gezeigt: 

   :::image type="content" source="./media/create-cluster-cli/apply-permissions.png" alt-text="Anwenden von Berechtigungen" lightbox="./media/create-cluster-cli/apply-permissions.png" border="true":::

> [!NOTE] 
> Die Azure Cosmos DB-Rollenzuweisung wird nur für Bereitstellungszwecke verwendet. Azure Managed Instance for Apache Cassandra verfügt über keine Back-End-Abhängigkeiten von Azure Cosmos DB.  

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Ressourcen nicht mehr benötigen, führen Sie den Befehl `az group delete` aus, um die Ressourcengruppe, die verwaltete Instanz und alle zugehörigen Ressourcen zu löschen:

```azurecli-interactive
az group delete --name <Resource_Group_Name>
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie gelernt, wie Sie mithilfe der Azure CLI einen Cluster vom Typ „Azure Managed Instance for Apache Cassandra“ erstellen. Nun können Sie mit der Nutzung des Clusters beginnen:

> [!div class="nextstepaction"]
> [Bereitstellen eines verwalteten Apache Spark-Clusters mit Azure Databricks](deploy-cluster-databricks.md)
