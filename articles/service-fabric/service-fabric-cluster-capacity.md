---
title: Planera kapacitet för Service Fabric-kluster | Microsoft Docs
description: Service Fabric-kluster kapacitetsplaneringsöverväganden. Nodetypes får, åtgärder, hållbarhet och tillförlitlighet nivåer
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: ''
ms.assetid: 4c584f4a-cb1f-400c-b61f-1f797f11c982
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/04/2018
ms.author: chackdan
ms.openlocfilehash: 848e27078d77a53a03d538b846eed0117d44dbe5
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/16/2018
---
# <a name="service-fabric-cluster-capacity-planning-considerations"></a>Service Fabric-kluster kapacitetsplaneringsöverväganden
För alla Produktionsdistribution är kapacitetsplanering ett viktigt steg. Här är några av de objekt som du måste väga som en del av den här processen.

* Antalet nodtyper klustret måste börja med
* Egenskaperna för varje nodtyp (storlek, primära internetuppkopplad, antalet virtuella datorer, osv.)
* Klustrets egenskaper för tillförlitlighet och hållbarhet

Låt oss gå igenom objekten.

## <a name="the-number-of-node-types-your-cluster-needs-to-start-out-with"></a>Antalet nodtyper klustret måste börja med
Du måste först ta reda på vad du skapar klustret som ska användas för.  Vilka typer av program som du planerar att distribuera till det här klustret? Om du inte avmarkera på syftet med klustret du troligen ännu inte är redo för att ange process för kapacitetsplanering.

Fastställ antalet nodtyper klustret måste börja med.  Varje nodtyp mappas till en skaluppsättning för virtuell dator. Varje nodtyp kan sedan skalas upp eller ned oberoende av de andra, ha olika portar öppna och ha olika kapacitet. Så handlar beslut av antalet nodtyper i stort sett om att följande överväganden:

* Har ditt program flera tjänster och någon av dem måste vara public eller mot internet? Vanliga program innehåller en frontend-gateway-tjänst som tar emot indata från en klient och en eller flera backend-tjänster som kommunicerar med frontend-tjänster. Så i detta fall kan få du med minst två nodtyper.
* Har dina tjänster (som utgör ditt program) annan infrastrukturbehov som större RAM-minne eller högre CPU-cykler? Låt oss anta att det program som du vill distribuera innehåller en frontend-tjänst och en backend tjänst. Frontend-tjänsten kan köras på mindre virtuella datorer (VM-storlekar som D2) som har portar är öppna till internet.  Backend-tjänsten, men beräkning intensiva och behöver köras på större virtuella datorer (med VM-storlekar som D4 D6, D15) som inte är internet riktas.
  
  Även om du kan välja att publicera alla tjänster på en nodtyp i det här exemplet rekommenderar vi att du ska placera dem i ett kluster med två nodtyper.  Därmed kan varje nodtyp har olika egenskaper, till exempel internet-anslutning eller VM-storlek. Antal virtuella datorer kan skalas oberoende av varandra, samt.  
* Eftersom du inte förutsäga framtida med fakta som du känner av och besluta om antalet nodtyper som dina program måste börja med. Du kan alltid lägga till eller ta bort nodtyper senare. Service Fabric-klustret måste ha minst en nodtyp.

## <a name="the-properties-of-each-node-type"></a>Egenskaperna för varje nodtyp
Den **nodtypen** kan ses som motsvarande roller i molntjänster. Nodtyper definiera storlek på Virtuella datorer, hur många virtuella datorer och deras egenskaper. Varje nodtyp som definieras i Service Fabric-klustret har konfigurerats som en separat virtuella datorns skaluppsättning. Skaluppsättning för virtuell dator är en Azure compute-resurs som används för att distribuera och hantera en samling med virtuella datorer som en uppsättning. Varje nodtyp är en distinkta skala ange kan skalas upp eller ned separat, har olika uppsättningar av öppna portar och har olika kapacitetsdata.

