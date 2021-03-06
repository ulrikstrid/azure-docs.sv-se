---
title: Kontrollera Azure CDN cachelagring av frågesträngar med cachelagring regler | Microsoft Docs
description: Du kan använda CDN cachelagring regler för att ange eller ändra standardbeteendet cache giltighetstid både globalt och med villkor, till exempel en URL-sökväg och filnamn tillägg.
services: cdn
documentationcenter: ''
author: dksimpson
manager: akucer
editor: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2018
ms.author: v-deasim
ms.openlocfilehash: 18704f2d2a553d7fafb16575ce81128ab47bf09c
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2018
---
# <a name="control-azure-cdn-caching-behavior-with-caching-rules"></a>Kontrollen Azure CDN cachelagring av frågesträngar med cachelagring regler

> [!NOTE] 
> Regler för cachelagring är bara tillgängliga för **Azure CDN Standard från Verizon** och **Azure CDN Standard från Akamai**. För **Azure CDN Premium från Verizon**, du kan använda den [Azure CDN regelmotor](cdn-rules-engine.md) i den **hantera** portalen för liknande funktionalitet.
 
Azure Content Delivery Network (CDN) finns två sätt att styra hur dina filer cachelagras: 

- Cachelagring regler: den här artikeln beskriver hur du kan använda innehållsleveransnätverk (CDN) cachelagring regler för att ange eller ändra standardbeteendet cache giltighetstid både globalt och med anpassade villkor, till exempel ett URL-sökväg och filnamn tillägg. Azure CDN finns två typer av cachelagring regler:

   - Global cachelagring regler: du kan ange en regel för globala cachelagring för varje slutpunkt i din profil, vilket påverkar alla förfrågningar till slutpunkten. Den globala cacheregeln åsidosätter eventuella cache-direktiv HTTP-huvuden om ange.

   - Anpassade regler för cachelagring: du kan ange en eller flera anpassade cachelagringsregler för varje slutpunkt i din profil. Ange anpassade cachelagring regler matchar specifika sökvägar filnamnstillägg, bearbetas i ordning och åsidosätta den globala cacheregeln om. 

- Cachelagring av frågesträngar i frågan: du kan ändra hur Azure CDN behandlar cachelagring för begäranden med frågesträngar. Mer information finns i [kontroll Azure CDN cachelagring av frågesträngar med frågesträngar](cdn-query-string.md). Om filen inte är Cacheable ställs har frågesträngen cachelagring inställningen ingen effekt, baserat på cachelagring regler och standardbeteenden för CDN.

Information om standard cachelagring direktivet sidhuvuden och funktionssätt för cachelagring finns [hur cachelagring fungerar](cdn-how-caching-works.md). 


## <a name="accessing-azure-cdn-caching-rules"></a>Åtkomst till Azure CDN cachelagring regler

1. Öppna Azure portal, Välj en CDN-profil och sedan väljer du en slutpunkt.

2. I det vänstra fönstret under inställningar, väljer **cachelagring regler**.

   ![Knappen CDN cachelagring regler](./media/cdn-caching-rules/cdn-caching-rules-btn.png)

   Den **cachelagring regler** visas.

   ![Sidan för cachelagring av CDN-regler](./media/cdn-caching-rules/cdn-caching-rules-page.png)


## <a name="caching-behavior-settings"></a>Inställningar för cachelagring beteende
För global och anpassade cachelagring regler, kan du ange följande **cachelagring av frågesträngar** inställningar:

- **Kringgå cache**: cachelagra inte och ignorera angivna ursprung cache-direktiv huvuden.

- **Åsidosätt**: ignorera angivna ursprung cache-direktiv huvuden; Använd angivna cachelagringens varaktighet i stället.

- **Ange om de saknas**: Kontrollera om ursprung cache-direktiv sidhuvud, om sådana finns; annars använder angivna cachelagringens varaktighet.

![Globala cachelagringsregler](./media/cdn-caching-rules/cdn-global-caching-rules.png)

