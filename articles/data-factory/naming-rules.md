---
title: Regler för namngivning av enheter i Azure Data Factory | Microsoft Docs
description: Beskriver namnregler för enheter som Data Factory.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: bc5e801d-0b3b-48ec-9501-bb4146ea17f1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: shlo
ms.openlocfilehash: 0d1ff97aef7be7fa9f9f07f2743e1a1a9399e48a
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/23/2018
---
# <a name="azure-data-factory---naming-rules"></a>Azure Data Factory - namngivningsregler
Följande tabell innehåller namngivningsregler för Data Factory-artefakter.

> [!NOTE]
> Den här artikeln gäller för version 2 av Data Factory, som för närvarande är en förhandsversion. Om du använder version 1 av Data Factory-tjänsten, som är allmänt tillgänglig (GA), se [namngivningsregler i Data Factory version1](v1/data-factory-naming-rules.md).

| Namn | Unika namn | Kontroller |
|:--- |:--- |:--- |
| Data Factory |Unikt över Microsoft Azure. Namn är inte skiftlägeskänsliga, det vill säga `MyDF` och `mydf` referera till samma data factory. |<ul><li>Varje datafabriken är kopplad till en Azure-prenumeration.</li><li>Objektnamn måste börja med en bokstav eller en siffra och får innehålla endast bokstäver, siffror och streck (-).</li><li>Varje streck (-) måste föregås och följas av en bokstav eller en siffra. Streck i följd är inte tillåtna i behållarnamn.</li><li>Namnet kan innehålla 3-63 tecken.</li></ul> |
| Länkade tjänster/tabeller/Pipelines |Unikt med i en data factory. Namn är inte skiftlägeskänsliga. |<ul><li>Maximalt antal tecken i ett tabellnamn: 260.</li><li>Objektnamn måste börja med en bokstav, siffra eller ett understreck (_).</li><li>Följande tecken är inte tillåtna ”:”., ”+” ”,”?, ”/” ”, <” ”, >” ”, *”, ”%”, ”&” ”,:” ”,\\”</li><li>Streck (”-”) är inte tillåtna i namnen på länkade tjänster och endast datauppsättningar.</li></ul>  |
| Resursgrupp |Unikt över Microsoft Azure. Namn är inte skiftlägeskänsliga. | Mer information finns i [Azure namngivningsregler och begränsningar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions#naming-rules-and-restrictions). |

## <a name="next-steps"></a>Nästa steg
Lär dig hur du skapar datafabriker genom att följa instruktionerna i [Snabbstart: skapa en datafabrik](quickstart-create-data-factory-powershell.md) artikel. 