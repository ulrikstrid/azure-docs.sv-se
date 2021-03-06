---
title: Allmänna datacenter integration överväganden för Azure-stacken integrerat system | Microsoft Docs
description: Lär dig vad du kan göra för att planera nu och förbereder för integrering av datacenter med flera noder Azure Stack.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: 55243ead4f088f7a2b3d54c0581c604f0dc63d07
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/03/2018
---
# <a name="datacenter-integration-considerations-for-azure-stack-integrated-systems"></a>Datacenter-integrering överväganden för Azure-stacken integrerat system
Om du är intresserad av en Azure-stacken integrerat system, bör du förstå några viktiga överväganden kring distribution och hur systemet passar in i ditt datacenter. Den här artikeln innehåller en översikt över dessa överväganden som hjälper dig att fatta viktiga infrastruktur beslut för ditt system med flera noder Azure stacken. Förstå dessa överväganden hjälper när du arbetar med maskinvaruleverantören OEM när de distribuerar Azure Stack till ditt datacenter.  

> [!NOTE]
> Azure-stacken system med flera noder kan endast köpas från auktoriserade maskinvaruleverantörer. 

Du måste förse din leverantör planeringsinformation innan distribution startas för att gå smidigt och snabbt processen för att distribuera Azure-stacken. Adressintervall upplysningar över nätverk, säkerhet och identitetsinformation med många viktiga beslut som kan kräva att information från många olika områden och beslutsfattare. Därför kanske du måste dra in personer från flera team i din organisation så att du har all nödvändig information som är klar innan du påbörjar distributionen. Det hjälper dig för att tala med din maskinvaruleverantör vid insamling av den här informationen som de kan ha råd bra att fattar ditt beslut.

Du kan behöva göra vissa före distributionen konfigurationsändringar i din nätverksmiljö när du undersöker och samla in nödvändig information. Det kan vara att reservera IP-adressutrymmen för Azure-stacken lösningen, hur du konfigurerar dina routrar, växlar och brandväggar för att förbereda för anslutningen till de nya Azure-stacken lösning växlarna. Se till att ha ämne området experten sida upp till hjälpa dig med planeringen.

## <a name="capacity-planning-considerations"></a>Kapacitetsplaneringsöverväganden
När du utvärderar en Azure-stacken lösning för göras maskinvara konfigurationsalternativ som har en direkt inverkan på den totala kapaciteten i sin Azure Stack-lösning. Dessa inkluderar klassiska val av CPU, minne densitet, lagringskonfiguration och övergripande lösning skala (t.ex. antalet servrar). Till skillnad från en traditionell virtualiseringslösning gäller inte enkla aritmetiska för dessa komponenter för att avgöra kapaciteten som kan användas. Det första skälet är att Azure-stacken är konstruerad för att vara värd för infrastruktur eller hantering av komponenterna i själva lösningen. Den andra orsaken är att några av lösningens kapacitet är reserverad för att stödja återhämtning; uppdatering av lösningens programvara på ett sätt som minimerar avbrott i klienternas arbetsbelastningar. 

Den [Azure Stack kapacitet planner kalkylblad](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) sätt att göra informerat beslut om att planera kapaciteten på två sätt: antingen den genom att välja ett erbjudande för maskinvara och försök att passa en kombination av resurser eller genom att definiera den arbetsbelastning som Azure-stacken är avsedd att köras för att visa tillgängliga SKU: er som stöds av maskinvaran. Slutligen är kalkylbladet avsedd som en guide för att fatta beslut som rör Azure Stack planering och konfiguration. 

Kalkylbladet är inte avsedd att fungera som en ersättning för egna undersökningar och analys.  Microsoft lämnar inga garantier, uttryckliga eller underförstådda, avseende informationen i kalkylbladet.



## <a name="management-considerations"></a>Hanteringsanmärkningar
Azure-stacken är ett förseglat system där infrastrukturen som är låst både från en behörigheter och perspektiv. Nätverket åtkomstkontrollistor (ACL) används för att blockera all inkommande trafik som obehörig och all onödig kommunikation mellan infrastrukturkomponenter. Detta gör det svårare för obehöriga användare att komma åt systemet.

För dagliga hantering och åtgärder finns det ingen obegränsad administratörsåtkomst till infrastrukturen. Azure Stack-operatorer måste hantera systemet via administratörsportalen eller via Azure Resource Manager (via PowerShell eller REST API). Det finns ingen åtkomst till datorn av andra hanteringsverktyg, till exempel Hyper-V Manager eller hanteraren för redundanskluster. Program från andra tillverkare (till exempel agenter) kan inte installeras i komponenterna i Azure Stack-infrastruktur för att skydda datorn. Samverkan med externa hanterings- och säkerhetsprogramvara görs via PowerShell eller REST API.