Läs [dokumentet](service-fabric-cluster-nodetypes.md) för mer information om relationen mellan nodtyper till virtuella datorer, hur till RDP till någon av instanserna, öppna nya portar osv.

Klustret kan ha fler än en nodtyp, men den primära nodtypen (den första som du definierar i portalen) måste ha minst fem datorer för kluster som används för produktionsarbetsbelastningar (eller minst tre virtuella datorer för testkluster). Om du skapar klustret med en Resource Manager-mall, leta efter **är primära** attribut under noden typdefinition. Den primära nodtypen är nodtypen Service Fabric systemtjänster placering.  

### <a name="primary-node-type"></a>Primära nodtypen
Du måste välja en av dem vara primär för ett kluster med flera nodtyper. Här följer en primära nodtypen egenskaper:

* Den **minsta storlek på virtuella datorer** för den primära noden typen bestäms av den **hållbarhetsnivån** du väljer. Standardvärdet för hållbarhet skiktet är Brons. Rulla nedåt för mer information om hållbarhetsnivån är och värdena kan det ta.  
* Den **minsta antal virtuella datorer** för den primära noden typen bestäms av den **tillförlitlighetsnivån** du väljer. Standardvärdet för tillförlitlighetsnivån är Silver. Rulla nedåt för mer information om tillförlitlighetsnivån är och det kan ta värdena. 


* Service Fabric-systemtjänster (till exempel Klusterhanterare för tjänsten eller Image Store-tjänsten) placeras på den primära nodtypen och så tillförlitlighet och hållbarhet i klustret bestäms av värdet för nivå av tillförlitlighet och hållbarhet nivå värdet som du väljer för den primära nodtypen.

![Skärmbild av ett kluster med två nodtyper ][SystemServices]

### <a name="non-primary-node-type"></a>Icke-primära nodtypen
Det finns en primära nodtypen och resten av dem är icke-primär för ett kluster med flera nodtyper. Här är en icke-primära nodtypen egenskaper:

* Den minimala storleken för virtuella datorer för den här nodtypen bestäms av hållbarhetsnivån som du väljer. Standardvärdet för hållbarhet skiktet är Brons. Rulla nedåt för mer information om hållbarhetsnivån är och värdena kan det ta.  
* Det minsta antalet virtuella datorer för den här nodtypen kan vara en. Dock bör du välja det här talet baserat på antalet repliker av/programtjänsterna som körs i den här nodtypen. Antal virtuella datorer i en nodtyp kan ökas när du har distribuerat klustret.

## <a name="the-durability-characteristics-of-the-cluster"></a>Hållbarhet egenskaper i klustret
Hållbarhetsnivån används för att ange att systemet de privilegier som din virtuella dator har med underliggande Azure-infrastrukturen. I den primära nodtypen kan den här behörigheten Service Fabric att pausa alla VM nivån infrastruktur förfrågningar (till exempel omstart VM, VM avbildningsåterställning eller VM-migrering) som påverkar kvorum kraven för systemtjänsterna och tillståndskänsliga tjänster. I icke-primär nodtyper kan den här behörigheten Service Fabric att pausa alla VM nivån infrastruktur förfrågningar (till exempel omstart VM, VM avbildningsåterställning och VM-migrering) som påverkar kvorum kraven för dina tillståndskänsliga tjänster.

Den här behörigheten uttrycks i följande värden:

* Guld - jobb kan pausas under en period på två timmar per UD infrastrukturen. Guld hållbarhet kan endast aktiveras fullständig nod VM SKU: er som L32s, GS5, G5, DS15_v2, D15_v2. I allmänhet VM-storlekar som anges i http://aka.ms/vmspecs som är markerade som ”instans är isolerad till maskinvara som är dedikerad till en kund” i noteringen, fullständig nod virtuella datorer.
* Silver - infrastruktur-jobb kan pausas för en varaktighet på 10 minuter per UD och är tillgänglig på alla standard virtuella datorer med enkel kärna och senare.
* Brons - inga privilegier. Detta är standardinställningen. Endast använda den här nivån av hållbarhet för nodtyper som kör _endast_ tillståndslösa arbetsbelastningar. 

