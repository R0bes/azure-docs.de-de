---
title: Erstellen eines benutzerdefinierten Azure DevTest Labs-Images aus einer VHD-Datei | Microsoft Docs
description: Erfahren Sie, wie Sie mit dem Azure-Portal aus einer VHD-Datei ein benutzerdefiniertes Image in Azure DevTest Labs erstellen.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 019399d9f6d9dfb8f9c21c52a631ff86fc2d6c34
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122355529"
---
# <a name="create-a-custom-image-from-a-vhd-file"></a>Erstellen eines benutzerdefinierten Images aus einer VHD-Datei

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

## <a name="step-by-step-instructions"></a>Schrittweise Anweisungen

Die folgenden Schritte führen Sie durch die Erstellung eines benutzerdefinierten Images aus einer VHD-Datei mithilfe des Azure-Portals:

1. Melden Sie sich beim [Azure-Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) an.

1. Wählen Sie **Alle Dienste** und dann in der Liste die Option **DevTest Labs**.

1. Wählen Sie in der Liste der Labs das gewünschte Lab aus.  

1. Wählen Sie im Hauptbereich des Labs die Option **Konfiguration und Richtlinien** aus. 

1. Wählen Sie im Bereich **Konfiguration und Richtlinien** die Option **Benutzerdefinierte Images**.

1. Wählen Sie im Bereich **Benutzerdefinierte Images** die Option **+ Hinzufügen**.

    ![Hinzufügen des benutzerdefinierten Images](./media/devtest-lab-create-template/add-custom-image.png)

1. Geben Sie den Namen des benutzerdefinierten Images ein. Dieser Name wird in der Liste der Basisimages angezeigt, wenn Sie einen virtuellen Computer erstellen.

1. Geben Sie die Beschreibung des benutzerdefinierten Images ein. Diese Beschreibung wird in der Liste der Basisimages angezeigt, wenn Sie einen virtuellen Computer erstellen.

1. Wählen Sie als **Betriebssystemtyp** entweder **Windows** oder **Linux**.

    - Wenn Sie **Windows** auswählen, können Sie über das Kontrollkästchen angeben, ob *sysprep* auf dem Computer ausgeführt wurde. 
    - Wenn Sie **Linux** auswählen, können Sie über das Kontrollkästchen angeben, ob *deprovision* auf dem Computer ausgeführt wurde. 

1. Wählen Sie im Dropdownmenü die Option **VHD**. Dies ist die virtuelle Festplatte (VHD), die zum Erstellen des neuen benutzerdefinierten Images verwendet werden soll. Aktivieren Sie bei Bedarf das Kontrollkästchen **VHD mithilfe von PowerShell hochladen**.

1. Sie können auch einen Plannamen, ein Planangebot und einen Planherausgeber eingeben, wenn das Image, das zum Erstellen des benutzerdefinierten Images verwendet wird, kein lizenziertes Image ist (von Microsoft veröffentlicht).

   - **Planname:** Geben Sie den Namen des Marketplace-Images (SKU) ein, aus dem dieses benutzerdefinierte Image erstellt wird. 
   - **Planangebot:** Geben Sie das Produkt (Angebot) des Marketplace-Images ein, aus dem dieses benutzerdefinierte Image erstellt wird. 
   - **Planherausgeber:** Geben Sie den Herausgeber des Marketplace-Images ein, aus dem dieses benutzerdefinierte Image erstellt wird.

   > [!NOTE]
   > Wenn das Image, das Sie zum Erstellen eines benutzerdefinierten Images verwenden, **kein** lizenziertes Image ist, sind diese Felder leer und können bei Bedarf mit Daten gefüllt werden. Wenn es sich um ein **lizenziertes Image** handelt, werden die Planinformationen automatisch in die Felder eingefügt. Falls Sie versuchen, diese Daten zu ändern, wird eine Warnmeldung angezeigt.
   >
   >

1. Wählen Sie **OK** , um das benutzerdefinierte Image zu erstellen.

Nach wenigen Minuten wird das benutzerdefinierte Image erstellt und im Speicherkonto des Labs gespeichert. Das Image ist in der Liste mit den Basisimages verfügbar, falls ein Lab-Benutzer einen neuen virtuellen Computer erstellen möchte.

![Verfügbares benutzerdefiniertes Image in der Liste mit den Basisimages](./media/devtest-lab-create-template/custom-image-available-as-base.png)


[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Verwandte Blogbeiträge

- [Custom images or formulas? (Benutzerdefinierte Images oder Formeln?)](/azure/devtest-labs/devtest-lab-faq#blog-post)
- [Copying Custom Images between Azure DevTest Labs (Kopieren benutzerdefinierter Images zwischen Azure DevTest Labs)](https://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

## <a name="next-steps"></a>Nächste Schritte

- [Hinzufügen eines virtuellen Computers zum Lab](./devtest-lab-add-vm.md)