När en högre nivå av åtkomst krävs för att felsöka problem som inte är löst aviseringen medling steg måste du arbeta med Microsoft-supporten. Det finns en metod för att tillhandahålla tillfällig fullständig administratörsåtkomst till systemet för att utföra mer avancerade åtgärder via support. 

## <a name="identity-considerations"></a>Identity-överväganden

### <a name="choose-identity-provider"></a>Välj identitetsleverantören.
Du behöver överväga vilka identitetsleverantör som du vill använda för distribution av Azure-stacken, Azure AD eller AD FS. Du kan inte växla identitetsleverantörer efter distributionen utan Omdistributionen för hela systemet. Om du inte äger Azure AD-kontot och använder ett konto som du fått av din Molntjänstleverantör, och om du vill växla providern och använder en annan Azure AD-kontot, då du måste kontakta din leverantör om du vill distribuera om lösningen f eller du dina kostnader.



Önskat identity-providern påverkar inte på virtuella datorer, identitet och konton som de använder, om de kan ansluta till en Active Directory-domän, osv. Detta är separat.

Du kan lära dig mer om hur du väljer en identitetsleverantör i den [Azure Stack integrerade system anslutning modeller artikel](.\azure-stack-connection-models.md).

### <a name="ad-fs-and-graph-integration"></a>Integrering med AD FS och diagram
Om du väljer att distribuera Azure-stacken använder AD FS som identitetsleverantören måste du integrera instansen av AD FS på Azure-stacken med en befintlig instans av AD FS via ett federationsförtroende. Detta gör att identiteter i en befintlig Active Directory-skog för att autentisera med resurser i Azure-stacken.

Du kan också integrera Graph-tjänsten i Azure-stacken med befintliga Active Directory. På så sätt kan du hantera rollbaserad åtkomstkontroll (RBAC) i Azure-stacken. När åtkomst till en resurs har delegerats söker komponenten diagrammet användarkonto i den befintliga Active Directory-skogen med LDAP-protokollet.

Följande diagram visar integrerad trafikflöde för AD FS och diagram.
![Diagram över AD FS och diagram trafikflöde](media/azure-stack-datacenter-integration/ADFSIntegration.PNG)

## <a name="licensing-model"></a>Licensieringsmodell
Du måste bestämma vilka licensieringsmodell som du vill använda. De tillgängliga alternativen beror på om du distribuerar Azure Stack ansluten till internet:
- För en [anslutna distribution](azure-stack-connected-deployment.md), kan du betala-som-du-användning eller kapacitet-baserade licensiering. Betala per-som-du-användningen kräver en anslutning till Azure för att rapportera användningen, som sedan debiteras via Azure handel. 
- Endast kapacitet-baserade licensiering stöds om du [distribuera frånkopplad](azure-stack-disconnected-deployment.md) från internet. 

Mer information om licensiering modeller finns [Microsoft Azure-stacken paketera och prissättning](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf).


## <a name="naming-decisions"></a>Namnge beslut

Du behöver tänka på hur du vill planera Azure Stack namnområdet, särskilt regionnamn och extern domän. Externa fullständigt kvalificerade domännamnet (FQDN) för Azure-stacken distributionen för offentliga slutpunkter är en kombination av dessa två namn: &lt; *region*&gt;.&lt; *fqdn*&gt;. Till exempel *east.cloud.fabrikam.com*. I det här exemplet skulle Azure Stack-portaler vara tillgängligt vid följande webbadresser:

- https://portal.east.cloud.fabrikam.com
- https://adminportal.east.cloud.fabrikam.com

> [!IMPORTANT]
> Regionnamn du väljer för distributionen av Azure-stacken måste vara unika och visas i portalen adresser. 

I följande tabell sammanfattas besluten domän namngivning.

| Namn | Beskrivning | 
| -------- | ------------- | 
|Regionnamn | Namnet på din första Azure Stack-region. Det här namnet används som en del av det fullständiga Domännamnet för de offentliga virtuella IP-adresser (VIP) som hanterar Azure-stacken. Normalt är regionnamn en identifierare för fysisk plats, till exempel en datacenter-plats. | 
| Externa domännamn | Namnet på zonen Domain Name System (DNS) för slutpunkter med externa virtuella IP-adresser. Används i det fullständiga Domännamnet för dessa offentliga VIP. | 
| Domännamn för privat (internt) | Namnet på domänen (och interna DNS-zon) skapas på Azure-Stack för infrastrukturhantering. 
| | |

