---
title: Aktivieren der reinen Azure Active Directory-Authentifizierung mit Azure SQL
description: Dieser Artikel beschreibt die Vorgehensweise zur Aktivierung der reinen Azure AD-Authentifizierung (Azure Active Directory) mit Azure SQL-Datenbank und Azure SQL Managed Instance.
ms.service: sql-db-mi
ms.subservice: security
ms.topic: tutorial
author: GithubMirek
ms.author: mireks
ms.reviewer: vanto
ms.date: 06/30/2021
ms.openlocfilehash: 6ea0da3b8a6ddcc2f41bd9e779742f3e2d9cbe4d
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2021
ms.locfileid: "114470761"
---
# <a name="tutorial-enable-azure-active-directory-only-authentication-with-azure-sql"></a>Tutorial: Aktivieren der reinen Azure Active Directory-Authentifizierung mit Azure SQL

[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

> [!NOTE]
> Die in diesem Artikel behandelte **reine Azure AD-Authentifizierung** befindet sich in der **Public Preview-Phase**. 

Dieser Artikel beschreibt die Vorgehensweise zur Aktivierung der [reinen Azure AD-Authentifizierung](authentication-azure-ad-only-authentication.md) mit Azure SQL-Datenbank und Azure SQL Managed Instance. Wenn Sie eine SQL-Datenbank oder eine verwaltete Instanz bereitstellen möchten, für die ausschließlich die Azure AD-Authentifizierung aktiviert ist, finden Sie weitere Informationen unter [Erstellen eines Servers mit aktivierter reiner Azure AD-Authentifizierung in Azure SQL](authentication-azure-ad-only-authentication-create-server.md).

In diesem Tutorial lernen Sie, wie die folgenden Aufgaben ausgeführt werden:

> [!div class="checklist"]
> - Zuweisen einer Rolle zum Aktivieren der reinen Azure AD-Authentifizierung
> - Aktivieren der reinen Azure AD-Authentifizierung mithilfe von Azure-Portal, Azure CLI oder PowerShell
> - Prüfen, ob die Option für die reine Azure AD-Authentifizierung aktiviert ist
> - Testen der Verbindung mit Azure SQL-DB
> - Deaktivieren der reinen Azure AD-Authentifizierung mithilfe von Azure-Portal, Azure CLI oder PowerShell


## <a name="prerequisites"></a>Voraussetzungen

- Eine Azure AD-Instanz. Weitere Informationen finden Sie unter [Konfigurieren und Verwalten der Azure Active Directory-Authentifizierung mit Azure SQL](authentication-aad-configure.md).
- Eine SQL-Datenbank oder SQL Managed Instance mit einer Datenbank sowie Anmeldungen oder Benutzern. Weitere Informationen finden Sie unter [Schnellstart: Erstellen einer Azure SQL-Einzeldatenbank](single-database-create-quickstart.md), wenn Sie noch keine Azure SQL-Datenbank erstellt haben, oder [Schnellstart: Erstellen einer verwalteten Azure SQL-Instanz](../managed-instance/instance-create-quickstart.md).

## <a name="assign-role-to-enable-azure-ad-only-authentication"></a>Zuweisen einer Rolle zum Aktivieren der reinen Azure AD-Authentifizierung

Um die reine Azure AD-Authentifizierung zu aktivieren oder zu deaktivieren, sind ausgewählte integrierte Rollen für die Azure AD-Benutzer erforderlich, die diese Vorgänge in diesem Tutorial ausführen. In diesem Tutorial weisen wir dem Benutzer die Rolle [SQL Security Manager](../../role-based-access-control/built-in-roles.md#sql-security-manager) zu.

Weitere Informationen über das Zuweisen von Rollen zu einem Azure AD-Konto finden Sie unter [Zuweisen von Administrator- und anderen Rollen zu Benutzern mithilfe von Azure Active Directory](../../active-directory/fundamentals/active-directory-users-assign-role-azure-portal.md).

Weitere Informationen über die erforderliche Berechtigung zum Aktivieren oder Deaktivieren der reinen Azure AD-Authentifizierung finden Sie im [Abschnitt Berechtigungen des Artikels zur reinen Azure AD-Authentifizierung](authentication-azure-ad-only-authentication.md#permissions).

1. In unserem Beispiel weisen wir dem Benutzer `UserSqlSecurityManager@contoso.onmicrosoft.com` die Rolle **SQL Security Manager** zu. Melden Sie sich mit dem privilegierten Benutzer, der Azure AD-Rollen zuweisen kann, beim [Azure-Portal](https://portal.azure.com/) an.
1. Wechseln Sie zu Ihrer SQL-Serverressource, und wählen Sie im Menü **Zugriffssteuerung (IAM)** aus. Klicken Sie auf die Schaltfläche **Hinzufügen** und dann im Dropdownmenü auf **Rollenzuweisung hinzufügen**.

   :::image type="content" source="media/authentication-azure-ad-only-authentication/azure-ad-only-authentication-access-control.png" alt-text="Bereich „Zugriffssteuerung“ im Azure-Portal":::

1. Wählen Sie im Bereich **Rollenzuweisung hinzufügen** die Rolle **SQL Security Manager** aus, und wählen Sie dann den Benutzer aus, der die Berechtigung zum Aktivieren und Deaktivieren der reinen Azure AD-Authentifizierung haben soll.

   :::image type="content" source="media/authentication-azure-ad-only-authentication/azure-ad-only-authentication-access-control-add-role.png" alt-text="Bereich „Rollenzuweisung hinzufügen“ im Azure-Portal":::

1. Klicken Sie auf **Speichern**.

## <a name="enable-azure-ad-only-authentication"></a>Aktivieren der reinen Azure AD-Authentifizierung

# <a name="portal"></a>[Portal](#tab/azure-portal)

## <a name="enable-in-sql-database-using-azure-portal"></a>Aktivieren in der SQL-Datenbank mithilfe von Azure-Portal

Führen Sie die folgenden Schritte aus, um die reine Azure AD-Authentifizierung im Azure-Portal zu aktivieren.

1. Melden Sie sich mit dem Benutzer mit der zugewiesenen Rolle [SQL Security Manager](../../role-based-access-control/built-in-roles.md#sql-security-manager) im [Azure-Portal](https://portal.azure.com/) an.
1. Wechseln Sie zu Ihrer SQL-Serverressource, und wählen Sie im Menü **Einstellungen** das **Azure Active Directory** aus.

   :::image type="content" source="media/authentication-azure-ad-only-authentication/azure-ad-only-authentication-portal.png" alt-text="Menü zum Aktivieren der reinen Azure AD-Authentifizierung":::

1. Bevor Sie die reine Azure AD-Authentifizierung aktivieren können, müssen Sie einen **Azure Active Directory-Administrator** hinzufügen, sofern Sie dies noch nicht getan haben.
1. Aktivieren Sie das Kontrollkästchen **Unterstützung der Authentifizierung nur bei Azure Active Directory-Authentifizierung für diesen Server**.
1. Das Popupfenster **Nur Azure AD-Authentifizierung aktivieren** wird angezeigt. Klicken Sie auf **Ja**, um die Funktion zu aktivieren, und **speichern** Sie die Einstellung.

## <a name="azure-sql-managed-instance"></a>Verwaltete Azure SQL-Instanz

Das Verwalten der reinen Azure AD-Authentifizierung für eine verwaltete SQL-Instanz im Portal wird derzeit nicht unterstützt.

# <a name="the-azure-cli"></a>[Die Azure-CLI](#tab/azure-cli)

## <a name="enable-in-sql-database-using-azure-cli"></a>Aktivieren in der SQL-Datenbank mithilfe von Azure CLI

Nutzen Sie die folgenden Befehle, um die reine Azure AD-Authentifizierung in der Azure SQL-Datenbank mithilfe von Azure CLI zu aktivieren. [Installieren Sie die neueste Version der Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli-windows). Sie benötigen mindestens die Azure CLI-Version **2.14.2**. Weitere Informationen über diese Befehle finden Sie unter [az sql server ad-only-auth](/cli/azure/sql/server/ad-only-auth).

Weitere Informationen über das Verwalten der reinen Azure AD-Authentifizierung mithilfe von APIs finden Sie unter [Verwalten der reinen Azure AD-Authentifizierung mithilfe von APIs](authentication-azure-ad-only-authentication.md#managing-azure-ad-only-authentication-using-apis).

> [!NOTE]
> Der Azure AD-Administrator muss vor dem Aktivieren der reinen Azure AD-Authentifizierung für den Server festgelegt werden. Andernfalls wird der Azure CLI-Befehl nicht erfolgreich ausgeführt.
>
> Informationen über Berechtigungen und Aktionen, die für das Ausführen der Befehle zur Aktivierung der reinen Azure AD-Authentifizierung durch den Benutzer erforderlich sind, finden Sie im Artikel [Reine Azure AD-Authentifizierung](authentication-azure-ad-only-authentication.md#permissions).

1. [Melden Sie sich bei Azure](/cli/azure/authenticate-azure-cli) mit dem Konto mit der Rolle [SQL Security Manager](../../role-based-access-control/built-in-roles.md#sql-security-manager) an.

   ```azurecli
   az login
   ```

1. Führen Sie den folgenden Befehl aus, und ersetzen Sie dabei `<myserver>` durch ihren SQL-Servernamen und `<myresource>` durch Ihre Azure-Ressource, die den SQL-Server enthält.

   ```azurecli
   az sql server ad-only-auth enable --resource-group <myresource> --name <myserver>
   ```

## <a name="enable-in-sql-managed-instance-using-azure-cli"></a>Aktivieren in SQL Managed Instance mithilfe von Azure CLI

Nutzen Sie die folgenden Befehle, um die reine Azure AD-Authentifizierung in Azure SQL Managed Instance mithilfe von Azure CLI zu aktivieren. [Installieren Sie die neueste Version der Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli-windows). 

1. [Melden Sie sich bei Azure](/cli/azure/authenticate-azure-cli) mit dem Konto mit der Rolle [SQL Security Manager](../../role-based-access-control/built-in-roles.md#sql-security-manager) an.

   ```azurecli
   az login
   ```

1. Führen Sie den folgenden Befehl aus, und ersetzen Sie dabei `<myserver>` durch ihren SQL-Servernamen und `<myresource>` durch Ihre Azure-Ressource, die den SQL-Server enthält.

   ```azurecli
   az sql mi ad-only-auth enable --resource-group <myresource> --name <myserver>
   ```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

## <a name="enable-in-sql-database-using-powershell"></a>Aktivieren in der SQL-Datenbank mithilfe von PowerShell

Nutzen Sie die folgenden Befehle, um die reine Azure AD-Authentifizierung in der Azure SQL-Datenbank mithilfe von PowerShell zu aktivieren. Zum Ausführen dieser Befehle ist das Modul [Az.Sql 2.10.0](https://www.powershellgallery.com/packages/Az.Sql/2.10.0) oder höher erforderlich. Weitere Informationen über diese Befehle finden Sie unter [Enable-AzSqlInstanceActiveDirectoryOnlyAuthentication](/powershell/module/az.sql/enable-azsqlinstanceactivedirectoryonlyauthentication).

Weitere Informationen über das Verwalten der reinen Azure AD-Authentifizierung mithilfe von APIs finden Sie unter [Verwalten der reinen Azure AD-Authentifizierung mithilfe von APIs](authentication-azure-ad-only-authentication.md#managing-azure-ad-only-authentication-using-apis).

> [!NOTE]
> Der Azure AD-Administrator muss vor dem Aktivieren der reinen Azure AD-Authentifizierung für den Server festgelegt werden. Andernfalls wird der PowerShell-Befehl nicht erfolgreich ausgeführt.
>
> Informationen über Berechtigungen und Aktionen, die für das Ausführen der Befehle zur Aktivierung der reinen Azure AD-Authentifizierung durch den Benutzer erforderlich sind, finden Sie im Artikel [Reine Azure AD-Authentifizierung](authentication-azure-ad-only-authentication.md#permissions). Wenn der Benutzer nicht über ausreichende Berechtigungen verfügt, erhalten Sie die folgende Fehlermeldung:
>
> ```output
> Enable-AzSqlServerActiveDirectoryOnlyAuthentication : The client
> 'UserSqlServerContributor@contoso.onmicrosoft.com' with object id
> '<guid>' does not have authorization to perform
> action 'Microsoft.Sql/servers/azureADOnlyAuthentications/write' over scope
> '/subscriptions/<guid>...'
> ```

1. [Melden Sie sich bei Azure](/powershell/azure/authenticate-azureps) mit dem Konto mit der Rolle [SQL Security Manager](../../role-based-access-control/built-in-roles.md#sql-security-manager) an.

   ```powershell
   Connect-AzAccount
   ```

1. Führen Sie den folgenden Befehl aus, und ersetzen Sie dabei `<myserver>` durch ihren SQL-Servernamen und `<myresource>` durch Ihre Azure-Ressource, die den SQL-Server enthält.

   ```powershell
   Enable-AzSqlServerActiveDirectoryOnlyAuthentication -ServerName <myserver>  -ResourceGroupName <myresource>
   ```

## <a name="enable-in-sql-managed-instance-using-powershell"></a>Aktivieren in SQL Managed Instance mithilfe von PowerShell

Nutzen Sie die folgenden Befehle, um die reine Azure AD-Authentifizierung in Azure SQL Managed Instance mithilfe von PowerShell zu aktivieren. Zum Ausführen dieser Befehle ist das Modul [Az.Sql 2.10.0](https://www.powershellgallery.com/packages/Az.Sql/2.10.0) oder höher erforderlich. 

Weitere Informationen über das Verwalten der reinen Azure AD-Authentifizierung mithilfe von APIs finden Sie unter [Verwalten der reinen Azure AD-Authentifizierung mithilfe von APIs](authentication-azure-ad-only-authentication.md#managing-azure-ad-only-authentication-using-apis).


1. [Melden Sie sich bei Azure](/powershell/azure/authenticate-azureps) mit dem Konto mit der Rolle [SQL Security Manager](../../role-based-access-control/built-in-roles.md#sql-security-manager) an.

   ```powershell
   Connect-AzAccount
   ```

1. Führen Sie den folgenden Befehl aus, und ersetzen Sie dabei `<myinstance>` durch den Namen Ihrer SQL Managed Instance und `<myresource>` durch Ihre Azure-Ressource, die die verwaltete SQL-Instanz enthält.

   ```powershell
   Enable-AzSqlInstanceActiveDirectoryOnlyAuthentication -InstanceName <myinstance> -ResourceGroupName <myresource>
   ```

---

## <a name="check-the-azure-ad-only-authentication-status"></a>Überprüfen des Status der reinen Azure AD-Authentifizierung

Überprüfen Sie, ob die reine Azure AD-Authentifizierung für Ihren Server oder Ihre Instanz aktiviert ist.

# <a name="portal"></a>[Portal](#tab/azure-portal)

Navigieren Sie im [Azure-Portal](https://portal.azure.com/) zu Ihrer **SQL Server**-Ressource. Wählen Sie im Menü **Einstellungen** das **Azure Active Directory** aus. Die Portalunterstützung für die reine Azure AD-Authentifizierung ist nur für die Azure SQL-Datenbank verfügbar.

# <a name="the-azure-cli"></a>[Die Azure-CLI](#tab/azure-cli)

Diese Befehle können verwendet werden, um zu überprüfen, ob die reine Azure AD-Authentifizierung für den logischen Server Ihrer SQL-Datenbank oder für Ihre verwaltete SQL-Instanz aktiviert ist. Mitglieder mit den Rollen [SQL Server-Mitwirkender](../../role-based-access-control/built-in-roles.md#sql-server-contributor) und [SQL Managed Instance-Mitwirkender](../../role-based-access-control/built-in-roles.md#sql-managed-instance-contributor) können diese Befehle verwenden, um den Status der reinen Azure AD-Authentifizierung zu überprüfen. Sie können die Funktion allerdings nicht aktivieren oder deaktivieren.

## <a name="check-status-in-sql-database"></a>Überprüfen des Status in SQL-Datenbank

1. [Melden Sie sich bei Azure](/cli/azure/authenticate-azure-cli) mit dem Konto mit der Rolle [SQL Security Manager](../../role-based-access-control/built-in-roles.md#sql-security-manager) an. Weitere Informationen über das Verwalten der reinen Azure AD-Authentifizierung mithilfe von APIs finden Sie unter [Verwalten der reinen Azure AD-Authentifizierung mithilfe von APIs](authentication-azure-ad-only-authentication.md#managing-azure-ad-only-authentication-using-apis).

   ```azurecli
   az login
   ```

1. Führen Sie den folgenden Befehl aus, und ersetzen Sie dabei `<myserver>` durch ihren SQL-Servernamen und `<myresource>` durch Ihre Azure-Ressource, die den SQL-Server enthält.

   ```azurecli
   az sql server ad-only-auth get --resource-group <myresource> --name <myserver>
   ```

1. Die folgende Ausgabe wird angezeigt.

   ```json
   {
    "azureAdOnlyAuthentication": true,
    "/subscriptions/<guid>/resourceGroups/mygroup/providers/Microsoft.Sql/servers/myserver/azureADOnlyAuthentications/Default",
    "name": "Default",
    "resourceGroup": "myresource",
    "type": "Microsoft.Sql/servers"
   }
   ```

## <a name="check-status-in-sql-managed-instance"></a>Überprüfen des Status in SQL Managed Instance

1. [Melden Sie sich bei Azure](/cli/azure/authenticate-azure-cli) mit dem Konto mit der Rolle [SQL Security Manager](../../role-based-access-control/built-in-roles.md#sql-security-manager) an.

   ```azurecli
   az login
   ```

1. Führen Sie den folgenden Befehl aus, und ersetzen Sie dabei `<myserver>` durch ihren SQL-Servernamen und `<myresource>` durch Ihre Azure-Ressource, die den SQL-Server enthält.

   ```azurecli
   az sql mi ad-only-auth get --resource-group <myresource> --name <myserver>
   ```

1. Die folgende Ausgabe wird angezeigt.

   ```json
   {
    "azureAdOnlyAuthentication": true,
    "id": "/subscriptions/<guid>/resourceGroups/myresource/providers/Microsoft.Sql/managedInstances/myinstance/azureADOnlyAuthentications/Default",
    "name": "Default",
    "resourceGroup": "myresource",
    "type": "Microsoft.Sql/managedInstances"
   }
   ```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Diese Befehle können verwendet werden, um zu überprüfen, ob die reine Azure AD-Authentifizierung für den logischen Server Ihrer SQL-Datenbank oder für Ihre verwaltete SQL-Instanz aktiviert ist. Mitglieder mit den Rollen [SQL Server-Mitwirkender](../../role-based-access-control/built-in-roles.md#sql-server-contributor) und [SQL Managed Instance-Mitwirkender](../../role-based-access-control/built-in-roles.md#sql-managed-instance-contributor) können diese Befehle verwenden, um den Status der reinen Azure AD-Authentifizierung zu überprüfen. Sie können die Funktion allerdings nicht aktivieren oder deaktivieren.

Der Status gibt **True** zurück, wenn die Funktion aktiviert ist, und **False**, wenn sie deaktiviert ist.

## <a name="check-status-in-sql-database"></a>Überprüfen des Status in SQL-Datenbank

1. [Melden Sie sich bei Azure](/powershell/azure/authenticate-azureps) mit dem Konto mit der Rolle [SQL Security Manager](../../role-based-access-control/built-in-roles.md#sql-security-manager) an. Weitere Informationen über das Verwalten der reinen Azure AD-Authentifizierung mithilfe von APIs finden Sie unter [Verwalten der reinen Azure AD-Authentifizierung mithilfe von APIs](authentication-azure-ad-only-authentication.md#managing-azure-ad-only-authentication-using-apis).

   ```powershell
   Connect-AzAccount
   ```

1. Führen Sie den folgenden Befehl aus, und ersetzen Sie dabei `<myserver>` durch ihren SQL-Servernamen und `<myresource>` durch Ihre Azure-Ressource, die den SQL-Server enthält.

   ```powershell
   Get-AzSqlServerActiveDirectoryOnlyAuthentication  -ServerName <myserver> -ResourceGroupName <myresource>
   ```

## <a name="check-status-in-sql-managed-instance"></a>Überprüfen des Status in SQL Managed Instance

1. [Melden Sie sich bei Azure](/powershell/azure/authenticate-azureps) mit dem Konto mit der Rolle [SQL Security Manager](../../role-based-access-control/built-in-roles.md#sql-security-manager) an.

   ```powershell
   Connect-AzAccount
   ```

1. Führen Sie den folgenden Befehl aus, und ersetzen Sie dabei `<myinstance>` durch den Namen Ihrer SQL Managed Instance und `<myresource>` durch Ihre Azure-Ressource, die die verwaltete SQL-Instanz enthält.

   ```powershell
   Get-AzSqlInstanceActiveDirectoryOnlyAuthentication -InstanceName <myinstance> -ResourceGroupName <myresource>
   ```

---

## <a name="test-sql-authentication-with-connection-failure"></a>Testen der SQL-Authentifizierung mit Verbindungsfehler

Nachdem Sie die reine Azure AD-Authentifizierung aktiviert haben, testen Sie mit [SQL Server Management Studio (SSMS)](/sql/ssms/download-sql-server-management-studio-ssms), ob Sie eine [Verbindung mit Ihrer SQL-Datenbank oder mit SQL Managed Instance herstellen können](connect-query-ssms.md). Verwenden Sie die SQL-Authentifizierung für die Verbindung.

Es wird in etwa folgende „Anmeldung fehlgeschlagen“-Meldung angezeigt:

```output
Cannot connect to <myserver>.database.windows.net.
Additional information:
  Login failed for user 'username'. Reason: Azure Active Directory only authentication is enabled.
  Please contact your system administrator. (Microsoft SQL Server, Error: 18456)
```

## <a name="disable-azure-ad-only-authentication"></a>Deaktivieren der reinen Azure AD-Authentifizierung

Wenn Sie die Funktion für die reine Azure AD-Authentifizierung deaktivieren, ermöglichen Sie sowohl die SQL-Authentifizierung als auch die Azure AD-Authentifizierung für Azure SQL.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. Melden Sie sich mit dem Benutzer mit der zugewiesenen Rolle [SQL Security Manager](../../role-based-access-control/built-in-roles.md#sql-security-manager) im [Azure-Portal](https://portal.azure.com/) an.
1. Wechseln Sie zu Ihrer SQL-Serverressource, und wählen Sie im Menü **Einstellungen** das **Azure Active Directory** aus.
1. Deaktivieren Sie das Kontrollkästchen **Unterstützung der Authentifizierung nur bei Azure Active Directory-Authentifizierung für diesen Server** und **speichern** Sie die Einstellung, um die reine Azure AD-Authentifizierung zu deaktivieren.

Das Verwalten der reinen Azure AD-Authentifizierung für eine verwaltete SQL-Instanz im Portal wird derzeit nicht unterstützt.

# <a name="the-azure-cli"></a>[Die Azure-CLI](#tab/azure-cli)

## <a name="disable-in-sql-database-using-azure-cli"></a>Deaktivieren in der SQL-Datenbank mithilfe von Azure CLI

Nutzen Sie die folgenden Befehle, um die reine Azure AD-Authentifizierung in der Azure SQL-Datenbank mithilfe von Azure CLI zu deaktivieren. 

1. [Melden Sie sich bei Azure](/cli/azure/authenticate-azure-cli) mit dem Konto mit der Rolle [SQL Security Manager](../../role-based-access-control/built-in-roles.md#sql-security-manager) an.

   ```azurecli
   az login
   ```

1. Führen Sie den folgenden Befehl aus, und ersetzen Sie dabei `<myserver>` durch ihren SQL-Servernamen und `<myresource>` durch Ihre Azure-Ressource, die den SQL-Server enthält.

   ```azurecli
   az sql server ad-only-auth disable --resource-group <myresource> --name <myserver>
   ```

1. Nachdem Sie die reine Azure AD-Authentifizierung deaktiviert haben, sollte beim Überprüfen des Status die folgende Ausgabe angezeigt werden:

   ```json
   {
    "azureAdOnlyAuthentication": false,
    "/subscriptions/<guid>/resourceGroups/mygroup/providers/Microsoft.Sql/servers/myserver/azureADOnlyAuthentications/Default",
    "name": "Default",
    "resourceGroup": "myresource",
    "type": "Microsoft.Sql/servers"
   }
   ```

## <a name="disable-in-sql-managed-instance-using-azure-cli"></a>Deaktivieren in SQL Managed Instance mithilfe von Azure CLI

Nutzen Sie die folgenden Befehle, um die reine Azure AD-Authentifizierung in Azure SQL Managed Instance mithilfe von Azure CLI zu deaktivieren. 

1. [Melden Sie sich bei Azure](/cli/azure/authenticate-azure-cli) mit dem Konto mit der Rolle [SQL Security Manager](../../role-based-access-control/built-in-roles.md#sql-security-manager) an.

   ```azurecli
   az login
   ```

1. Führen Sie den folgenden Befehl aus, und ersetzen Sie dabei `<myserver>` durch ihren SQL-Servernamen und `<myresource>` durch Ihre Azure-Ressource, die den SQL-Server enthält.

   ```azurecli
   az sql mi ad-only-auth disable --resource-group <myresource> --name <myserver>
   ```

1. Nachdem Sie die reine Azure AD-Authentifizierung deaktiviert haben, sollte beim Überprüfen des Status die folgende Ausgabe angezeigt werden:

   ```json
   {
    "azureAdOnlyAuthentication": false,
    "id": "/subscriptions/<guid>/resourceGroups/myresource/providers/Microsoft.Sql/managedInstances/myinstance/azureADOnlyAuthentications/Default",
    "name": "Default",
    "resourceGroup": "myresource",
    "type": "Microsoft.Sql/managedInstances"
   }
   ```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

## <a name="disable-in-sql-database-using-powershell"></a>Deaktivieren in der SQL-Datenbank mithilfe von PowerShell

Nutzen Sie die folgenden Befehle, um die reine Azure AD-Authentifizierung in der Azure SQL-Datenbank mithilfe von PowerShell zu deaktivieren.

1. [Melden Sie sich bei Azure](/powershell/azure/authenticate-azureps) mit dem Konto mit der Rolle [SQL Security Manager](../../role-based-access-control/built-in-roles.md#sql-security-manager) an.

   ```powershell
   Connect-AzAccount
   ```

1. Führen Sie den folgenden Befehl aus, und ersetzen Sie dabei `<myserver>` durch ihren SQL-Servernamen und `<myresource>` durch Ihre Azure-Ressource, die den SQL-Server enthält.

   ```powershell
   Disable-AzSqlServerActiveDirectoryOnlyAuthentication -ServerName <myserver>  -ResourceGroupName <myresource>
   ```

## <a name="disable-in-sql-managed-instance-using-powershell"></a>Deaktivieren in SQL Managed Instance mithilfe von PowerShell

Nutzen Sie die folgenden Befehle, um die reine Azure AD-Authentifizierung in Azure SQL Managed Instance mithilfe von PowerShell zu deaktivieren.

1. [Melden Sie sich bei Azure](/powershell/azure/authenticate-azureps) mit dem Konto mit der Rolle [SQL Security Manager](../../role-based-access-control/built-in-roles.md#sql-security-manager) an.

   ```powershell
   Connect-AzAccount
   ```

1. Führen Sie den folgenden Befehl aus, und ersetzen Sie dabei `<myinstance>` durch den Namen Ihrer SQL Managed Instance und `<myresource>` durch Ihre Azure-Ressource, die die verwaltete SQL-Instanz enthält.

   ```powershell
   Disable-AzSqlInstanceActiveDirectoryOnlyAuthentication -InstanceName <myinstance> -ResourceGroupName <myresource>
   ```

---

## <a name="test-connecting-to-azure-sql-again"></a>Wiederholtes Testen der Verbindung mit Azure SQL-DB

Nachdem Sie die reine Azure AD-Authentifizierung deaktiviert haben, testen Sie, ob Sie über die Anmeldedaten für die SQL-Authentifizierung eine Verbindung herstellen können. Sie sollten nun eine Verbindung mit Ihrem Server oder Ihrer Instanz herstellen können.

## <a name="next-steps"></a>Nächste Schritte

- [Reine Azure AD-Authentifizierung mit Azure SQL](authentication-azure-ad-only-authentication.md)
- [Erstellen eines Servers mit aktivierter reiner Azure AD-Authentifizierung in Azure SQL](authentication-azure-ad-only-authentication-create-server.md)
