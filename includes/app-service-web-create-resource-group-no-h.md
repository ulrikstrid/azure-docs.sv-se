---
title: ta med fil
description: ta med fil
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 26bfeed5315394b9030bff9c471dd5fbedca117f
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/06/2018
---
[!INCLUDE [resource group intro text](resource-group.md)]

Skapa i Cloud Shell en resursgrupp med kommandot [`az group create`](/cli/azure/group?view=azure-cli-latest#az_group_create). I följande exempel skapas en resursgrupp med namnet *myResourceGroup* på platsen *Europa, västra*. Om du vill se alla platser som stöds för App Service på **Kostnadsfri** nivå, kör du kommandot [`az appservice list-locations --sku FREE`](/cli/azure/appservice?view=azure-cli-latest#az_appservice_list_locations).

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

Du skapar vanligtvis din resursgrupp och resurserna i en region nära dig. 

När kommandot har slutförts visar JSON-utdata resursgruppens egenskaper.