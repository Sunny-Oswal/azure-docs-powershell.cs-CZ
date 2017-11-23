---
title: "Zadávání dotazů na prostředky Azure a formátování výsledků | Dokumentace Microsoftu"
description: "Zde se dozvíte, jak zadávat dotazy na prostředky v Azure a jak formátovat výsledky."
services: azure
author: sdwheeler
ms.author: sewhee
manager: carmonm
ms.product: azure
ms.service: azure-powershell
ms.devlang: powershell
ms.topic: conceptual
ms.date: 03/30/2017
ms.openlocfilehash: 93a031ce90352286bb1a5e01dc65e6db7cbe5c7e
ms.sourcegitcommit: 358d260a867c6ee2e8700b91f776ba4f1b0e24a5
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/11/2017
---
# <a name="querying-for-azure-resources"></a><span data-ttu-id="c9a63-103">Zadávání dotazů na prostředky Azure</span><span class="sxs-lookup"><span data-stu-id="c9a63-103">Querying for Azure resources</span></span>

<span data-ttu-id="c9a63-104">Dotazování v prostředí PowerShell můžete dokončit s použitím integrovaných rutin.</span><span class="sxs-lookup"><span data-stu-id="c9a63-104">Querying in PowerShell can be completed by using built-in cmdlets.</span></span> <span data-ttu-id="c9a63-105">V prostředí PowerShell mají názvy rutin tvar  **_sloveso-podstatné jméno_**.</span><span class="sxs-lookup"><span data-stu-id="c9a63-105">In PowerShell, cmdlet names take the form of **_Verb-Noun_**.</span></span> <span data-ttu-id="c9a63-106">Rutiny používající sloveso  **_Get_**  jsou rutiny pro dotazování.</span><span class="sxs-lookup"><span data-stu-id="c9a63-106">The cmdlets using the verb **_Get_** are the query cmdlets.</span></span> <span data-ttu-id="c9a63-107">Rutiny s podstatnými jmény jsou typy prostředků Azure, se kterými se pracuje prostřednictvím rutin se slovesy.</span><span class="sxs-lookup"><span data-stu-id="c9a63-107">The cmdlet nouns are the types of Azure resources that are acted upon by the cmdlet verbs.</span></span>


## <a name="selecting-simple-properties"></a><span data-ttu-id="c9a63-108">Výběr jednoduchých vlastností</span><span class="sxs-lookup"><span data-stu-id="c9a63-108">Selecting simple properties</span></span>

<span data-ttu-id="c9a63-109">V prostředí Azure PowerShell je pro každou rutinu definováno výchozí formátování.</span><span class="sxs-lookup"><span data-stu-id="c9a63-109">Azure PowerShell has default formatting defined for each cmdlet.</span></span> <span data-ttu-id="c9a63-110">Nejběžnější vlastnosti pro každý typ prostředku se automaticky zobrazují ve formátu tabulky nebo seznamu.</span><span class="sxs-lookup"><span data-stu-id="c9a63-110">The most common properties for each resource type are displayed in a table or list format automatically.</span></span> <span data-ttu-id="c9a63-111">Další informace o formátování výstupu najdete v tématu [Formátování výsledků dotazu](formatting-output.md).</span><span class="sxs-lookup"><span data-stu-id="c9a63-111">For more information about formatting output, see [Formatting query results](formatting-output.md).</span></span>

<span data-ttu-id="c9a63-112">Pomocí rutiny `Get-AzureRmVM` můžete zadat dotaz na seznam virtuálních počítačů ve vašem účtu.</span><span class="sxs-lookup"><span data-stu-id="c9a63-112">Use the `Get-AzureRmVM` cmdlet to query for a list of VMs in your account.</span></span>

```powershell
Get-AzureRmVM
```

<span data-ttu-id="c9a63-113">Výchozí výstup je automaticky formátován jako tabulka.</span><span class="sxs-lookup"><span data-stu-id="c9a63-113">The default output is automatically formatted as a table.</span></span>

