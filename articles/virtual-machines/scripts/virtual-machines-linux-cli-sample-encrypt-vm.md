---
title: Skriptexempel för Azure CLI – Kryptera en virtuell Linux-dator | Microsoft Docs
description: Skriptexempel för Azure CLI – Kryptera en virtuell Linux-dator
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/15/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: f5362a323703d895d98f95e0dddebcbbaa089a87
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/09/2018
---
# <a name="encrypt-a-linux-virtual-machine-in-azure"></a>Kryptera en virtuell Linux-dator i Azure

Det här skriptet skapar ett säkert Azure Key Vault, krypteringsnycklar, Azure Active Directory-tjänstens huvudnamn och en virtuell Linux-dator (VM). Den virtuella datorn krypteras sedan med hjälp av krypteringsnyckeln från Key Vault och autentiseringsuppgifterna för tjänstens huvudnamn.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/encrypt-disks/encrypt_vm.sh "Encrypt VM disks")]

## <a name="clean-up-deployment"></a>Rensa distribution 

Kör följande kommando för att ta bort resursgruppen, den virtuella datorn och alla relaterade resurser.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Förklaring av skript

I det här skriptet används följande kommandon för att skapa en resursgrupp, Azure Key Vault, tjänstens huvudnamn, en virtuell dator och alla relaterade resurser. Varje kommando i tabellen länkar till kommandospecifik dokumentation.

| Kommando | Anteckningar |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Skapar en resursgrupp där alla resurser lagras. |
| [az keyvault create](https://docs.microsoft.com/cli/azure/keyvault#az_keyvault_create) | Skapar ett Azure Key Vault för lagring av säkra data, till exempel krypteringsnycklar. |
| [az keyvault key create](https://docs.microsoft.com/cli/azure/keyvault/key#az_keyvault_key_create) | Skapar en krypteringsnyckel i Key Vault. |
| [az ad sp create-for-rbac](https://docs.microsoft.com/cli/azure/ad/sp#az_ad_sp_create_for_rbac) | Skapar ett huvudnamn för Azure Active Directory-tjänsten för att autentisera och kontrollera åtkomst till krypteringsnycklarna på ett säkert sätt. |
| [az keyvault set-policy](https://docs.microsoft.com/cli/azure/keyvault#az_keyvault_set_policy) | Anger behörigheter för Key Vault så att tjänstens huvudnamn får åtkomst till krypteringsnycklarna. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm#az_vm_create) | Skapar den virtuella datorn och ansluter den till nätverkskortet, ett virtuellt nätverk, ett undernät och en NSG. Det här kommandot anger även den virtuella datoravbildning som ska användas samt administrativa autentiseringsuppgifter.  |
| [az vm encryption enable](https://docs.microsoft.com/cli/azure/vm/encryption#az_vm_encryption_enable) | Aktiverar kryptering på en virtuell dator med hjälp av autentiseringsuppgifterna för tjänstens huvudnamn och krypteringsnyckeln. |
| [az vm encryption show](https://docs.microsoft.com/cli/azure/vm/encryption#az_vm_encryption_show) | Visar status för krypteringsprocessen för den virtuella datorn. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Tar bort en resursgrupp, inklusive alla kapslade resurser. |

## <a name="next-steps"></a>Nästa steg

Mer information om Azure CLI finns i [Azure CLI-dokumentationen](https://docs.microsoft.com/cli/azure).

Ytterligare CLI-skriptexempel för virtuella datorer finns i [dokumentationen för virtuella Azure Linux-datorer](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