## <a name="certificate-requirements"></a>Certifikatkrav

För distribution måste du tillhandahålla Secure Sockets Layer (SSL)-certifikat för offentliga slutpunkter. På en hög nivå har certifikat följande krav:

- Du kan använda en enda jokerteckencertifikat eller du kan använda en uppsättning dedikerade certifikat och använder jokertecken endast för slutpunkter som lagring och Nyckelvalvet.
- Certifikat kan utfärdas av en offentlig betrodda certifikatutfärdare (CA) eller en Certifikatutfärdare som hanteras av kunden.

Mer information om vilka PKI certifikat krävs för att distribuera Azure-stacken och hur du hämtar dem finns, [Azure Stack infrastruktur för offentliga nycklar certifikatkrav](azure-stack-pki-certs.md).  


> [!IMPORTANT]
> Informationen för PKI-certifikat ska användas som en allmän vägledning. Innan du skaffar PKI-certifikat för Azure-Stack fungera med partnern OEM-maskinvara. De ger mer detaljerad vägledning för certifikat och krav.


## <a name="time-synchronization"></a>Tidssynkronisering
Du måste välja en viss tid server med används för att synkronisera Azure stacken.  Tid symbolization är viktigt att Azure-stacken och sin infrastruktur-roller som används för att generera en Kerberos-biljetter som används för att autentisera interna tjänster med varandra.

Du måste ange en IP-adress för synkroniseringsserver tid även om de flesta av komponenter i infrastrukturen kan lösa en URL vissa stöder bara IP-adresser. Om du har med distributionsalternativet frånkopplade, måste du ange en tidsserver på företagets nätverk som du är säker kan nås från nätverkets infrastruktur i Azure-stacken.

## <a name="connect-azure-stack-to-azure"></a>Ansluta Azure Stack till Azure

Scenarier med hybrid cloud behöver du planera hur du vill ansluta Azure Stack till Azure. Det finns två sätt att ansluta virtuella nätverk i Azure-stacken till virtuella nätverk i Azure: 
- **Plats-till-plats**. Ett virtuellt privat nätverk (VPN)-anslutning via IPsec (IKE v1 och v2 för IKE). Den här typen av anslutning kräver en VPN-enhet eller Routning och fjärråtkomst (RRAS). Mer information om VPN-gatewayer i Azure finns [om VPN-Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). Kommunikation via den här tunnel är krypterad och är säker. Dock begränsas bandbredd av maximalt dataflöde för tunneln (100 – 200 Mbit/s).
- **Utgående NAT**. Som standard kommer alla virtuella datorer i Azure-stacken är ansluten till externa nätverk via utgående NAT. Varje virtuellt nätverk som skapats i Azure-stacken hämtar en offentlig IP-adress som tilldelats. Om den virtuella datorn direkt har tilldelats en offentlig IP-adress, eller bakom en belastningsutjämnare med en offentlig IP-adress, har utgående åtkomst via utgående NAT med VIP för det virtuella nätverket. Detta fungerar endast för kommunikation som initieras av den virtuella datorn och är avsedd för externa nätverk (internet eller intranätet). Den kan inte användas för att kommunicera med den virtuella datorn från utanför.

### <a name="hybrid-connectivity-options"></a>Hybrid anslutningsalternativ

Hybridanslutning är det viktigt att tänka på vilken typ av distribution du vill erbjuda och där det ska distribueras. Du behöver avgöra om du behöver isolera nätverkstrafiken per klient, och om du har ett intranät eller internet-distribution.

- **Stöd för en innehavare Azure Stack**. Ett Azure Stack-distribution som ser ut, minst ur nätverk som om det är en klient. Det kan finnas många klient prenumerationer, men som alla intranät-tjänster all trafik färdas i samma nätverk. Nätverkstrafik från en prenumeration färdas över samma nätverksanslutning som en annan prenumeration och behöver inte isoleras via en krypterad tunnel.

- **Flera innehavare Azure Stack**. En Azure-stacken distribution där varje innehavare prenumeration trafik som är bunden till nätverk som är externa för Azure-stacken måste isoleras från andra klienter nätverkstrafik.
 