```
ResourceGroupName          Name   Location          VmSize  OsType              NIC ProvisioningState
-----------------          ----   --------          ------  ------              --- -----------------
MYWESTEURG        MyUnbuntu1610 westeurope Standard_DS1_v2   Linux myunbuntu1610980         Succeeded
MYWESTEURG          MyWin2016VM westeurope Standard_DS1_v2 Windows   mywin2016vm880         Succeeded
```

<span data-ttu-id="c9a63-114">Pomocí rutiny `Select-Object` můžete vybrat konkrétní vlastnosti, které vás zajímají.</span><span class="sxs-lookup"><span data-stu-id="c9a63-114">The `Select-Object` cmdlet can be used to select the specific properties that are interesting to you.</span></span>

```powershell
Get-AzureRmVM | Select Name,ResourceGroupName,Location
```

```
Name          ResourceGroupName Location
----          ----------------- --------
MyUnbuntu1610 MYWESTEURG        westeurope
MyWin2016VM   MYWESTEURG        westeurope
```

## <a name="selecting-complex-nested-properties"></a><span data-ttu-id="c9a63-115">Výběr komplexních vnořených vlastností</span><span class="sxs-lookup"><span data-stu-id="c9a63-115">Selecting complex nested properties</span></span>

<span data-ttu-id="c9a63-116">Pokud je vlastnost, kterou chcete vybrat, vnořená hluboko ve výstupu JSON, je třeba zadat úplnou cestu k této vnořené vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="c9a63-116">If the property you want to select is nested deep in the JSON output you need to supply the full path to that nested property.</span></span> <span data-ttu-id="c9a63-117">Následující příklad ilustruje, jak vybrat název virtuálního počítače a typ operačního systému v rámci rutiny `Get-AzureRmVM`.</span><span class="sxs-lookup"><span data-stu-id="c9a63-117">The following example shows how to select the VM Name and the OS type from the `Get-AzureRmVM` cmdlet.</span></span>

```powershell
Get-AzureRmVM | Select Name,@{Name='OSType'; Expression={$_.StorageProfile.OSDisk.OSType}}
```

```
Name           OSType
----           ------
MyUnbuntu1610   Linux
MyWin2016VM   Windows
```

## <a name="filter-result-using-the-where-object-cmdlet"></a><span data-ttu-id="c9a63-118">Filtrování výsledků s použitím rutiny Where-Object</span><span class="sxs-lookup"><span data-stu-id="c9a63-118">Filter result using the Where-Object cmdlet</span></span>

<span data-ttu-id="c9a63-119">Rutina `Where-Object` umožňuje filtrování výsledků na základě kterékoli hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="c9a63-119">The `Where-Object` cmdlet allows you to filter the result based on any property value.</span></span> <span data-ttu-id="c9a63-120">V následujícím příkladu vybere filtr jenom virtuální počítače, které mají v názvu text „RGD“.</span><span class="sxs-lookup"><span data-stu-id="c9a63-120">In the following example, the filter selects only VMs that have the text "RGD" in their name.</span></span>

```powershell
Get-AzureRmVM | Where ResourceGroupName -like RGD* | Select ResourceGroupName,Name
```

```
ResourceGroupName  Name
-----------------  ----
RGDEMO001          KBDemo001VM
RGDEMO001          KBDemo020
```

<span data-ttu-id="c9a63-121">V dalším příkladu jsou jako výsledky vráceny virtuální počítače s nastavením vmSize 'Standard_DS1_V2'.</span><span class="sxs-lookup"><span data-stu-id="c9a63-121">With the next example, the results will return the VMs that have the vmSize 'Standard_DS1_V2'.</span></span>

```powershell
Get-AzureRmVM | Where vmSize -eq Standard_DS1_V2
```

```
ResourceGroupName          Name     Location          VmSize  OsType              NIC ProvisioningState
-----------------          ----     --------          ------  ------              --- -----------------
MYWESTEURG        MyUnbuntu1610   westeurope Standard_DS1_v2   Linux myunbuntu1610980         Succeeded
MYWESTEURG          MyWin2016VM   westeurope Standard_DS1_v2 Windows   mywin2016vm880         Succeeded
```