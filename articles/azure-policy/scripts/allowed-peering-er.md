---
title: "Exempel för Azure princip-json - tillåtna peeringplatsen för Express Route | Microsoft Docs"
description: "Den här principen kräver att Express vägar Använd angetts peering platser."
services: azure-policy
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 
ms.service: azure-policy
ms.devlang: 
ms.topic: sample
ms.tgt_pltfrm: 
ms.workload: 
ms.date: 10/30/2017
ms.author: banders
ms.custom: mvc
ms.openlocfilehash: 218d06d7595a8a2d937afa36682f67baf1995bd5
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/13/2017
---
# <a name="allowed-peering-location-for-express-route"></a>Tillåtna peeringplatsen för Express Route

Den här principen kräver att Express vägar Använd angetts peering platser. Du anger en matris med tillåtna peering platser.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>Exempelmall

[!code-json[main](../../../policy-templates/samples/Network/express-route-peeringLocation/azurepolicy.json "Allowed Peering Location for Express Route")]

Du kan distribuera den här mallen med hjälp av den [Azure-portalen](#deploy-with-the-portal), med [PowerShell](#deploy-with-powershell) eller med den [Azure CLI](#deploy-with-azure-cli).

## <a name="deploy-with-the-portal"></a>Distribuera med portalen

[![Distribuera till Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?feature.customportal=false&microsoft_azure_policy=true&microsoft_azure_policy_policyinsights=true&feature.microsoft_azure_security_policy=true&microsoft_azure_marketplace_policy=true#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FNetwork%2Fexpress-route-peeringLocation%2Fazurepolicy.json)

## <a name="deploy-with-powershell"></a>Distribuera med PowerShell

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

```powershell
$definition = New-AzureRmPolicyDefinition -Name "express-route-peeringLocation" -DisplayName "Allowed Peering Location for Express Route" -description "This policy enables you to specify a set of allowed peering location for express route" -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/express-route-peeringLocation/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/express-route-peeringLocation/azurepolicy.parameters.json' -Mode All
$definition
$assignment = New-AzureRMPolicyAssignment -Name <assignmentname> -Scope <scope>  -listOfLocations <Allowed Peering Locations> -PolicyDefinition $definition
$assignment
```

### <a name="clean-up-powershell-deployment"></a>Rensa PowerShell-distribution

Kör följande kommando för att ta bort resursgruppen, virtuell dator och alla relaterade resurser.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="deploy-with-azure-cli"></a>Distribuera med Azure CLI

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

```azurecli-interactive
az policy definition create --name 'express-route-peeringLocation' --display-name 'Allowed Peering Location for Express Route' --description 'This policy enables you to specify a set of allowed peering location for express route' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/express-route-peeringLocation/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/express-route-peeringLocation/azurepolicy.parameters.json' --mode All

az policy assignment create --name <assignmentname> --scope <scope> --policy "express-route-peeringLocation"
```

### <a name="clean-up-azure-cli-deployment"></a>Rensa Azure CLI-distribution

Kör följande kommando för att ta bort resursgruppen, virtuell dator och alla relaterade resurser.

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Nästa steg

- Nya Azure princip mallen exempel finns på [mallar för Azure princip](../json-samples.md).
