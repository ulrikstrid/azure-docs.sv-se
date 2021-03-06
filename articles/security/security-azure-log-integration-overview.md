---
title: Integrera loggar från Azure-resurser med SIEM-system | Microsoft Docs
description: Läs mer om Azure Log Integration, de viktigaste funktionerna och hur det fungerar.
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TerryLanfear
ms.assetid: 9c1346e1-baf8-4975-b2f2-42ae05b2dc0a
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/15/2018
ms.author: TomSh
ms.custom: azlog
ms.openlocfilehash: 6d91692a64a4d3def80990a439fe0a0898bf2f09
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/23/2018
---
# <a name="introduction-to-azure-log-integration"></a>Introduktion till Azure logganalys-integrering

Du kan använda Azure Log-integrering för att integrera loggarna från Azure-resurser med dina lokala Security Information and Event Management SIEM ()-system. Använd Azure Log-integrering bara om en koppling till [Azure-Monitor](../monitoring-and-diagnostics/monitoring-get-started.md) är inte tillgänglig från SIEM-leverantör.

Den bästa metoden för integrerande Azure loggar är med SIEM leverantörens Azure-Monitor-anslutningen. För att använda anslutningstjänsten, följ instruktionerna i [övervakaren dataströmmen övervakning för data händelsehubbar](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md). 

Om din SIEM-leverantör inte ger en koppling till Azure-Monitor, kan du att kunna använda Azure Log-integrering som en tillfällig lösning förrän en anslutning är tillgänglig. Azure Log-integrering är ett alternativ om Azure Log Integration stöder SIEM.

> [!IMPORTANT]
> Om ditt primära intresse samlar in loggar för virtuell dator, inkluderar det här alternativet i sitt lösning i de flesta SIEM-leverantörer. Med SIEM leverantörens connector är alltid bra alternativ.

Azure Log-integrering samlar in Windows-händelser från Windows Loggboken, [Azure aktivitetsloggar](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), [Azure Security Center-aviseringar](../security-center/security-center-intro.md), och [Azure Diagnostics loggar](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) från Azure-resurser. Integrering hjälper till din SIEM-lösning som ger en enhetlig instrumentpanel för alla dina tillgångar, om lokalt eller i molnet. Du kan använda en instrumentpanel för att ta emot, sammanställa, korrelera och analysera aviseringar för säkerhetshändelser.

> [!NOTE]
> Azure Log Integration stöder för närvarande, endast Azure kommersiella och Azure Government-moln. Andra moln stöds inte.

![Diagram över hur Azure Log-integrering][1]

## <a name="what-logs-can-i-integrate"></a>Vilka loggar kan integrera

Azure ger utförlig loggning för varje Azure-tjänsten. Loggarna representerar tre typer av loggen:

* **Kontroll och hantering loggar**: ger insyn i den [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) skapa, uppdatera och ta bort åtgärder. En Azure-aktivitetsloggen är ett exempel på den här typen av loggen.
* **Data plan loggar**: ger inblick i händelser som aktiveras när du använder en Azure-resurs. Ett exempel på den här typen av loggen är Windows Loggboken **System**, **säkerhet**, och **programmet** kanaler i en Windows-dator. Ett annat exempel är Azure-diagnostik loggning som du konfigurerar via Azure-Monitor.
* **Bearbetade händelser**: Ange analyserade händelse och aviseringsinformation som bearbetas åt dig. Ett exempel på den här typen av händelse är Azure Security Center-aviseringar. Azure Security Center bearbetar och analyserar din prenumeration för att tillhandahålla aviseringar som är relevanta för din aktuella säkerhetstillståndet.

Azure Log-Integration stöder ArcSight och QRadar Splunk. Kontrollera med din SIEM-leverantör för att bedöma om leverantören har en intern anslutning. Använd inte Azure Log-integrering om en intern anslutning är tillgänglig.

Om inga andra alternativ är tillgängliga, Överväg att använda Azure Log-integrering. Följande tabell innehåller våra rekommendationer:

|SIEM | Kunden använder redan Azure logganalys integrator | Kunden undersöker integreringsalternativen för SIEM|
|---------|--------------------------|-------------------------------------------|
|**Splunk** | Börja migrera till den [Azure-Monitor-tillägget för Splunk](https://splunkbase.splunk.com/app/3534/). | Använd den [Splunk connector](https://splunkbase.splunk.com/app/3534/). |
|**QRadar** | Migrera till eller börjar använda QRadar-koppling som beskrivs i det sista avsnittet i [dataströmmen Azure övervakningsdata till en händelsehubb för användning av ett externt verktyg](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md). | Använd QRadar anslutningen beskrivs i det sista avsnittet i [dataströmmen Azure övervakningsdata till en händelsehubb för användning av ett externt verktyg](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md). |
|**ArcSight** | Fortsätta att använda Azure logganalys integrator förrän en anslutning är tillgänglig. Migrera sedan till connector-baserad lösning.  | Överväg att använda Azure logganalys som ett alternativ. Inte publicera till Azure Log-integrering om du är beredd att gå igenom migreringsprocessen när anslutningen blir tillgänglig. |

> [!NOTE]
> Även om Azure Log-integrering är en lösning som är ledigt, finns det Azure storage kostnader för lagring av information för loggfiler.

Om du behöver hjälp kan du skapa en [supportbegäran](../azure-supportability/how-to-create-azure-support-request.md). Tjänsten, Välj **loggen Integration**.

## <a name="next-steps"></a>Nästa steg

Den här artikeln ger en introduktion till Azure Log-integrering. Mer information om Azure Log-integrering och vilka typer av loggar som stöds finns i följande artiklar:

* [Kom igång med Azure Log-integrering](security-azure-log-integration-get-started.md). Den här självstudiekursen vägleder dig genom installationen av Azure Log-integrering. Det beskriver också hur du integrerar loggar från Windows Azure Diagnostics (BOMULLSTUSS) lagring, Azure aktivitetsloggar, Azure Security Center-aviseringar och Azure Active Directory-granskningsloggar.
* [Azure Log-integrering vanliga frågor (FAQ)](security-azure-log-integration-faq.md). Det här avsnittet får du svar vanliga frågor om Azure Log-integrering.
* Mer information om hur du [strömma Azure övervakningsdata till en händelsehubb för användning av ett externt verktyg](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md).

<!--Image references-->
[1]: ./media/security-azure-log-integration-overview/azure-log-integration.png
