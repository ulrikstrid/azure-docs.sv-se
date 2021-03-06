---
title: Förstå Azure reserverade virtuella instanser rabatt program | Microsoft Docs
description: Lär dig hur Azure reserverade VM-instans rabatt tillämpas på virtuella datorer som körs.
services: billing
documentationcenter: ''
author: vikramdesai01
manager: vikdesai
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/10/2017
ms.author: vikdesai
ms.openlocfilehash: 6e551fddfc5ba8924cd8c71a1775307e7569b847
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/04/2018
---
# <a name="understand-how-the-reserved-virtual-machine-instance-discount-is-applied"></a>Förstå hur reserverade virtuella datorinstans rabatt används
När du köper reserverade VM-instans tillämpas reservation rabatten automatiskt på virtuella datorer som matchar de attribut och kvantitet för reservation. En reservation omfattar infrastrukturkostnader för de virtuella datorerna. I följande tabell visas kostnaderna för den virtuella datorn när du har köpt en reservation. I samtliga fall måste du debiteras för lagring och nätverk i de normala takt.

| Typ av virtuell dator  | Tillägg med reservation |    
|-----------------------|--------------------------------------------| 
|Virtuella Linux-datorer utan ytterligare programvara | Reservation omfattar VM-infrastrukturkostnader.|
|Linux virtuella datorer med programvara avgifter (till exempel Red Hat) | Reservation omfattar kostnader med infrastruktur. Du debiteras för ytterligare programvara.|
|Virtuella Windows-datorer utan ytterligare programvara |Reservation omfattar kostnader med infrastruktur. Du debiteras för Windows-program.|
|Virtuella Windows-datorer med ytterligare programvara (till exempel SQLServer) | Reservation omfattar kostnader med infrastruktur. Du debiteras för Windows-program och för ytterligare programvara.|
|Virtuella Windows-datorer med [Hybridrapportering i Azure-förmån](https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing) | Reservation omfattar kostnader med infrastruktur. Windows-programvarukostnader omfattas av Hybrid-förmån för Azure. Ytterligare programvara debiteras separat.| 

## <a name="application-of-reservation-discount-to-non-windows-vms"></a>Program för reservation rabatt till icke - Windows-datorer
 Reservation rabatt tillämpas på VM-instanser som körs på timbasis. Reservationer som du har köpt matchas Usage sänds av de virtuella datorerna som körs att tillämpa reservation rabatt. För virtuella datorer som fungerar som en fullständig timme, fylls reservationen från andra virtuella datorer som inte använder en reservation, inklusive virtuella datorer som körs samtidigt. Programmet reservation för virtuella datorer i en timme är låst i slutet av en timme. En virtuell dator körs inte i en timma eller samtidiga virtuella datorer inom en timme inte fyller timmes reservation underutnyttjade reservationen för den timmen. Följande diagram illustrerar tillämpningen av en fakturerbar användning av VM-reservation. Bilden är baserad på en reservation inköp och två matchande VM-instanser.

![Reserverade VM-instans-program](media/billing-reserved-vm-instance-application/billing-reserved-vm-instance-application.png)

1.  Alla användning är över den reserverade Virtuella Datorinstansen linjen hämtar debiteras enligt regelbundna betalning per användning priser. Du är inte för användning under raden reserverade VM-instanser eftersom det redan har betalats som en del av reservation inköp.
2.  Instansen 1 körs 0,75 timmar timmen 1 och 2-instans som körs i 0,5 timmar. Total användning för timme 1 är 1,25 timmar. Du debiteras betalning per användning priser för återstående 0.25 timmar.
3.  Båda instanser kördes för 2 respektive timmarsformat 3, 1 timme. En instans täcks av reservationen och den andra debiteras med betalning per användning priser.
4.  För timme 4, 1-instans som körs i 0,5 timmar och 2-instans som körs i en timme. Instansen 1 omfattas fullständigt av reservationen och 0,5 instans 2 timmar som omfattas. Du är debiteras hastigheten med betalning per användning för de återstående 0,5 timmarna.

För att förstå och visa tillämpningen av dina reservationer i fakturering användningsrapporter, se [förstå reserverade VM-instans användning](https://go.microsoft.com/fwlink/?linkid=862757).

## <a name="application-of-reservation-discount-to-windows-vms"></a>Användning av reservation rabatt på virtuella Windows-datorer
När du kör Windows VM-instanser används reservationen så att den täcker kostnader med infrastruktur. Användning av VM-infrastrukturkostnader reservationen för virtuella Windows-datorer är samma som för icke - Windows-datorer. Du debiteras separat för Windows-program på grundval av per vCPU. Se [Windows programvarukostnader med reservationer](https://go.microsoft.com/fwlink/?linkid=862756). Du kan omfatta din Windows licensieringskostnader med [Azure Hybrid-förmån för Windows Server] (https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing).

## <a name="next-steps"></a>Nästa steg
Mer information om reserverade instanser för virtuella datorer, finns i följande artiklar.

- [Förskottsbetala virtuella datorer med reserverade VM-instanser](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Hantera reserverade virtuella Datorinstanser](billing-manage-reserved-vm-instance.md)
- [Spara pengar på virtuella datorer med den reserverade virtuella instanser](billing-save-compute-costs-reservations.md)
- [Förstå reserverade användning för din prenumeration med användningsbaserad betalning](billing-understand-reserved-instance-usage.md)
- [Förstå reserverade användning för Enterprise-registrering](billing-understand-reserved-instance-usage-ea.md)
- [Windows programvarukostnader ingår inte i reserverade instanser](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-support"></a>Behöver du hjälp? Kontakta supporten

Om du behöver hjälp, [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) få snabbt lösa problemet.