> [!WARNING]
> Nodetypes får körs med Brons hållbarhet hämta _inga privilegier_. Det innebär att infrastrukturen jobb som påverkar din tillståndslösa arbetsbelastningar inte stoppats eller fördröjd. Det är möjligt att sådan jobb fortfarande kan påverka dina arbetsbelastningar, orsaka driftsavbrott eller andra problem. För alla slags produktion arbetsbelastning körs med minst Silver rekommenderas. Du måste behålla det minsta antalet fem noder för alla nodtyper som har en hållbarhet guld eller Silver. 
> 

Du får välja hållbarhet nivå för var och en av dina nodtyper. Du kan välja en nodtyp att ha en hållbarhet nivå av guld eller silver och den andra har Brons i samma kluster. **Du måste behålla det minsta antalet fem noder för alla nodtyper som har en hållbarhet guld eller silver**. 

**Fördelarna med att använda Silver eller guld hållbarhet nivåer**
 
- Minskar antalet steg i en åtgärd för skalan (det vill säga nodinaktiveringen och ta bort ServiceFabricNodeState anropas automatiskt)
- Minskar risken för dataförluster på grund av en kund-initierad lokalt VM SKU ändring eller de Azure-infrastrukturen åtgärder.
     
**Nackdelarna med att använda Silver eller guld hållbarhet nivåer**
 
- Distributioner så att virtuella datorns skaluppsättning och andra relaterade Azure-resurser) kan vara fördröjd, kan tar för lång tid eller blockeras helt efter problem i klustret eller på infrastrukturnivå. 
- Ökar antalet [replik Livscykelhändelser](service-fabric-reliable-services-lifecycle.md) (till exempel primära växlingar) på grund av automatiserad nod avaktiveringar under Azure-infrastrukturen.
- Tar noder out-of-service för tidsperioder när programuppdateringar för Azure-plattformen eller maskinvara Underhåll aktiviteter som utförs. Du kan se noder med status inaktiverar/inaktiverad under dessa aktiviteter. Detta minskar kapaciteten på klustret tillfälligt, men inte ska påverka tillgängligheten för ditt kluster eller ett program.

### <a name="recommendations-on-when-to-use-silver-or-gold-durability-levels"></a>Rekommendationer för när du ska använda Silver eller guld hållbarhet nivåer

Använda Silver eller guld hållbarhet för alla nodtyper som värd för tillståndskänsliga tjänster som du förväntar dig att skala i (minska VM-instanser) ofta, och du föredrar att distributionsåtgärder förskjutas och kapacitet minskas för att förenkla dessa skala i åtgärder. Skalbar-scenarier (lägga till instanser för virtuella datorer) play inte till valet av hållbarhetsnivån, endast skala-modulen utför.

### <a name="changing-durability-levels"></a>Ändra hållbarhet nivåer
- Nodtyper med hållbarhet Silver eller guld kan inte nedgraderas till Brons.
- Uppgradera från Brons till Silver eller guld kan ta några timmar.
- När du ändrar nivå av hållbarhet, måste du uppdatera den i både Service Fabric-tillägget konfigurationen i din virtuella scale set resurs och i noden typdefinitionen i din Service Fabric-klusterresurs. Dessa värden måste matcha.

### <a name="operational-recommendations-for-the-node-type-that-you-have-set-to-silver-or-gold-durability-level"></a>Operativa rekommendationer för noden ange att du har angett till silver eller guld hållbarhet nivå.

