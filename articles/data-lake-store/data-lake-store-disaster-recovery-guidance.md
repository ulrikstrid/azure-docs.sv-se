---
title: Disaster recovery-vägledning för Azure Data Lake Store | Microsoft Docs
description: Riktlinjer för katastrofåterställning för Azure Data Lake Store
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/21/2018
ms.author: nitinme
ms.openlocfilehash: 9a2705a2372ea4f2fbea1e27bf675679dfda28f8
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/28/2018
---
# <a name="disaster-recovery-guidance-for-data-in-data-lake-store"></a>Disaster recovery-vägledning för data i Data Lake Store

Azure Data Lake Store ger lokalt redundant lagring (LRS). Därför kan är data i Azure Data Lake Store-konto känsligt för tillfälliga maskinvarufel inom en region via automatisk repliker. Detta säkerställer hållbarheten och hög tillgänglighet, vilket uppfyller SLA för Azure Data Lake Store. Den här artikeln innehåller råd om hur du ytterligare skydda dina data från sällsynta region hela avbrott eller oavsiktliga borttagningar.

## <a name="disaster-recovery-guidance"></a>Vägledning om haveriberedskap
Det är viktigt för varje kund att förbereda en plan för haveriberedskap. Läs informationen i den här artikeln för att skapa din plan för katastrofåterställning. Här finns resurser som hjälper dig att skapa ett eget schema.

* [Haveriberedskap och hög tillgänglighet för Azure-program](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)
* [Azure-återhämtning, tekniska riktlinjer](../resiliency/resiliency-technical-guidance.md)

### <a name="best-practices"></a>Bästa praxis
Vi rekommenderar att du kopierar dina viktigaste data till ett annat Data Lake Store-konto i en annan region med en frekvens som är anpassad till din haverberedskapsplan. Det finns en mängd olika metoder för att kopiera data, inklusive [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShell](data-lake-store-get-started-powershell.md) eller [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md). Azure Data Factory är användbart för att skapa och distribuera pipelines för datarörlighet regelbundet.

Om ett nationellt strömavbrott uppstår kan du sedan hämta dina data från regionen där de kopierades. Du kan övervaka [Hälsoinstrumentpanelen för Azure](https://azure.microsoft.com/status/) för att bestämma statusen för Azure-tjänsten i hela världen.

## <a name="data-corruption-or-accidental-deletion-recovery-guidance"></a>Skadade data eller vägledning för återställning av oavsiktligt borttagna data
Azure Data Lake Store skyddar data med hjälp av automatiska repliker. Detta utgör dock inte ett skydd mot att ditt program (eller utvecklare/användare) skadar eller oavsiktligen raderar data.

### <a name="best-practices"></a>Bästa praxis
För att förhindra oavsiktlig borttagning rekommenderar vi att du först ställer in rätt åtkomstprinciper för ditt Data Lake Store-konto.  Det innebär att tillämpa [lås för Azure-resurser](../azure-resource-manager/resource-group-lock-resources.md) för att låsa viktiga resurser samt tillämpa åtkomstkontroll på konto- och filnivå med de tillgängliga [säkerhetsfunktionerna i Data Lake Store](data-lake-store-security-overview.md). Vi rekommenderar dessutom att du skapar regelbundna kopior av dina viktigaste data med [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShell](data-lake-store-get-started-powershell.md) eller [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md) i ett annat Data Lake Store-konto eller Azure-prenumeration.  Detta kan användas för att återställa från skadade data eller en borttagning. Azure Data Factory är användbart för att skapa och distribuera pipelines för datarörlighet regelbundet.

Organisationer kan även aktivera [Diagnostisk loggning](data-lake-store-diagnostic-logs.md) för sitt konto för att samla in granskningshistorik för dataåtkomst till Azure Data Lake Store som ger information om vem som har tagit bort eller uppdaterat en fil.

## <a name="next-steps"></a>Nästa steg
* [Kom igång med Azure Data Lake Store](data-lake-store-get-started-portal.md)
* [Säkra data i Data Lake Store](data-lake-store-secure-data.md)

