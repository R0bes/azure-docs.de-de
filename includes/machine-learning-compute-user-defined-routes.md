---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 08/12/2021
ms.author: larryfr
ms.openlocfilehash: c2ad7408f00d8abf4cb5afdbdba44af5e0779380
ms.sourcegitcommit: 0ede6bcb140fe805daa75d4b5bdd2c0ee040ef4d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2021
ms.locfileid: "122603967"
---
Wenn Sie eine Azure Machine Learning-__Compute-Instanz__ oder einen -__Computecluster__ verwenden, gestatten Sie den eingehenden Datenverkehr von Azure Batch Management- und Azure Machine Learning-Diensten. Wenn Sie die benutzerdefinierten Routen für diesen Datenverkehr erstellen, können Sie entweder **IP-Adressen** oder **Diensttags** verwenden, um den Datenverkehr weiterzuleiten.

> [!IMPORTANT]
> Die Verwendung von Diensttags mit benutzerdefinierten Routen ist derzeit in der Vorschauversion und wird möglicherweise nicht vollständig unterstützt. Weitere Informationen finden Sie unter [Routing in virtuellen Netzwerken](/azure/virtual-network/virtual-networks-udr-overview#service-tags-for-user-defined-routes-preview).

# <a name="ip-address-routes"></a>[IP-Adressrouten](#tab/ipaddress)

Für den Azure Machine Learning Service müssen Sie die IP-Adresse sowohl den __primären__ als auch den __sekundären__ Regionen hinzufügen. Informationen zum Auffinden der sekundären Region finden Sie im Abschnitt [Sicherstellen von Geschäftskontinuität und Notfallwiederherstellung mit Azure-Regionspaaren](/azure/best-practices-availability-paired-regions#azure-regional-pairs). Wenn sich Ihr Azure Machine Learning Service z. B. in „USA, Osten 2“ befindet, ist die sekundäre Region „USA, Mitte“. 

Über die folgenden Methoden können Sie eine Liste der IP-Adressen des Batch-Diensts und von Azure Machine Learning Service abrufen:

* Laden Sie die [Azure-IP-Adressbereiche und Diensttags](https://www.microsoft.com/download/details.aspx?id=56519) herunter, und suchen Sie in der Datei nach `BatchNodeManagement.<region>` und `AzureMachineLearning.<region>`, wobei `<region>` Ihre Azure-Region ist.

* Laden Sie die Informationen mithilfe der [Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli) herunter. Im folgenden Beispiel werden die IP-Adressinformationen heruntergeladen und die Informationen für die Region „USA, Osten 2“ (primär) und „USA, Mitte“ (sekundär) herausgefiltert:

    ```azurecli-interactive
    az network list-service-tags -l "East US 2" --query "values[?starts_with(id, 'Batch')] | [?properties.region=='eastus2']"
    # Get primary region IPs
    az network list-service-tags -l "East US 2" --query "values[?starts_with(id, 'AzureMachineLearning')] | [?properties.region=='eastus2']"
    # Get secondary region IPs
    az network list-service-tags -l "Central US" --query "values[?starts_with(id, 'AzureMachineLearning')] | [?properties.region=='centralus']"
    ```

    > [!TIP]
    > Wenn Sie die Regionen „USA – Virginia“, „USA – Arizona“ oder „China, Osten 2“ verwenden, geben diese Befehle keine IP-Adressen zurück. Verwenden Sie stattdessen einen der folgenden Links zum Herunterladen einer Liste von IP-Adressen:
    >
    > * [Azure IP Ranges and Service Tags for Azure Government](https://www.microsoft.com/download/details.aspx?id=57063) (Azure-IP-Bereiche und -Diensttags für Azure Government)
    > * [Azure IP Ranges and Service Tags for Azure China](https://www.microsoft.com//download/details.aspx?id=57062) (Azure-IP-Bereiche und -Diensttags für Azure China)

> [!IMPORTANT]
> Die IP-Adressen können sich im Laufe der Zeit ändern.

Legen Sie beim Erstellen des UDR die Option __Typ des nächsten Hops__ auf __Internet__ fest. Die folgende Abbildung zeigt ein Beispiel für eine IP-Adress-basierte benutzerdefinierte Route (UDR) im Azure-Portal:

:::image type="content" source="./media/machine-learning-compute-user-defined-routes/user-defined-route.png" alt-text="Abbildung einer benutzerdefinierten Routenkonfiguration":::

# <a name="service-tag-routes"></a>[Diensttagrouten](#tab/servicetag)

Erstellen Sie benutzerdefinierte Routen für die folgenden Diensttags:

* `AzureMachineLearning`
* `BatchNodeManagement.<region>`, wobei `<region>` Ihre Azure-Region darstellt.

Die folgenden Befehle veranschaulichen das Hinzufügen von Routen für diese Diensttags:

```azurecli
az network route-table route create -g MyResourceGroup --route-table-name MyRouteTable -n AzureMLRoute --address-prefix AzureMachineLearning --next-hop-type Internet
az network route-table route create -g MyResourceGroup --route-table-name MyRouteTable -n BatchRoute --address-prefix BatchNodeManagement.westus2 --next-hop-type Internet
```

---

Informationen zum Konfigurieren von benutzerdefinierten Routen finden Sie unter [Weiterleiten von Netzwerkdatenverkehr mit einer Routingtabelle](/azure/virtual-network/tutorial-create-route-table-portal).