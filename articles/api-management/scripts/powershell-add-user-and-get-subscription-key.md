---
title: Skriptexempel för Azure PowerShell – Lägga till en användare | Microsoft Docs
description: Skriptexempel för Azure PowerShell – Lägga till en användare
services: api-management
documentationcenter: ''
author: juliako
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.devlang: na
ms.topic: sample
ms.date: 11/16/2017
ms.author: apimpm
ms.custom: mvc
ms.openlocfilehash: 49b2ed42f0fedde06dd452bd56580a53b0170259
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/19/2018
---
# <a name="add-a-user"></a>Lägga till en användare

Det här skriptexemplet skapar en användare i API Management och hämtar en prenumerationsnyckel.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Om du väljer att installera och använda PowerShell lokalt kräver den här självstudien Azure PowerShell-modul version 3.6 eller senare. Kör ` Get-Module -ListAvailable AzureRM` för att hitta versionen. Om du behöver uppgradera kan du läsa [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps) (Installera Azure PowerShell-modul). Om du kör PowerShell lokalt måste du också köra `Connect-AzureRmAccount` för att skapa en anslutning till Azure.

## <a name="sample-script"></a>Exempelskript

[!code-powershell[main](../../../powershell_scripts/api-management/add-user-and-get-subscription-key/add_a_user_and_get_a_subscriptionKey.ps1 "Add a user")]

## <a name="clean-up-resources"></a>Rensa resurser

När resursgruppen inte längre behövs kan du använda kommandot [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) för att ta bort resursgruppen och alla relaterade resurser.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Nästa steg

Mer information om Azure PowerShell-modulen finns i [Azure PowerShell-dokumentationen](https://docs.microsoft.com/powershell/azure/overview).

Ytterligare Azure Powershell-exempel för Azure API Management finns i [PowerShell-exemplen](../powershell-samples.md).
