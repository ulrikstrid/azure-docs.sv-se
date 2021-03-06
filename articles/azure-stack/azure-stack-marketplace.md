---
title: Publicera en anpassad marketplace-objektet i Azure-stacken (moln operator) | Microsoft Docs
description: Som operatör Azure Stack lär du dig hur du publicerar en anpassad marketplace-objektet i Azure-stacken.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 60871cbb-eed2-433c-a76d-d605c7aec06c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/06/2018
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: 588da055d06d7e63510085ff48169f3ea756c53c
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/06/2018
---
# <a name="the-azure-stack-marketplace-overview"></a>Azure-stacken Marketplace-översikt

*Gäller för: Azure Stack integrerat system och Azure-stacken Development Kit*

Marketplace är en uppsättning tjänster, program och resurser som är anpassade för Azure-stacken. Resurser omfattar nätverk, virtuella datorer, lagring och så vidare. Användare komma här för att skapa nya resurser och distribuera nya program. Du kan se det som en shopping katalog där användare kan bläddra och välj de objekt som de vill använda. Om du vill använda en Marketplace-objektet, måste användare prenumerera på ett erbjudande som ger dem åtkomst till objektet.

Som operatör Azure Stack kan du bestämma vilka objekt du lägger till (publicera) på Marketplace. Du kan publicera t.ex. databaser, Apptjänster och så vidare. Publicering gör dem synliga för alla användare. Du kan publicera anpassade objekt som du skapar. Du kan också publicera objekt från en växande [lista över Azure Marketplace-objekt](azure-stack-marketplace-azure-items.md). När du publicerar ett objekt på Marketplace kan användare se den inom fem minuter.

Klicka för att öppna Marketplace **ny**.

![](media/azure-stack-publish-custom-marketplace-item/image1.png)

## <a name="marketplace-items"></a>Marketplace-objekt
Ett Azure-stacken Marketplace-objekt är en tjänst, program eller resurs som användarna kan ladda ned och använda. Alla Azure-stacken Marketplace-objekt är synliga för alla användare, inklusive administrativa element som planer och erbjudanden. Dessa objekt behöver inte ha en prenumeration på vyn, men är icke-fungerande till användare.

Varje Marketplace-objekt har:

* En Azure Resource Manager-mall för resursetablering
* Metadata, som strängar, ikoner och andra marknadsföringsmaterial
* Formatering information för att visa objektet i portalen

Alla objekt som har publicerats till Marketplace använder Azure Gallery-paket (.azpkg)-formatet. Lägg till distribution eller runtime resurser (till exempel kod zip-filer med programvaran eller avbildningar av virtuella datorer) till Azure-stacken separat, inte som en del av Marketplace-objektet. 

Med version 1803 och senare konverterar Azure Stack bilder till sparse-filer när de hämtar från Azure eller när du överför anpassade avbildningar. Den här processen lägger på tid när du lägger till en bild, men sparar utrymme och snabbare att distribuera dessa avbildningar. Konverteringen gäller bara för nya bilder.  Befintliga avbildningar ändras inte. 

## <a name="next-steps"></a>Nästa steg
[Skapa och publicera en Marketplace-objektet](azure-stack-create-and-publish-marketplace-item.md)