- Skydda klustret och program felfria vid alla tidpunkter och se till att program ska svara alla [tjänsten replik Livscykelhändelser](service-fabric-reliable-services-lifecycle.md) (t.ex. replik i build har fastnat) inom rimlig tid.
- Anta säkrare sätt att göra en ändring (skala upp eller ned) för VM-SKU: ändra VM SKU för skaluppsättning för virtuell dator kombination är en osäker åtgärd och så bör undvikas om möjligt. Här är den process som du kan följa för att undvika vanliga problem.
    - **För icke-primära nodtyper:** bör du skapa nya virtuella datorns skaluppsättning, ändra begränsningen service placering för att inkludera den nya virtuella dator skala set/nodtypen och minska den gamla virtuella scale set instansen Antal 0, en nod i taget (detta är att se till att ta bort noder inte påverkar tillförlitligheten i klustret).
    - **För den primära nodtypen:** vår rekommendation är att du inte ändrar den primära nodtypen VM SKU. Ändring av den primära nodtypen SKU inte stöds. Om orsaken till det nya SKU är kapacitet, rekommenderar vi att lägga till flera instanser. Om den inte kan skapa ett nytt kluster och [Återställ programtillstånd](service-fabric-reliable-services-backup-restore.md) (om tillämpligt) från ditt gamla kluster. Du behöver inte återställa alla service systemtillståndet kan de återskapas när du distribuerar ditt program till det nya klustret. Om du bara tillståndslösa program som körs på klustret och sedan behöver du bara distribuera ditt program till det nya klustret, du har inte något att återställa. Om du vill gå stöds inte vägen och vill ändra VM SKU ange sedan gör ändringar till virtuella datorns skaluppsättning Model definition så att den återger den nya SKU: N. Om klustret har endast en nodtyp, kontrollerar du att alla tillståndskänsliga program svarar på alla [tjänsten replik Livscykelhändelser](service-fabric-reliable-services-lifecycle.md) (t.ex. replik i build har fastnat) i rimlig tid och att tjänsten repliken återskapa varaktighet är mindre än fem minuter (Silver hållbarhet nivå). 

    > [!WARNING]
    > Ändrar storlek på VM-SKU för skalningsuppsättningar i virtuella datorer inte körs på minst Silver hållbarhet inte rekommenderas. Ändra storlek på VM-SKU är skadliga data på plats-infrastruktur. Det är möjligt att det kan leda till dataförlust tillståndskänsliga tjänster eller orsaka andra oförutsedda driftsproblem, även för tillståndslösa arbetsbelastningar utan någon möjlighet att fördröja eller övervaka den här ändringen. 
    > 
    
- Det minsta antalet fem noder för alla skaluppsättning för virtuell dator som har hållbarhet guld eller Silver aktiverad för underhåll
- Inte ta bort slumpmässiga VM-instanser måste alltid använda virtual machine scale set skala ned funktionen. Borttagning av slumpmässiga VM-instanser har en potentiell skapar obalans i VM-instans som är fördelade på UD och FD. Detta att sänka system möjligheten att korrekt belastningsutjämna bland service instanser/Service replikerna.
- Om du använder Autoskala, anger du regler så att skala (att ta bort av VM-instanser) görs endast en nod i taget. Det är inte säkert att skala ned mer än en instans i taget.
- Om skalning av primära nodtypen skala du aldrig den ned över vilka tillförlitlighetsnivån tillåter.

## <a name="the-reliability-characteristics-of-the-cluster"></a>Tillförlitlighetsegenskaper i klustret
Tillförlitlighetsnivån används för att ange antalet repliker av systemtjänster som du vill köra i det här klustret på den primära nodtypen. Det mer antalet repliker mer tillförlitlig systemtjänsterna är i klustret.  

Tillförlitlighetsnivån kan ha följande värden:

* Platina - köra System-tjänster med ett mål replikuppsättningen antal nio
* Guld - köra System-tjänster med ett mål replikuppsättningen antal sju
* Silver - köra System-tjänster med ett mål replikuppsättningen antal fem 
* Brons - köra System-tjänster med ett mål replikuppsättningen antal tre

> [!NOTE]
> Tillförlitlighetsnivån som du anger det minsta antalet noder som den primära nodtypen måste ha. 
> 
> 

### <a name="recommendations-for-the-reliability-tier"></a>Rekommendationer för tillförlitlighetsnivån

