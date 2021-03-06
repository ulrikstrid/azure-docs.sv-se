---
title: Snabbstart – Skapa din första Azure Container Instances-behållare med Azure Portal
description: I den här snabbstarten använder du Azure Portal för att distribuera en behållare i Azure Container Instances
services: container-instances
author: mmacy
manager: timlt
ms.service: container-instances
ms.topic: quickstart
ms.date: 04/02/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: cb0c8c5f5730ae1f7a2e9b38c3ef3e04ee8cde67
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/03/2018
---
# <a name="quickstart-create-your-first-container-in-azure-container-instances"></a>Snabbstart: Skapa din första behållare i Azure Container Instances

Med Azure Container Instances är det enkelt att skapa och hantera behållare i Azure. I den här snabbstarten skapar du en behållare i Azure och gör den tillgänglig på Internet med en offentlig IP-adress. Den här åtgärden genomförs via Azure Portal. Efter några klickningar visas det här i webbläsaren:

![App som distribuerats via Azure Container Instances visas i webbläsare][aci-portal-07]

## <a name="log-in-to-azure"></a>Logga in på Azure

Logga in på Azure Portal på http://portal.azure.com.

## <a name="create-a-container-instance"></a>Skapa en behållarinstans

Välj **Skapa en resurs** > **Behållare** > **Azure Container Instances**.

![Skapa en ny behållarinstans i Azure Portal][aci-portal-01]

Ange följande värden i textfälten **Behållarnamn**, **Behållaravbildning** och **Resursgrupp**. Lämna de övriga standardvärdena oförändrade och klicka på **OK**.

* Behållarnamn: `mycontainer`
* Behållaravbildning: `microsoft/aci-helloworld`
* Resursgrupp: `myResourceGroup`

![Konfigurera grundläggande inställningar för en ny behållarinstans i Azure Portal][aci-portal-03]

Du kan skapa både Windows- och Linux-behållare i Azure Container Instances. I den här snabbstarten använder vi standardinställningen **Linux** eftersom vi har angett en Linux-baserad behållare (`microsoft/aci-helloworld`) i föregående steg.

Lämna de övriga standardinställningarna **Konfiguration** oförändrade och klicka sedan på **OK** att bekräfta konfigurationen.

![Konfigurera en ny behållarinstans i Azure Portal][aci-portal-04]

När verifieringen är klar visas en sammanfattning av behållarinställningarna. Välj **OK** för att skicka din begäran om distribution av behållare.

![Sammanfattning av inställningar för en ny behållarinstans i Azure Portal][aci-portal-05]

När distributionen inleds visas en förloppsindikator i form av en ikon på instrumentpanelen i portalen. När distributionen är klar panelen uppdateras ikonen och indikerar istället behållargruppen **mycontainer-myc1**.

![Förloppsindikator under tillkomsten av en ny behållarinstans i Azure Portal][aci-portal-08]

Markera behållargruppen **mycontainer-myc1** för att visa egenskaperna för behållargruppen. Anteckna behållargruppens **IP-adress** och behållarens **TILLSTÅND**.

![Översikt över gruppbehållare i Azure-portalen][aci-portal-06]

När behållaren övergår till tillståndet **Körs** kan du navigera till IP-adressen som du antecknade i föregående steg om du vill visa programmet i din nya behållare.

![App som distribuerats via Azure Container Instances visas i webbläsare][aci-portal-07]

## <a name="delete-the-container"></a>Ta bort behållaren
När du är färdig med behållaren väljer du behållargruppen **mycontainer-myc1** och klickar på **Ta bort**.

![Ta bort behållarinstansen i Azure Portal][aci-portal-09]

Då visas en bekräftelseruta, välj **Ja** när du tillfrågas.

![Borttagningsbekräftelse för en behållarinstans i Azure Portal][aci-portal-10]

<!-- IMAGES -->
[aci-portal-01]: ./media/container-instances-quickstart-portal/qs-portal-01.png
<!--[aci-portal-02]: ./media/container-instances-quickstart-portal/qs-portal-02.png-->
[aci-portal-03]: ./media/container-instances-quickstart-portal/qs-portal-03.png
[aci-portal-04]: ./media/container-instances-quickstart-portal/qs-portal-04.png
[aci-portal-05]: ./media/container-instances-quickstart-portal/qs-portal-05.png
[aci-portal-06]: ./media/container-instances-quickstart-portal/qs-portal-06.png
[aci-portal-07]: ./media/container-instances-quickstart-portal/qs-portal-07.png
[aci-portal-08]: ./media/container-instances-quickstart-portal/qs-portal-08.png
[aci-portal-09]: ./media/container-instances-quickstart-portal/qs-portal-09.png
[aci-portal-10]: ./media/container-instances-quickstart-portal/qs-portal-10.png

## <a name="next-steps"></a>Nästa steg

I den här snabbstarten har du skapat en Azure-behållarinstans utifrån en avbildning som finns i en offentlig Docker Hub-lagringsplats. Om du vill försöka skapa en behållare på egen hand och distribuera den till Azure Container Instances via Azure Container Registry, går du vidare till självstudien för Azure Container Instances.

> [!div class="nextstepaction"]
> [Azure Container Instances-självstudier](./container-instances-tutorial-prepare-app.md)