![Anpassade cachelagringsregler](./media/cdn-caching-rules/cdn-custom-caching-rules.png)

## <a name="cache-expiration-duration"></a>Giltighetstid för cache
Du kan ange cache giltighetstiden för global och anpassade cachelagring regler, i dagar, timmar, minuter och sekunder:

- För den **åsidosätta** och **om saknas** **cachelagring av frågesträngar** inställningar, giltig cache varaktigheter intervallet mellan 0 sekunder och 366 dagar. För ett värde av 0 sekunder CDN cachelagrar innehållet, men måste verifiera varje begäran med den ursprungliga servern.

- För den **kringgå cache** inställningen cachelagringens varaktighet automatiskt till 0 sekunder och kan inte ändras.

## <a name="custom-caching-rules-match-conditions"></a>Anpassade cachelagringsregler matchar villkor

Två matchar villkor är tillgängliga för anpassade cache regler:
 
- **Sökvägen**: det här tillståndet matchar sökvägen för URL-Adressen, exklusive domännamnet och stöder jokertecknet (\*). Till exempel _/myfile.html_, _/min/mappen / *_, och _/my/images/*.jpg_. Den maximala längden är 260 tecken.

- **Tillägget**: det här tillståndet matchar filnamnstillägget för den begärda filen. Du kan ange en lista över kommaavgränsade filnamnstillägg som ska matcha. Till exempel _.jpg_, _.mp3_, eller _.png_. Det maximala antalet tillägg är 50 och det maximala antalet tecken per tillägget är 16. 

## <a name="global-and-custom-rule-processing-order"></a>Bearbetningsordningen för globala och anpassad regel
Global och anpassade cachelagring regler bearbetas i följande ordning:

- Globala cachelagring regler företräde framför CDN cachelagring standardbeteendet (HTTP-huvud för cache-direktiv inställningar). 

- Anpassade regler för cachelagring företräde framför globala cachelagring regler, där de tillämpas. Anpassade cachelagring regler bearbetas i ordning uppifrån och ned. Det vill säga om en begäran matchar bägge dessa villkor, åsidosätter regler längst ned i listan regler överst i listan. Därför ska du placera särskilda regler längst ned i listan.

**Exempel**:
- Globala cacheregeln: 
   - Cachelagring av frågesträngar: **åsidosätta**
   - Cachelagra giltighetstiden: 1 dag

- Anpassad cachelagring regel #1:
   - Matcha villkor: **sökväg**
   - Matcha värden:   _/home / *_
   - Cachelagring av frågesträngar: **åsidosätta**
   - Cachelagra giltighetstiden: 2 dagar

- Anpassad cachelagring regeln #2:
   - Matcha villkor: **tillägg**
   - Matcha värden: _.html_
   - Cachelagring av frågesträngar: **om saknas**
   - Cachelagra giltighetstiden: 3 dagar

När dessa regler är inställda, en begäran om  _&lt;slutpunktens värdnamn&gt;_.azureedge.net/home/index.html utlösare anpassade cachelagring regel #2, som har angetts till: **om saknas** och 3 dagar. Därför, om den *index.html* filen har `Cache-Control` eller `Expires` HTTP-huvuden som de är hanteras; annars, om dessa huvuden inte har angetts filen lagras i 3 dagar.

> [!NOTE] 
> Filer som cachelagras innan en ändring för regeln behåller sitt ursprung cache-inställningen för varaktighet. Om du vill återställa sina cache varaktighet, måste du [Rensa filen](cdn-purge-endpoint.md). För **Azure CDN från Verizon** slutpunkter kan det ta upp till 90 minuter för nya regler för cachelagring ska börja gälla.

## <a name="see-also"></a>Se också

- [Så här fungerar cachelagring](cdn-how-caching-works.md)
- [Självstudier: Ange Azure CDN cachelagring regler](cdn-caching-rules-tutorial.md)