När du ökar eller minskar storleken på ditt kluster (summan av VM-instanser i alla nodtyper), måste du uppdatera klustret från ett lager till en annan tillförlitlighet. Detta utlöser klusteruppgradering behövs för att ändra system services repliken ange antal. Vänta medan uppgraderingen pågår slutförts innan du gör några andra ändringar i klustret, som att lägga till noder.  Du kan övervaka förloppet för uppgradering i Service Fabric-Utforskaren eller genom att köra [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)

Här följer rekommendationer om hur du väljer tillförlitlighetsnivån.

| **Klusterstorleken** | **Tillförlitlighetsnivån** |
| --- | --- |
| 1 |Ange inte parametern tillförlitlighetsnivån beräknar systemet |
| 3 |Brons |
| 5 eller 6|Silver |
| 7 eller 8 |Guld |
| 9 och senare |Platina |

## <a name="primary-node-type---capacity-guidance"></a>Primära nodtypen - kapacitet vägledning

Här följer riktlinjer för planering av kapacitet för primära noden typ:

- **Antal VM-instanser för att köra alla produktion arbetsbelastningar i Azure:** måste du ange en primär nod typen minimistorleken på 5. 
- **Antal VM-instanser för att köra arbetsbelastningar för testning i Azure** du kan ange en minsta primära noden typen storlek på 1 eller 3. En nod-kluster med en specialkonfiguration och kan skala utanför klustret stöds inte. En nod-klustret har inga tillförlitlighet och så i Resource Manager-mall måste du ta bort/inte ange att konfigurationen (inte inställningen Konfigurationsvärdet räcker inte). Om du konfigurerar en nod klustret ställa in via portalen är automatiskt konfigurationen tagit hand om. Kluster med en och tre noder stöds inte för att köra produktionsarbetsbelastningar. 
- **VM-SKU:** primära nodtypen är där systemtjänsterna körs, så VM SKU du väljer det, måste ta hänsyn övergripande belastning Utjämning av du planerar att placera i klustret. Här är en liknelsen att illustrera vad jag betyder här - tänka på den primära nodtypen som din ”lungor”, är det den erbjuder kontroll till din hjärna och därför om hjärna inte får tillräckligt med kontroll din brödtext drabbas av. 

Eftersom kapacitetsbehov i ett kluster bestäms av arbetsbelastning som du planerar att köra i klustret, kan vi inte ge dig med kvalitativa vägledning för din specifika arbetsbelastning, men det här är den allmänna riktlinjer som hjälper dig att komma igång

För produktionsarbetsbelastningar: 

- Den rekommenderade VM-SKU är Standard D3 Standard D3_V2 eller motsvarande med minst 14 GB lokal SSD.
- Minsta stöds Använd VM SKU är Standard D1 Standard D1_V2 eller motsvarande med minst 14 GB lokal SSD. 
- Partiell core VM SKU: er som Standard A0 stöds inte för produktionsarbetsbelastningar.
- A1 SKU: N stöds inte för produktionsarbetsbelastningar av prestandaskäl.

> [!WARNING]
> För närvarande kan stöds ändra den primära noden VM SKU-storleken på ett kluster som körs inte. Så väljer du den primära nodtypen VM SKU noggrant, med hänsyn till din framtida kapacitetsbehov. Just nu är det enda sättet att flytta den primära nodtypen till en ny VM-SKU (mindre eller större) är att skapa ett nytt kluster med rätt kapacitet distribuera dina program och sedan återställa programtillståndet (om tillämpligt) från den [ senaste tjänsten säkerhetskopieringar](service-fabric-reliable-services-backup-restore.md) du har vidtagit från det gamla klustret. Du behöver inte återställa alla service systemtillståndet kan de återskapas när du distribuerar program till det nya klustret. Om du bara tillståndslösa program som körs på klustret och sedan behöver du bara distribuera ditt program till det nya klustret, du har inte något att återställa.
> 

