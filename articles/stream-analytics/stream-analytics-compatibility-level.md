---
title: Förstå kompatibilitetsnivån för Azure Stream Analytics-jobb
description: Lär dig hur du anger en kompatibilitetsnivå för ett Azure Stream Analytics-jobb och större ändringar i den senaste kompatibilitetsnivån
services: stream-analytics
author: jasonwhowell
ms.author: jasonh
manager: kfile
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/03/2018
ms.openlocfilehash: 32e73918b2dd98822d42d74002b705ff730145d9
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/06/2018
---
# <a name="compatibility-level-for-azure-stream-analytics-jobs"></a>Kompatibilitetsnivån för Azure Stream Analytics-jobb
 
Kompatibilitetsnivån refererar till version-specifika funktioner för Azure Stream Analytics-tjänsten. Azure Stream Analytics är en hanterad tjänst med för regelbundna funktionsuppdateringar och prestanda. Vanligtvis görs uppdateringar automatiskt tillgängliga för slutanvändarna. Vissa nya funktioner kan dock införa större ändringar sådana som-förändringen av ett befintligt jobb, ändra i processer som använder data från dessa jobb osv. En kompatibilitetsnivå som används för att representera en större ändring som introducerades i Stream Analytics. Större ändringar introduceras alltid med en ny kompatibilitetsnivå. 

Kompatibilitetsnivån ser till att befintliga jobb körs utan några fel. När du skapar ett nytt Stream Analytics-jobb är en bra idé att skapa den med hjälp av den senaste kompatibilitetsnivå som är tillgängliga för dig. 
 
## <a name="set-a-compatibility-level"></a>Ange en kompatibilitetsnivå 

Kompatibilitetsnivån styr beteende under körning av ett stream analytics-jobbet. Du kan ange kompatibilitetsnivån för ett Stream Analytics-jobb med hjälp av portalen eller med hjälp av den [jobbet REST API-anropet för att skapa](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job). Azure Stream Analytics stöder för närvarande två kompatibilitet nivåer-”1.0” och ”1.1”. Kompatibilitetsnivån är som standard ”1.0”, som introducerades vid allmän tillgänglighet för Azure Stream Analytics. Om du vill uppdatera standardvärdet, navigera till din befintliga Stream Analytics-jobbet > Välj den **kompatibilitetsnivå** alternativet i **konfigurera** avsnittet och ändra värdet. 

Kontrollera att du stoppa jobbet innan du uppdaterar kompatibilitetsnivå. Du kan inte uppdatera kompatibilitetsnivån om jobbet är i körningstillstånd. 

![Kompatibilitetsnivån på portalen](media\stream-analytics-compatibility-level/image1.png)

 
När du uppdaterar kompatibilitetsnivån verifierar T-SQL-kompileraren jobbet med syntaxen som motsvarar den valda kompatibilitetsnivån. 

## <a name="major-changes-in-the-latest-compatibility-level-11"></a>Större ändringar i den senaste kompatibilitetsnivån (1.1)

Följande viktiga ändringar har införts i kompatibilitetsnivå 1.1:

* **Service Bus XML-format**  

  * **tidigare versioner:** Azure Stream Analytics används DataContractSerializer, så att innehållet i meddelandet ingår XML-taggar. Exempel:
    
   @\u0006string\b3http://schemas.microsoft.com/2003/10/Serialization/\u0001{ “SensorId”:”1”, “Temperature”:64\}\u0001 

  * **aktuell version:** innehållet i meddelandet innehåller dataströmmen direkt med inga ytterligare taggar. Exempel:
  
   {”SensorId”: ”1”, ”temperatur”: 64} 
 
* **Spara skiftlägeskänslighet för fältnamn**  

  * **tidigare versioner:** fältnamn ändrades till gemener när de bearbetas av Azure Stream Analytics-motorn. 

  * **aktuell version:** skiftlägeskänslighet sparas för fältnamn när de bearbetas av Azure Stream Analytics-motorn. 

  > [!NOTE] 
  > Spara skiftlägeskänslighet är ännu inte tillgänglig för dataströmmen analytiska jobb finns med hjälp av Edge-miljö. Därför kan konverteras alla fältnamn till gemener om ditt jobb finns på kanten. 

* **FloatNaNDeserializationDisabled**  

  * **tidigare versioner:** CREATE TABLE-kommando inte att filtrera händelser med NaN (inte ett tal. Till exempel oändligt, -oändligt) i en FLOAT kolumn eftersom de ligger utanför det dokumenterade intervallet för dessa siffror.

  * **aktuell version:** CREATE TABLE kan du ange ett starkt schema. Stream Analytics-motorn validerar att informationen som överensstämmer med det här schemat. Kommandot kan filtrera händelser med NaN-värden med den här modellen. 

* **Inaktivera automatisk upcast för datetime-strängar i JSON.**  

  * **tidigare versioner:** JSON-parsern skulle automatiskt ”uppåt” sträng värden med datum / / tidszonsinformation för DateTime anger och sedan konvertera den till UTC. Detta resulterade i att förlora Tidszonsinformationen.

  * **aktuell version:** det finns inga fler automatiskt ”uppåt” av strängvärden med datum/tidszon information till typen DateTime. Därför sparas Tidszonsinformationen. 

## <a name="next-steps"></a>Nästa steg
* [Felsökningsguide för Azure Stream Analytics](stream-analytics-troubleshooting-guide.md)
* [Stream Analytics hälsa resursbladet](stream-analytics-resource-health.md)