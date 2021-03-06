---
title: Azure-snabbstart – Skapa ett nyckelvalv via CLI | Microsoft Docs
description: Snabbstart som visar hur du skapar ett Azure-nyckelvalv med hjälp av CLI:t
services: key-vault
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: ''
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.date: 04/16/2018
ms.author: barclayn
ms.openlocfilehash: aaf8b93a41399b7754fb458d7d1d278a64f82139
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/23/2018
---
# <a name="quickstart-create-an-azure-key-vault-using-the-cli"></a>Snabbstart: Skapa ett nyckelvalv med hjälp av CLI:t

Azure Key Vault är en molntjänst som fungerar som säkert lager för hemligheter. Du kan på ett säkert sätt lagra nycklar, lösenord, certifikat och andra hemligheter. Mer information om Key Vault finns i [översikten](key-vault-overview.md). Med Azure-CLI:t kan du skapa och hantera Azure-resurser med hjälp av kommandon eller skript. I den här artikeln skapar du ett nyckelvalv. I den här snabbstarten skapar du ett nyckelvalv. När du har gjort det kommer du att lagra en hemlighet.

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-try-it.md)]

Om du väljer att installera och använda CLI:t lokalt måste du köra Azure CLI version 2.0.4 eller senare. Kör `az --version` för att hitta versionen. Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).

När du ska logga in på Azure via CLI:t kan du skriva så här:

```azurecli
az login
```

Mer information om inloggningsalternativen via CLI finns i [Logga in med Azure CLI 2.0](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest)

## <a name="create-a-resource-group"></a>Skapa en resursgrupp

En resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras. I följande exempel skapas en resursgrupp med namnet *ContosoResourceGroup* på platsen *eastus*.

```azurecli
az group create --name 'ContosoResourceGroup' --location eastus
```

## <a name="create-a-key-vault"></a>Skapa en Key Vault-lösning

Därefter skapar du ett nyckelvalv i resursgruppen du skapade i föregående steg. Du måste ange en del information:

- I den här snabbstarten använder vi **Contoso-vault2**. Du måste ange ett unikt namn när du testar funktionen.
- Resursgruppnamnet **ContosoResourceGroup**.
- Platsen är **Östra USA**.

```azurecli
az keyvault create --name 'Contoso-Vault2' --resource-group 'ContosoResourceGroup' --location eastus
```

Utdata från denna cmdlet visar egenskaper för nyckelvalvet du precis skapade. Anteckna de två egenskaperna som visas nedan:

- **Valvnamn**: I det här exemplet är namnet **Contoso-vault2**. Du kommer att använda det här namnet i andra Key Vault-kommandon.
- **Valvets URI**: I det här exemplet är det https://contoso-vault2.vault.azure.net/. Program som använder ditt valv via dess REST-API måste använda denna URI.

Nu är ditt Azure-konto det enda kontot med behörighet att utföra åtgärder i det nya valvet.

## <a name="add-a-secret-to-key-vault"></a>Lägga till en hemlighet i Key Vault

När du ska lägga till en hemlighet i valvet behöver du bara utföra några ytterligare steg. Det här lösenordet kan användas av ett program. Lösenordet kallas **ExamplePassword** och vi lagrar värdet **Pa$$w0rd** i det.

Skriv kommandona nedan för att skapa en hemlighet i Key Vault med namnet **ExamplePassword**, där värdet **Pa$$w0rd** lagras:

```azurecli
az keyvault secret set --vault-name 'Contoso-Vault2' --name 'ExamplePassword' --value 'Pa$$w0rd'
```

Nu kan du referera till det här lösenordet som du lagt till i Azure Key Vault med hjälp av dess URI. Använd **https://ContosoVault.vault.azure.net/secrets/ExamplePassword** till att hämta aktuell version. 

Så här visar du värdet som finns i hemligheten som oformaterad text:

```azurecli
az keyvault secret show --name 'ExamplePassword' --vault-name 'Contoso-Vault2'
```

Nu har du skapat ett nyckelvalv, lagrat en hemlighet och hämtat den.

## <a name="clean-up-resources"></a>Rensa resurser

De andra snabbstarterna och självstudierna i den här samlingen bygger på den här snabbstarten. Om du planerar att fortsätta med efterföljande snabbstarter och självstudier kan du lämna kvar de här resurserna.
När du inte behöver resursgruppen längre kan du använda kommandot [az group delete](/cli/azure/group#delete) till att ta bort resursgruppen och alla relaterade resurser. Så här tar du bort resurserna:

```azurecli
az group delete --name ContosoResourceGroup
```

## <a name="next-steps"></a>Nästa steg

I den här snabbstarten har du skapat ett nyckelvalv och lagrat en hemlighet i det. Om du vill lära dig mer om Key Vault och hur du kan använda det med dina program fortsätter du till självstudien om att använda webbprogram med Key Vault.

> [!div class="nextstepaction"]
> [Använda Azure Key Vault från ett webbprogram](key-vault-use-from-web-application.md) Om du vill lära dig att läsa en hemlighet från Key Vault via ett webbprogram som använder [hanterade tjänstidentiteter](/active-directory/managed-service-identity/overview.md) fortsätter du med självstudien [Konfigurera ett Azure-webbprogram att läsa en hemlighet från Key Vault](tutorial-web-application-keyvault.md)