## <a name="non-primary-node-type---capacity-guidance-for-stateful-workloads"></a>Icke-primära nodtypen - kapacitet vägledning för tillståndskänsliga arbetsbelastningar

Den här vägledningen är för tillståndskänsliga arbetsbelastningar med hjälp av Service fabric [tillförlitliga samlingar eller reliable Actors](service-fabric-choose-framework.md) som körs i icke-primära nodtypen.

**Antal VM-instanser:** för produktionsarbetsbelastningar som är tillståndskänslig rekommenderas att köra med det lägsta och mål replikantalet 5. Det innebär att i stabilt tillstånd du att få en replik (från en replikuppsättning) i varje feldomän och uppgraderingsdomänen. Hela tillförlitlighet nivå konceptet för den primära nodtypen är ett sätt att ange den här inställningen för systemtjänster. Så gäller samma betalning dina tillståndskänsliga tjänster.

För produktionsarbetsbelastningar är teckenstorlek minsta rekommenderade icke - primära noden därför 5, om du kör tillståndskänsliga arbetsbelastningar i den.

**VM-SKU:** det här är nodtypen där ditt program körs, så VM SKU du väljer för det, måste ta hänsyn till belastning som du planerar att placera i varje nod. Kapacitetsbehov för nodetype, bestäms av arbetsbelastning som du planerar att köra i klustret, så vi inte kan tillhandahålla dig med kvalitativa vägledning för din specifika arbetsbelastning, men det här är den allmänna riktlinjer som hjälper dig att komma igång

För produktionsarbetsbelastningar 

- Den rekommenderade VM-SKU är Standard D3 Standard D3_V2 eller motsvarande med minst 14 GB lokal SSD.
- Minsta stöds Använd VM SKU är Standard D1 Standard D1_V2 eller motsvarande med minst 14 GB lokal SSD. 
- Partiell core VM SKU: er som Standard A0 stöds inte för produktionsarbetsbelastningar.
- A1 SKU: N stöds inte specifikt för produktionsarbetsbelastningar av prestandaskäl.

## <a name="non-primary-node-type---capacity-guidance-for-stateless-workloads"></a>Icke-primära nodtypen - kapacitet vägledning för tillståndslösa arbetsbelastningar

Den här vägledningen tillståndslösa arbetsbelastningar som körs på icke-primär nodetype.

**Antal VM-instanser:** för produktionsarbetsbelastningar som är tillståndslös minsta stöds icke - primära noden standardstorleken är 2. På så sätt kan du köra två tillståndslös instanser av programmet och låta din tjänst att överleva förlust av en VM-instans. 

**VM-SKU:** det här är nodtypen där ditt program körs, så VM SKU du väljer för det, måste ta hänsyn till belastning som du planerar att placera i varje nod. Kapacitetsbehov för nodetype, bestäms av arbetsbelastning som du planerar att köra i klustret, så vi inte kan tillhandahålla dig med kvalitativa vägledning för din specifika arbetsbelastning, men det här är den allmänna riktlinjer som hjälper dig att komma igång

För produktionsarbetsbelastningar 

- Den rekommenderade VM-SKU är Standard D3 Standard D3_V2 eller motsvarande. 
- Minsta stöds Använd VM SKU är Standard D1 Standard D1_V2 eller motsvarande. 
- Partiell core VM SKU: er som Standard A0 stöds inte för produktionsarbetsbelastningar.
- A1 SKU: N stöds inte för produktionsarbetsbelastningar av prestandaskäl.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>Nästa steg
När du slutför din kapacitetsplanering och konfigurera ett kluster kan du läsa följande:

* [Säkerhet för Service Fabric-kluster](service-fabric-cluster-security.md)
* [Skalning av Service Fabric-klustret](service-fabric-cluster-scaling.md)
* [Planering för katastrofåterställning](service-fabric-disaster-recovery.md)
* [Ange relationen mellan nodetypes får och virtuella datorn](service-fabric-cluster-nodetypes.md)

<!--Image references-->
[SystemServices]: ./media/service-fabric-cluster-capacity/SystemServices.png
