---
title: Azure Network Watcher Agent virtuella tillägget för Linux | Microsoft Docs
description: Distribuera Network Watcher Agent på Linux-dator som använder ett tillägg för virtuell dator.
services: virtual-machines-linux
documentationcenter: ''
author: gurudennis
manager: amku
editor: ''
tags: azure-resource-manager
ms.assetid: 5c81e94c-e127-4dd2-ae83-a236c4512345
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/14/2017
ms.author: dennisg
ms.openlocfilehash: 5a33f183470ec3879344f0cfe335bab38f9ff30f
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/03/2018
---
# <a name="network-watcher-agent-virtual-machine-extension-for-linux"></a>Nätverk Watcher Agent tillägg för virtuell dator för Linux

## <a name="overview"></a>Översikt

[Azure Nätverksbevakaren](/azure/network-watcher/) är en prestanda övervakning, diagnostik och analytics nätverkstjänst som tillåter övervakning för Azure-nätverk. Tillägget för virtuell dator Network Watcher Agent är ett krav för några av de Nätverksbevakaren funktionerna på virtuella Azure-datorer. Detta omfattar att samla in nätverkstrafik på begäran och andra avancerade funktioner.

Det här dokumentet beskriver de plattformar som stöds och distributionsalternativ för tillägget för virtuell dator Network Watcher Agent för Linux. Installationen av agenten inte störa eller kräver en omstart av den virtuella datorn.

## <a name="prerequisites"></a>Förutsättningar

### <a name="operating-system"></a>Operativsystem

Tillägget Network Watcher Agent kan köras mot dessa Linux-distributioner:

| Distribution | Version |
|---|---|
| Ubuntu | 16.04 LTS, 14.04 LTS och 12.04 LTS |
| Debian | 7 och 8 |
| Redhat | 6 och 7 |
| Oracle Linux | 6.8 + och 7 |
| SUSE Linux Enterprise Server | 11 och 12 |
| OpenSUSE Leap | 42.3 + |
| CentOS | 6.5 + och 7 |
| CoreOS | 899.17.0+ |

Observera att virtuell CoreOS inte stöds just nu.

### <a name="internet-connectivity"></a>Internetanslutning

Några av funktioner som Network Watcher Agent kräver att den virtuella måldatorn är ansluten till Internet. Inte kan upprätta utgående anslutningar vissa Network Watcher Agent-funktioner kan fungera helt eller inte tillgänglig. Mer information finns i [Nätverksbevakaren dokumentationen](/azure/network-watcher/).

## <a name="extension-schema"></a>Tilläggsschema

Följande JSON visar schemat för tillägget Network Watcher Agent. Tillägget varken kräver inte heller stöder alla inställningar som anges av användaren just nu och förlitar sig på en standardkonfiguration.

```json
{
    "type": "extensions",
    "name": "Microsoft.Azure.NetworkWatcher",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Azure.NetworkWatcher",
        "type": "NetworkWatcherAgentLinux",
        "typeHandlerVersion": "1.4",
        "autoUpgradeMinorVersion": true
    }
}
```

### <a name="property-values"></a>Egenskapsvärden

| Namn | Värdet / exempel |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Publisher | Microsoft.Azure.NetworkWatcher |
| typ | NetworkWatcherAgentLinux |
| typeHandlerVersion | 1.4 |

## <a name="template-deployment"></a>Malldistribution

Azure VM-tillägg kan distribueras med Azure Resource Manager-mallar. JSON-schema som beskrivs i föregående avsnitt kan användas i en Azure Resource Manager-mall för att köra tillägget Network Watcher Agent under en Azure Resource Manager för malldistribution.

## <a name="azure-cli-deployment"></a>Azure CLI-distribution

Azure CLI kan användas för att distribuera Network Watcher Agent VM-tillägget till en befintlig virtuell dator.

```azurecli
azure vm extension set myResourceGroup1 myVM1 NetworkWatcherAgentLinux Microsoft.Azure.NetworkWatcher 1.4
```

## <a name="troubleshooting-and-support"></a>Felsökning och support

### <a name="troubleshooting"></a>Felsökning

Data om tillståndet för distributioner av tillägget kan hämtas från Azure-portalen och genom att använda Azure CLI. Om du vill se distributionsstatusen för tillägg för en viss virtuell dator, kör du följande kommando med hjälp av Azure CLI.

```azurecli
azure vm extension get myResourceGroup1 myVM1
```

Tillägget utförande-utdatan loggas till filer som finns i följande katalog:

`
/var/log/azure/Microsoft.Azure.NetworkWatcher.NetworkWatcherAgentLinux/
`

### <a name="support"></a>Support

Om du behöver mer hjälp när som helst i den här artikeln Nätverksbevakaren dokumentationen eller kontakta Azure-experter på den [MSDN Azure och Stack Overflow-forum](https://azure.microsoft.com/support/forums/). Alternativt kan du lagra en incident i Azure-supporten. Gå till den [Azure supportwebbplats](https://azure.microsoft.com/support/options/) och välja Get support. Information om hur du använder Azure stöder finns i [vanliga frågor om Microsoft Azure-supporten](https://azure.microsoft.com/support/faq/).
