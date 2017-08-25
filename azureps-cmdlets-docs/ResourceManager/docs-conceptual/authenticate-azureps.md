---
title: "Přihlášení s využitím prostředí Azure PowerShell"
description: "Přihlášení s využitím prostředí Azure PowerShell"
services: azure
author: sdwheeler
ms.author: sewhee
manager: carmonm
ms.product: azure
ms.service: azure-powershell
ms.devlang: powershell
ms.topic: conceptual
ms.date: 05/15/2017
ms.openlocfilehash: f6d249ca5bb09c4fe8445ba5b339ffa6012815ed
ms.sourcegitcommit: 226527be7cb647acfe2ea9ab151185053ab3c6db
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/29/2017
---
# <a name="log-in-with-azure-powershell"></a>Přihlášení s využitím prostředí Azure PowerShell

Prostředí Azure PowerShell podporuje několik způsobů přihlášení. Nejjednodušším způsobem, jak začít, je přihlásit se interaktivně na příkazovém řádku.

## <a name="interactive-log-in"></a>Interaktivní přihlášení

1. Zadejte `Login-AzureRmAccount`. Zobrazí se dialogové okno s výzvou k zadání přihlašovacích údajů Azure.

2. Zadejte e-mailovou adresu a heslo, které jsou spojené s vaším účtem. Azure přihlašovací údaje ověří, uloží je a pak zavře okno.

## <a name="log-in-with-a-service-principal"></a>Přihlášení pomocí instančního objektu

Instanční objekty umožňují vytvořit neinteraktivní účty, pomocí kterých můžete manipulovat s prostředky. Instanční objekty se podobají uživatelským účtům, na které můžete pomocí Azure Active Directory aplikovat pravidla. Tím, že instančnímu objektu přidělíte minimální potřebná oprávnění, můžete zajistit ještě větší zabezpečení skriptů pro automatizaci.

1. Pokud ještě nemáte instanční objekt, [vytvořte si ho](create-azure-service-principal-azureps.md).

2. Přihlaste se pomocí instančního objektu.

    ```powershell
    Login-AzureRmAccount -ServicePrincipal -ApplicationId  "http://my-app" -Credential $pscredential -TenantId $tenantid
    ```

    Pokud chcete zjistit svou hodnotu TenantId, interaktivně se přihlaste a pak z předplatného zjistěte hodnotu TenantId.

    ```powershell
    Get-AzureRmSubscription
    ```

    ```
    Environment           : AzureCloud
    Account               : username@contoso.com
    TenantId              : XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
    SubscriptionId        : XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
    SubscriptionName      : My Production Subscription
    CurrentStorageAccount :
    ```

## <a name="log-in-to-another-cloud"></a>Přihlášení k jinému cloudu

Azure Cloud Services poskytují různá prostředí, která dodržují vládní nařízení různých zemí týkající se manipulace s daty. Pokud se váš účet Azure nachází v jednom z cloudů pro státní správu, musíte při přihlášení zadat prostředí. Pokud máte účet Azure například v cloudu v Číně, přihlásíte pomocí tohoto příkazu:

```powershell
Login-AzureRmAccount -EnvironmentName AzureChinaCloud
```

Seznam dostupných prostředí zobrazíte spuštěním tohoto příkazu:

```powershell
Get-AzureRmEnvironment | Select-Object Name
```

```
Name
----
AzureCloud
AzureChinaCloud
AzureUSGovernment
AzureGermanCloud
```

## <a name="learn-more-about-managing-azure-role-based-access"></a>Další informace o správě přístupu na základě rolí Azure

Další informace o ověřování a správě předplatných v Azure najdete v článku [Správa účtů, předplatných a správních rolí](/azure/active-directory/role-based-access-control-configure).

Rutiny prostředí Azure PowerShell pro správu rolí

* [Get-AzureRmRoleAssignment](/powershell/module/AzureRM.Resources/Get-AzureRmRoleAssignment)
* [Get-AzureRmRoleDefinition](/powershell/module/AzureRM.Resources/Get-AzureRmRoleDefinition)
* [New-AzureRmRoleAssignment](/powershell/module/AzureRM.Resources/New-AzureRmRoleAssignment)
* [New-AzureRmRoleDefinition](/powershell/module/AzureRM.Resources/New-AzureRmRoleDefinition)
* [Remove-AzureRmRoleAssignment](/powershell/module/AzureRM.Resources/Remove-AzureRmRoleAssignment)
* [Remove-AzureRmRoleDefinition](/powershell/module/AzureRM.Resources/Remove-AzureRmRoleDefinition)
* [Set-AzureRmRoleDefinition](/powershell/moduel/AzureRM.Resources/Set-AzureRmRoleDefinition)