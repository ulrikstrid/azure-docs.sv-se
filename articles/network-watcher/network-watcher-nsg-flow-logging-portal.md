---
title: Hantera nätverket grupp flödet säkerhetsloggar med Azure Nätverksbevakaren | Microsoft Docs
description: Den här sidan förklarar hur du hanterar Nätverkssäkerhetsgruppen flöde loggar i Azure Nätverksbevakaren
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: 01606cbf-d70b-40ad-bc1d-f03bb642e0af
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: cb41781c5ac8fb759cecea01402c08dd716bf7d7
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/03/2018
---
# <a name="manage-network-security-group-flow-logs-in-the-azure-portal"></a>Hantera nätverket grupp flödet säkerhetsloggar i Azure-portalen

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-nsg-flow-logging-portal.md)
> - [PowerShell](network-watcher-nsg-flow-logging-powershell.md)
> - [CLI 1.0](network-watcher-nsg-flow-logging-cli-nodejs.md)
> - [CLI 2.0](network-watcher-nsg-flow-logging-cli.md)
> - [REST API](network-watcher-nsg-flow-logging-rest.md)

Nätverket säkerhetsloggar grupp flöde är en funktion i Nätverksbevakaren där du kan visa information om ingående och utgående IP-trafik via en nätverkssäkerhetsgrupp. Loggarna flödet skrivs i JSON-format och innehåller viktig information, inklusive: 

- Utgående och inkommande flöden på grundval av per regel.
- Nätverkskortet som flödet gäller för.
- 5-tuppel information om flödet (källan/målet IP, källan/målet port och protokoll).
- Information om huruvida trafik tillåtas eller nekas.

## <a name="before-you-begin"></a>Innan du börjar

Du måste redan ha följande resurser för att slutföra stegen i den här artikeln:

- En befintlig Nätverksbevakaren. Om du vill skapa en Nätverksbevakaren finns [skapa en instans av Nätverksbevakaren](network-watcher-create.md).
- En befintlig resursgrupp med en giltig virtuell dator. Om du inte har en virtuell dator, se Skapa en [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) eller [Windows](../virtual-machines/windows/quick-create-portal.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) virtuella datorn.

## <a name="register-insights-provider"></a>Registrera providern insikter

För flöde loggning för att fungera korrekt, den **Microsoft.Insights** provider måste vara registrerad. Gör följande om du vill registrera providern: 

1. Gå till **prenumerationer**, och sedan väljer du den prenumeration som du vill aktivera flödet loggar. 
2. På den **prenumeration** bladet väljer **Resursproviders**. 
3. Titta på listan med providers och kontrollera att den **microsoft.insights** providern är registrerad. Om inte, välj sedan **registrera**.

![Visa providers][providers]

## <a name="enable-flow-logs"></a>Aktivera flödet loggar

Dessa steg leder dig genom processen för att aktivera flödet loggar på en nätverkssäkerhetsgrupp.

### <a name="step-1"></a>Steg 1

Gå till en Nätverksbevakaren-instans och välj sedan **NSG flöda loggar**.

![Översikt över flödet-loggar][1]

### <a name="step-2"></a>Steg 2

Välj en säkerhetsgrupp för nätverk i listan.

![Översikt över flödet-loggar][2]

### <a name="step-3"></a>Steg 3 

På den **flöde loggar inställningar** bladet Ange status till **på**, och konfigurera ett lagringskonto. Välj ett befintligt lagringskonto som har **alla nätverk** (standard) som är markerad under **brandväggar och virtuella nätverk**under den **inställningar** för lagringskontot. När du har markerat ett lagringskonto, Välj **OK**, och välj sedan **spara**.

![Översikt över flödet-loggar][3]

## <a name="download-flow-logs"></a>Hämta flödet loggar

Flödet loggar sparas i ett lagringskonto. Hämta loggarna om du vill visa dem flödet.

### <a name="step-1"></a>Steg 1

Om du vill hämta loggar flödet, Välj **kan du hämta flödet loggar från konfigurerade lagringskonton**. Det här steget kommer du till vyn storage-konto där du kan välja vilka loggarna om du vill hämta.

![Flödesloggsinställningar][4]

### <a name="step-2"></a>Steg 2

Gå till rätt storage-konto. Välj sedan **behållare** > **insights-log-networksecuritygroupflowevent**.

![Flödesloggsinställningar][5]

### <a name="step-3"></a>Steg 3

Gå till platsen för loggen flödet, markerar du den och välj sedan **hämta**.

![Flödesloggsinställningar][6]

Information om strukturen i loggen finns [nätverk grupp flödet loggen Säkerhetsöversikt](network-watcher-nsg-flow-logging-overview.md).

## <a name="next-steps"></a>Nästa steg

Lär dig hur du [visualisera dina NSG flödet loggar med PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md).

<!-- Image references -->
[1]: ./media/network-watcher-nsg-flow-logging-portal/figure1.png
[2]: ./media/network-watcher-nsg-flow-logging-portal/figure2.png
[3]: ./media/network-watcher-nsg-flow-logging-portal/figure3.png
[4]: ./media/network-watcher-nsg-flow-logging-portal/figure4.png
[5]: ./media/network-watcher-nsg-flow-logging-portal/figure5.png
[6]: ./media/network-watcher-nsg-flow-logging-portal/figure6.png
[providers]: ./media/network-watcher-nsg-flow-logging-portal/providers.png
