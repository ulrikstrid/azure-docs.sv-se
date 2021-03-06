---
title: "I kursen får du lära dig hur du prenumererar på ett erbjudande på Azure-stacken | Microsoft Docs"
description: "Den här kursen visar hur du skapar en ny prenumeration på Azure-stacken tjänster och testa erbjudandet genom att skapa virtuella testdatorer."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: d2adda5613b9a10bc3314045b9d68b0f3196f19a
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/17/2018
---
# <a name="tutorial-create-and-test-a-subscription"></a>Självstudier: skapa och testa en prenumeration
Den här kursen visar hur du skapar en prenumeration som innehåller ett erbjudande och sedan testa den. För testet, som du ska logga in på den [användarportalen](https://portal.local.azurestack.external) som en moln-administratör kan prenumerera på erbjudandet och sedan skapa en virtuell dator.

> [!TIP]
> För mer en mer avancerad utvärderingsversionen, kan du [skapa en prenumeration för en viss användare](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm#create-a-subscription-as-a-cloud-operator) och sedan logga in som användaren i användarportalen. 

Den här kursen visar hur du prenumererar på det erbjudande som skapats i den [tidigare kursen](asdk-offer-services.md).

Vad får du lära dig:

> [!div class="checklist"]
> * Prenumerera på ett erbjudande 
> * Testa erbjudandet

## <a name="subscribe-to-an-offer"></a>Prenumerera på ett erbjudande
Om du vill prenumerera på ett erbjudande som en användare, måste du logga in på Azure-stacken användarportalen att identifiera de tjänster som har erbjudits av Azure Stack-operatorn.

1. Logga in på den [användarportalen](https://portal.local.azurestack.external) och på **skaffa en prenumeration**.

   ![Skaffa en prenumeration](media/asdk-subscribe-services/get-subscription.png)

2. Skriv ett namn för prenumerationen i fältet **Visningsnamn**. Klicka på **erbjuder** att välja en av de tillgängliga erbjudandena i den **väljer du ett erbjudande** avsnittet och klicka sedan på **skapa**.

   ![Skapa ett erbjudande](media/asdk-subscribe-services/create-subscription.png)

   > [!TIP]
   > Nu måste du uppdatera användarportalen om du vill börja använda din prenumeration.

3. Så här visar du just har skapat prenumerationen **fler tjänster**, klickar du på **prenumerationer**, klicka på den nya prenumerationen. När du prenumererar på ett erbjudande uppdatera portalen för att se om nya tjänster har tagits med i den nya prenumerationen. I det här exemplet **virtuella datorer** har lagts till.

   ![Visa prenumeration](media/asdk-subscribe-services/view-subscription.png)


## <a name="test-the-offer"></a>Testa erbjudandet
Logga in den [användarportalen](https://portal.local.azurestack.external), kan du testa erbjudandet genom att etablera en virtuell dator med hjälp av de nya funktionerna för prenumerationen. 

> [!NOTE]
> Det här testet kräver Windows Server 2016 Datacenter VM-avbildning som har lagts till i Azure-stacken marketplace i en [tidigare kursen](asdk-marketplace-item.md). 

1. Logga in på den [användarportalen](https://portal.local.azurestack.external).

2. I användarportalen, klickar du på **virtuella datorer** > **Lägg till** > **Windows Server 2016 Datacenter**, och klicka sedan på **skapa** .

3. I den **grunderna** skriver en **namn**, **användarnamn**, och **lösenord**, Välj en **prenumeration**, Skapa en **resursgruppen** (eller välj ett befintligt) och klicka sedan på **OK**.

4. I den **välja en storlek** klickar du på **A1 Standard**, och klicka sedan på **Välj**.  

5. I bladet inställningar acceptera standardinställningarna och klickar på **OK**.

6. I den **sammanfattning** klickar du på **OK** att skapa den virtuella datorn.  

7. Klicka för att visa den nya virtuella datorn **virtuella datorer**Sök sedan efter den nya virtuella datorn och klicka på dess namn.

    ![Alla resurser](media/asdk-subscribe-services/view-vm.png)

> [!NOTE]
> Distributionen av virtuella datorer tar några minuter att slutföra.


## <a name="next-steps"></a>Nästa steg

Vad du lärt dig i den här självstudiekursen:

> [!div class="checklist"]
> * Prenumerera på ett erbjudande 
> * Testa erbjudandet

Gå vidare till nästa kurs att lära dig hur du skapar en virtuell dator med en mall.

> [!div class="nextstepaction"]
> [Skapa en virtuell dator från en mall](asdk-create-vm-template.md)