- **Intranät distribution**. En Azure-Stack-distribution som placeras på en företags-intranät, vanligtvis på privata IP-adressutrymme och bakom en eller flera brandväggar. De offentliga IP-adresserna är inte helt offentliga som de inte kan dirigeras direkt via det offentliga internet.

- **Distribution av Internet**. Ett Azure Stack-distribution som är anslutet till offentligt internet och använder internet-dirigerbara offentliga IP-adresser för det offentliga VIP-adressintervallet. Distributionen kan fortfarande hamnar bakom en brandvägg, men det offentliga VIP-intervallet kan nås direkt från offentliga internet och Azure.
 
I följande tabell sammanfattas hybrid anslutningen scenarier med experter, nackdelar och användningsfall.

| Scenario | Anslutningsmetod | Tekniker | Nackdelar | Bra för |
| -- | -- | --| -- | --|
| Enkel klient Azure-stacken, intranät-distribution | Utgående NAT | Bättre bandbredd för överföring av snabbare. Enkel att implementera; Inga gateways som krävs. | Trafik som inte är krypterade; Ingen isolering eller kryptering utöver TOR. | Enterprise-distributioner där alla klienter är lika betrodda.<br><br>Företag som har en Azure ExpressRoute-krets till Azure. |
| Flera innehavare Azure stacken, intranät-distribution | Plats-till-plats VPN | Trafiken från innehavaren VNet till mål är säker. | Bandbredd begränsas av plats-till-plats VPN-tunnel.<br><br>Kräver en gateway i det virtuella nätverket och en VPN-enhet i målnätverket. | Enterprise-distributioner där vissa klienttrafik måste skyddas från andra klienter. |
| Enkel klient Azure-stacken, internet-distribution | Utgående NAT | Bättre bandbredd för överföring av snabbare. | Trafik som inte är krypterade; Ingen isolering eller kryptering utöver TOR. | Värdar för scenarier där klienten hämtar sina egna Azure Stack-distribution och en särskild krets Azure Stack-miljön. Till exempel ExpressRoute och Multiprotocol etikett växlar (MPLS).
| Flera innehavare Azure stacken, internet-distribution | Plats-till-plats VPN | Trafiken från innehavaren VNet till mål är säker. | Bandbredd begränsas av plats-till-plats VPN-tunnel.<br><br>Kräver en gateway i det virtuella nätverket och en VPN-enhet i målnätverket. | Värd för scenarier där providern vill erbjuda ett moln med flera innehavare, där klienterna inte förtroende för varandra och trafik måste vara krypterad.
|  |  |  |  |  |

### <a name="using-expressroute"></a>Med hjälp av ExpressRoute

Du kan ansluta Azure Stack till Azure via [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) för både en klient intranät- och flera innehavare scenarier. Du behöver en etablerade ExpressRoute-krets via [en anslutning provider](https://docs.microsoft.com/azure/expressroute/expressroute-locations).

Följande diagram visar ExpressRoute för en enskild klient-scenario (där ”kundens anslutning” är ExpressRoute-kretsen).

![Diagram som visar en klient ExpressRoute scenario](media/azure-stack-datacenter-integration/ExpressRouteSingleTenant.PNG)

Följande diagram visar ExpressRoute för ett scenario med flera innehavare.

![Diagram som visar flera innehavare ExpressRoute scenario](media/azure-stack-datacenter-integration/ExpressRouteMultiTenant.PNG)

## <a name="external-monitoring"></a>Externa övervakning
Hämta en enda vy av alla aviseringar från dina Azure-stacken distribution och enheter och integrera aviseringar i befintliga IT service management-arbetsflöden för biljetter, kan du [integrera Azure stacken med externa datacenter övervakningslösningar](azure-stack-integrate-monitor.md).

Maskinvara livscykel värden kan ingår i Azure Stack-lösningen, en dator utanför Azure-stacken som kör OEM-tillverkarens hanteringsverktyg för maskinvara. Du kan använda dessa verktyg eller andra lösningar som kan integreras direkt med befintliga lösningar övervakning i ditt datacenter.

I följande tabell sammanfattas i listan över tillgängliga alternativ.

| Område | Externa övervakningslösning |
| -- | -- |
| Azure Stack-programvara | [Azure-stacken hanteringspaket för Operations Manager](https://azure.microsoft.com/blog/management-pack-for-microsoft-azure-stack-now-available/)<br>[Nagios plugin-program](https://exchange.nagios.org/directory/Plugins/Cloud/Monitoring-AzureStack-Alerts/details)<br>REST-baserad API-anrop | 
| Fysiska servrar (bmc via IPMI) | OEM-maskinvara – hanteringspaket för Operations Manager-leverantör<br>OEM-tillverkarens maskinvarulösning<br>Maskinvaruleverantören Nagios plugin-program | OEM-partner stöds övervakningslösning (ingår) | 
| Nätverksenheter (SNMP) | Operations Manager identifiering av nätverksenheter<br>OEM-tillverkarens maskinvarulösning<br>Nagios växla plugin-program |
| Hälsoövervakning för klienten prenumeration | [System Center Management Pack för Windows Azure](https://www.microsoft.com/download/details.aspx?id=50013) | 
|  |  | 

Observera följande krav:
- Lösningen som du använder måste vara utan Agent. Du kan inte installera agenter från tredje part i Azure Stack-komponenter. 
- Om du vill använda System Center Operations Manager krävs Operations Manager 2012 R2 eller Operations Manager 2016.

## <a name="backup-and-disaster-recovery"></a>Säkerhetskopiering och katastrofåterställning

Planera för säkerhetskopiering och katastrofåterställning omfattar planering för både den underliggande Azure Stack infrastruktur som är värd för virtuella datorer för IaaS och PaaS-tjänster och för klient-program och data. Du måste planera för dessa separat.

### <a name="protect-infrastructure-components"></a>Skydda infrastrukturkomponenter

Du kan [säkerhetskopiera Azure Stack](azure-stack-backup-back-up-azure-stack.md) infrastrukturkomponenter på en SMB-resursen som du anger:

- Du behöver en extern SMB-filresurs på en befintlig Windows-baserad filserver eller en enhet från tredje part.
- Du bör använda samma resursen för säkerhetskopiering av nätverksväxlar och maskinvara livscykel värden. Maskinvaruleverantören OEM hjälper dig att ge vägledning för säkerhetskopiering och återställning av dessa komponenter som dessa är externa för Azure-stacken. Du är ansvarig för att köra säkerhetskopiering arbetsflöden baserat på en rekommendation för OEM-tillverkare.

Om oåterkallelig dataförlust uppstår, kan du använda infrastruktur för att säkerhetskopiera till Anger startvärde distribution data, till exempel distribution indata och identifierare, tjänstkonton Certifikatutfärdarens rotcertifikat, externa resurser (i frånkopplade distributioner), planer, erbjudanden prenumerationer, kvoter, RBAC principen och rollen tilldelningar och hemligheter i Nyckelvalvet.
 
### <a name="protect-tenant-applications-on-iaas-virtual-machines"></a>Skydda program för klienter på IaaS-virtuella datorer

Azure-stacken säkerhetskopierar inte upp klient program och data. Du måste planera för säkerhetskopiering och disaster recovery-skydd till ett mål som är externa för Azure-stacken. Klient-skydd är en klient-driven aktivitet. För IaaS-virtuella datorer använda klienter-gäst tekniker för att skydda mappar, programdata och systemtillstånd. Som en enterprise- eller service provider, kan du dock vill erbjuda en säkerhetskopierings- och återställningslösning i samma datacenter eller externt i ett moln.

Om du vill säkerhetskopiera Linux eller Windows IaaS virtuella datorer måste du använda säkerhetskopiering produkter med åtkomst till gästoperativsystemet för att skydda fil, mapp, operativsystemtillstånd och programdata. Du kan använda Azure-säkerhetskopiering, System Center Data Center Protection Manager eller produkter från tredje part som stöds.

För att replikera data till en sekundär plats och dirigera programmet redundans om en olycka inträffar, kan du använda Azure Site Recovery eller produkter från tredje part. (Vid den första versionen av integrerade system Azure Site Recovery inte stöd för återställning efter fel. Men kan du uppnå återställning via en manuell process.) Program som stöder intern replikering (till exempel Microsoft SQL Server) kan också replikera data till en annan plats där programmet körs.

> [!IMPORTANT]
> Vid den första versionen av integrerade system ska vi stöder skydd tekniker som arbetar på gästnivå för en virtuell IaaS-dator. Du kan inte installera agenter på underliggande infrastruktur-servrar.

## <a name="learn-more"></a>Läs mer

- Information om användningsområden, köpa, partners och OEM maskinvaruleverantörer finns i [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) produktsidan.
- Information om plan och geo tillgänglighet för Azure-stacken integrerade system finns i faktabladet: [Azure stacken: ett tillägg för Azure](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/). 

## <a name="next-steps"></a>Nästa steg
[Azure Stack anslutning distributionsmodeller](azure-stack-connection-models.md)