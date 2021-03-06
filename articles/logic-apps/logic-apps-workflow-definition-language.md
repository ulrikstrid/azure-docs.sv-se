---
title: Arbetsflödet Definition Language schema - Azure Logic Apps | Microsoft Docs
description: Skriva anpassade arbetsflödesdefinitioner för Logic Apps i Azure med arbetsflödet Definition Language
services: logic-apps
author: ecfan
manager: SyntaxC4
editor: ''
documentationcenter: ''
ms.assetid: 26c94308-aa0d-4730-97b6-de848bffff91
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: reference
ms.date: 04/25/2018
ms.author: estfan
ms.openlocfilehash: 7c253fd83bcc1f1dde93ac6ef0c26da1fa1a9a4b
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2018
---
# <a name="logic-apps-workflow-definitions-with-the-workflow-definition-language-schema"></a>Logic Apps arbetsflödesdefinitioner med språk i Arbetsflödesdefinitionen för schemat

När du skapar ett arbetsflöde med logik app med [Azure Logikappar](../logic-apps/logic-apps-overview.md), din underliggande arbetsflödesdefinitionen beskrivs den faktiska logik som körs för din logikapp. Den här beskrivningen följer en struktur som har definierats och godkänts av schemat språk i arbetsflödesdefinitionen som använder [JavaScript Object Notation (JSON)](https://www.json.org/) format. 
  
## <a name="workflow-definition-structure"></a>Arbetsflödets definition struktur

En arbetsflödesdefinition har minst en utlösare som instantierar logikappen plus en eller flera åtgärder som körs i din logikapp. 

Här är den övergripande strukturen för en arbetsflödesdefinition:  
  
```json
"definition": {
  "$schema": "<workflow-definition-language-schema-version>",
  "contentVersion": "<workflow-definition-version-number>",
  "parameters": { "<workflow-parameter-definitions>" },
  "triggers": { "<workflow-trigger-definitions>" },
  "actions": { "<workflow-action-definitions>" },
  "outputs": { "<workflow-output-definitions>" }
}
```
  
| Element | Krävs | Beskrivning | 
|---------|----------|-------------| 
| definition | Ja | Det första elementet för din arbetsflödesdefinitionen | 
| $schema | Endast när externt refererar till en arbetsflödesdefinition | Plats för JSON-schemafilen som beskriver den språk i arbetsflödesdefinitionen-versionen som finns här: <p>`https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json`</p> |   
| contentVersion | Nej | Versionsnummer för din arbetsflödesdefinitionen är ”1.0.0.0” som standard. Ange ett värde som ska användas för att identifiera och bekräfta rätt definition när du distribuerar ett arbetsflöde. | 
| parameters | Nej | Definitioner för en eller flera parametrar som överför data i ditt arbetsflöde <p><p>Maximal parametrar: 50 | 
| Utlösare | Nej | Definitioner för en eller flera utlösare som kan skapa en instans av arbetsflödet. Du kan definiera mer än en utlösare, men endast med arbetsflödet Definition Language, inte visuellt via Logic Apps Designer. <p><p>Maximal utlösare: 10 | 
| åtgärder | Nej | Definitioner för en eller flera åtgärder att köra vid körning av arbetsflöde <p><p>Maximal åtgärder: 250 | 
| utdata | Nej | Definitioner för utdata som returneras från ett arbetsflöde som kör <p><p>Maximal utdata: 10 |  
|||| 

## <a name="parameters"></a>Parametrar

I den `parameters` och definierar de parametrar som accepterar indata för arbetsflödet vid körning. Innan du kan använda dessa parametrar i andra avsnitt i arbetsflödet, se till att du deklarera alla parametrar i dessa avsnitt.

Här är den allmänna strukturen för en parameterdefinition:  

```json
"parameters": {
  "<parameter-name>": {
    "type": "<parameter-type>",
    "defaultValue": "<default-parameter-value>",
    "allowedValues": [ <array-with-permitted-parameter-values> ],
    "metadata": { 
      "key": { 
        "name": "<key-value>"
      } 
    }
  }
},
```

| Element | Krävs | Typ | Beskrivning |  
|---------|----------|------|-------------|  
| typ | Ja | int, float, string, securestring, bool, matris, JSON-objekt, secureobject <p><p>**Obs**: för alla lösenord, nycklar och hemligheter kan använda den `securestring` och `secureobject` typer eftersom de `GET` åtgärden returnerar inte dessa typer. | Typen för parametern |
| Standardvärde | Nej | Samma som `type` | Standard-parametervärdet när inget värde anges när arbetsflödet instansierar | 
| allowedValues | Nej | Samma som `type` | En matris med värden som kan användas med parametern |  
| metadata | Nej | JSON-objekt | Övriga parametern detaljer, till exempel namnet eller en läsbar beskrivning för din logikapp eller designläge data som används av Visual Studio eller andra verktyg |  
||||
  
## <a name="triggers-and-actions"></a>Utlösare och åtgärder  

I en arbetsflödesdefinition den `triggers` och `actions` avsnitten definierar anropen under körning av ditt arbetsflöde. Syntax och mer information om dessa avsnitt finns [arbetsflöde utlösare och åtgärder](../logic-apps/logic-apps-workflow-actions-triggers.md).
  
## <a name="outputs"></a>Utdata 

I den `outputs` avsnittet, definiera data som arbetsflödet kan returnera när du är klar kör. Ange till exempel att arbetsflödet utdata returnerar informationen för att spåra en viss status eller ett värde från varje körning. 

Här är den allmänna strukturen för en definition av utdata: 

```json
"outputs": {
  "<key-name>": {  
    "type": "<key-type>",  
    "value": "<key-value>"  
  }
} 
```

| Element | Krävs | Typ | Beskrivning | 
|---------|----------|------|-------------| 
| <*nyckel-namn*> | Ja | Sträng | Nyckelnamnet utdata för returvärde |  
| typ | Ja | int, float, string, securestring, bool, matris, JSON-objekt | Typen för returvärdet för utdata | 
| värde | Ja | Samma som `type` | Returvärdet för utdata |  
||||| 

Granska logikappen körningshistorik och information i Azure-portalen för att hämta utdata från ett arbetsflöde som körs eller använda den [arbetsflöde REST API](https://docs.microsoft.com/rest/api/logic/workflows). Du kan också skicka utdata till externa system, till exempel PowerBI så att du kan skapa instrumentpaneler. 

> [!NOTE]
> Använd inte när svarar på inkommande begäranden från en tjänst REST API: et `outputs`. Använd i stället de `Response` åtgärdstyp. Mer information finns i [arbetsflöde utlösare och åtgärder](../logic-apps/logic-apps-workflow-actions-triggers.md).

<a name="expressions"></a>

## <a name="expressions"></a>Uttryck

Med JSON, kan du ha litterala värden som finns vid designtillfället, till exempel:

```json
"customerName": "Sophia Owen", 
"rainbowColors": ["red", "orange", "yellow", "green", "blue", "indigo", "violet"], 
"rainbowColorsCount": 7 
```

Du kan också har värden som inte finns några tills körning. Du kan använda för att representera värdena *uttryck*, som utvärderas vid körning. Ett uttryck är en sekvens som kan innehålla en eller flera [funktioner](#functions), [operatörer](#operators), variabler, explicita värden eller konstanter. I din arbetsflödesdefinitionen du kan använda ett uttryck var som helst i ett JSON-strängvärde med uttrycket med @-tecknet (@). När du utvärderar ett uttryck som representerar ett JSON-värde, uttryck brödtexten hämtas genom att ta bort den @-tecken och alltid resulterar i en annan JSON-värde. 

Till exempel för tidigare definierad `customerName` egenskap, kan du hämta egenskapens värde med hjälp av den [parameters()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) fungera i ett uttryck och tilldela värdet till den `accountName` egenskapen:

```json
"customerName": "Sophia Owen", 
"accountName": "@parameters('customerName')"
```

*Sträng interpolerade* också kan du använda flera uttryck i strängar som omslutas av det @-tecken och klammerparenteser ({}). Här är syntax:

```json
@{ "<expression1>", "<expression2>" }
```

Resultatet är alltid en sträng, vilket gör den här funktionen liknar den `concat()` fungerar, till exempel: 

```json
"customerName": "First name: @{parameters('firstName')} Last name: @{parameters('lastName')}"
```

Om du har en teckensträng som börjar med den @-tecknet, prefix i @-tecknet med en annan @-tecknet som escape-tecken: @@

De här exemplen visar hur uttryck utvärderas:

| JSON-värde | Resultat |
|------------|--------| 
| ”Sophia Owen” | Returnera följande tecken: 'Sophia Owen' |
| ”array [1]” | Returnera följande tecken: 'array [1]' |
| "@@" | Returnerar tecknen som en sträng med ett tecken ”: @” |   
| " @" | Returnerar tecknen som en sträng med två tecken ”: @” |
|||

Anta att du anger ”myBirthMonth” lika med ”januari” och ”myAge” lika med antalet 42 för de här exemplen:  
  
```json
"myBirthMonth": "January",
"myAge": 42
```

De här exemplen visar hur följande uttryck utvärderas:

| JSON-uttryck | Resultat |
|-----------------|--------| 
| ”@parameters(myBirthMonth)” | Returnera följande sträng: ”januari” |  
| ”@{parameters('myBirthMonth')}” | Returnera följande sträng: ”januari” |  
| ”@parameters(myAge)” | Returnerar det antal: 42 |  
| ”@{parameters('myAge')}” | Returnerar det antal som en sträng: ”42” |  
| ”Min ålder @{parameters('myAge')}” | Returnera följande sträng: ”min ålder är 42” |  
| ”@concat(Min ålder är, string(parameters('myAge')))” | Returnera följande sträng: ”min ålder är 42” |  
| ”Min ålder är @@ {parameters('myAge')}” | Returnerar den här strängen, som innehåller uttrycket ”: min ålder @{parameters('myAge')}” | 
||| 

När du arbetar visuellt i Logic Apps Designer, kan du skapa uttryck via uttrycket builder, till exempel: 

![Logic Apps Designer > uttrycksbyggare](./media/logic-apps-workflow-definition-language/expression-builder.png)

När du är klar uttrycket visas för motsvarande egenskap i din arbetsflödesdefinitionen, till exempel, `searchQuery` egenskapen här:

```json
"Search_tweets": {
  "inputs": {
    "host": {
      "connection": {
       "name": "@parameters('$connections')['twitter']['connectionId']"
      }
    }
  },
  "method": "get",
  "path": "/searchtweets",
  "queries": {
    "maxResults": 20,
    "searchQuery": "Azure @{concat('firstName','', 'LastName')}"
  }
},
```

<a name="operators"></a>

## <a name="operators"></a>Operatorer

I [uttryck](#expressions) och [funktioner](#functions), operatörer utföra vissa uppgifter, till exempel en egenskap eller ett värde i en matris. 

| Operator | Aktivitet | 
|----------|------|
| ' | Om du vill använda en stränglitteral som indata eller i uttryck och funktioner, omsluter strängen endast med enkla citattecken, till exempel `'<myString>'`. Använd inte dubbla citattecken (””), vilket står i konflikt med JSON-formatering runt ett helt uttryck. Exempel: <p>**Ja**: length('Hello') </br>**Inte**: length("Hello") <p>När du skickar matriser eller siffror, behöver du inte wrapping skiljetecken. Exempel: <p>**Ja**: längd ([1, 2, 3]) </br>**Inte**: längd (”[1, 2, 3]”) | 
| [] | Om du vill referera till ett värde på en viss plats (index) i en matris, använder du hakparenteser. Till exempel för att hämta det andra objektet i en matris: <p>`myArray[1]` | 
| . | Använd punktoperatorn för att referera till en egenskap i ett objekt. Till exempel för att hämta den `name` -egenskapen för en `customer` JSON-objekt: <p>`"@parameters('customer').name"` | 
| ? | Om du vill referera null egenskaper i ett objekt utan ett körningsfel, använder du operatorn frågetecken. Du kan till exempel använda det här uttrycket för att hantera null utdata från en utlösare: <p>`@coalesce(trigger().outputs?.body?.<someProperty>, '<property-default-value>')` | 
||| 

<a name="functions"></a>

## <a name="functions"></a>Funktioner

Vissa uttryck hämta sina värden från runtime-åtgärder som inte kanske ännu finns när en logikapp börjar köras. Du kan använda för att referera eller arbeta med dessa värden i uttryck, *funktioner*. T.ex, du kan använda matematiska funktioner för beräkningar, som den [Add ()](../logic-apps/workflow-definition-language-functions-reference.md#add) som returnerar summan från heltal eller flyttal. 

Här följer ett par exempel på uppgifter som du kan utföra med funktioner: 

| Aktivitet | Funktionen | Resultat | 
| ---- | --------------- | -------------- | 
| Returnera en sträng i gemener format. | toLower (”<*text*>”) <p>Till exempel: toLower('Hello') | ”hello” | 
| Returnera en globalt unik identifierare (GUID). | GUID() |”c2ecc88d-88c8-4096-912c-d6f2e2b138ce” | 
|||| 

Det här exemplet illustrerar hur du kan hämta värdet från den `customerName` parametern och tilldela ett värde till den `accountName` egenskapen med hjälp av den [parameters()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) funktion i ett uttryck:

```json
"accountName": "@parameters('customerName')"
```

Här följer några andra allmänna metoder för att du kan använda i uttryck:

| Aktivitet | Syntaxen för funktionen i ett uttryck | 
| ---- | -------------------------------- | 
| Utföra arbete med ett objekt genom att överföra objektet till en funktion. | ”@<*functionName*> (<*objektet*>)” | 
| 1. Hämta den *parameterName*'s värde med hjälp av den kapslade `parameters()` funktion. </br>2. Utföra arbetet med resultatet genom att ange värdet till *functionName*. | ”@<*functionName*> (parametrar (” <*parameterName*>')) ” | 
| 1. Hämta resultatet från den kapslade inre funktionen *functionName*. </br>2. Skickar resultatet till funktionen yttre *functionName2*. | ”@<*functionName2*> (<*functionName*> (<*objektet*>))” | 
| 1. Hämta resultatet från *functionName*. </br>2. Med hänsyn till att resultatet är ett objekt med egenskapen *propertyName*, hämta egenskapens värde. | ”@<*functionName*>(<*item*>). <*propertyName*>” | 
||| 

Till exempel den `concat()` funktionen kan ta minst två strängvärden som parametrar. Den här funktionen kombinerar de strängarna till en sträng. Du kan antingen skicka i stränglitteraler, till exempel ”Sophia” och ”Owen” så att du får en kombinerad sträng, ”SophiaOwen”:

```json
"customerName": "@concat('Sophia', 'Owen')"
```

Eller så kan du få strängvärden från parametrar. Det här exemplet används den `parameters()` funktion i varje `concat()` parameter och `firstName` och `lastName` parametrar. Du sedan skickar de resulterande strängarna till den `concat()` fungera så att du får en kombinerad sträng, till exempel ”SophiaOwen”:

```json
"customerName": "@concat(parameters('firstName'), parameters('lastName'))"
```

Oavsett hur både exempel tilldela resultatet ska den `customerName` egenskapen. 

Detaljerad information om varje funktion finns i [alfabetisk referensartikeln](../logic-apps/workflow-definition-language-functions-reference.md).
Eller fortsätta lära dig mer om funktioner som är baserat på deras generella.

<a name="string-functions"></a>

### <a name="string-functions"></a>Strängfunktioner

Om du vill arbeta med strängar som du kan använda dessa strängfunktioner och även vissa [samling funktioner](#collection-functions). Strängfunktioner fungerar endast för strängar. 

| Funktionen String | Aktivitet | 
| --------------- | ---- | 
| [concat](../logic-apps/workflow-definition-language-functions-reference.md#concat) | Kombinera två eller flera strängar och returnera kombinerade strängen. | 
| [endsWith](../logic-apps/workflow-definition-language-functions-reference.md#endswith) | Kontrollera om en sträng som slutar med den angivna delsträngen. | 
| [GUID](../logic-apps/workflow-definition-language-functions-reference.md#guid) | Generera en globalt unik identifierare (GUID) som en sträng. | 
| [indexOf](../logic-apps/workflow-definition-language-functions-reference.md#indexof) | Returnera startpositionen för en understräng. | 
| [lastIndexOf](../logic-apps/workflow-definition-language-functions-reference.md#lastindexof) | Returnera den sista positionen för en understräng. | 
| [Ersätt](../logic-apps/workflow-definition-language-functions-reference.md#replace) | Ersätta en understräng med den angivna strängen och returnerar uppdaterade strängen. | 
| [split](../logic-apps/workflow-definition-language-functions-reference.md#split) | Returnera en matris som har alla tecken från en sträng och avgränsar varje tecken med ett specifikt avgränsningstecken. | 
| [StartsWith](../logic-apps/workflow-definition-language-functions-reference.md#startswith) | Kontrollera om en sträng som börjar med en specifik delsträng. | 
| [delsträngen](../logic-apps/workflow-definition-language-functions-reference.md#substring) | Returnera tecken från en sträng från den angivna positionen. | 
| [toLower](../logic-apps/workflow-definition-language-functions-reference.md#toLower) | Returnera en sträng i gemener format. | 
| [toUpper](../logic-apps/workflow-definition-language-functions-reference.md#toUpper) | Returnera en sträng i versaler format. | 
| [Rensa](../logic-apps/workflow-definition-language-functions-reference.md#trim) | Ta bort inledande och avslutande blanksteg från en sträng och returnera uppdaterade sträng. | 
||| 

<a name="collection-functions"></a>

### <a name="collection-functions"></a>Samlingen funktioner

Om du vill arbeta med samlingar, vanligtvis matriser, strängar och ibland ordlistor, kan du använda dessa funktioner för samlingen. 

| Funktionen för samlingen | Aktivitet | 
| ------------------- | ---- | 
| [Innehåller](../logic-apps/workflow-definition-language-functions-reference.md#contains) | Kontrollera om en samling har ett specifikt objekt. |
| [tom](../logic-apps/workflow-definition-language-functions-reference.md#empty) | Kontrollera om en samling är tom. | 
| [första](../logic-apps/workflow-definition-language-functions-reference.md#first) | Returnera det första objektet från en samling. | 
| [skärningspunkten](../logic-apps/workflow-definition-language-functions-reference.md#intersection) | Returnera en samling som har *endast* vanliga objekt mellan de angivna samlingarna. | 
| [join](../logic-apps/workflow-definition-language-functions-reference.md#join) | Returnerar en sträng som har *alla* objekt från en matris, avgränsade med det angivna tecknet. | 
| [senaste](../logic-apps/workflow-definition-language-functions-reference.md#last) | Returnera det sista objektet från en samling. | 
| [Längd](../logic-apps/workflow-definition-language-functions-reference.md#length) | Returnera antalet objekt i en sträng eller en matris. | 
| [skip](../logic-apps/workflow-definition-language-functions-reference.md#skip) | Ta bort objekt från längst fram i en mängd och returnerar *alla andra* objekt. | 
| [ta](../logic-apps/workflow-definition-language-functions-reference.md#take) | Returnera objekt från framför en samling. | 
| [Union](../logic-apps/workflow-definition-language-functions-reference.md#union) | Returnera en samling som har *alla* artiklar från de angivna samlingarna. | 
||| 

<a name="comparison-functions"></a>

### <a name="comparison-functions"></a>Jämförelse funktioner

Om du vill arbeta med villkor, jämför värden och uttryck resultaten eller utvärdera olika typer av logik, kan du använda dessa funktioner för jämförelse. Fullständig referens om varje funktion finns i [alfabetisk referensartikeln](../logic-apps/workflow-definition-language-functions-reference.md).

| Jämförelsefunktion | Aktivitet | 
| ------------------- | ---- | 
| [Och](../logic-apps/workflow-definition-language-functions-reference.md#and) | Kontrollera om alla uttryck utvärderas som true. | 
| [är lika med](../logic-apps/workflow-definition-language-functions-reference.md#equals) | Kontrollera om båda värdena är likvärdiga. | 
| [större](../logic-apps/workflow-definition-language-functions-reference.md#greater) | Kontrollera om det första värdet är större än det andra värdet. | 
| [greaterOrEquals](../logic-apps/workflow-definition-language-functions-reference.md#greaterOrEquals) | Kontrollera om det första värdet är större än eller lika med det andra värdet. | 
| [Om](../logic-apps/workflow-definition-language-functions-reference.md#if) | Kontrollera om ett uttryck är SANT eller FALSKT. Baserat på resultatet returnera ett angivet värde. | 
| [mindre](../logic-apps/workflow-definition-language-functions-reference.md#less) | Kontrollera om det första värdet är mindre än det andra värdet. | 
| [lessOrEquals](../logic-apps/workflow-definition-language-functions-reference.md#lessOrEquals) | Kontrollera om det första värdet är mindre än eller lika med det andra värdet. | 
| [inte](../logic-apps/workflow-definition-language-functions-reference.md#not) | Kontrollera om ett uttryck är false. | 
| [Eller](../logic-apps/workflow-definition-language-functions-reference.md#or) | Kontrollera om minst ett uttryck är sant. |
||| 

<a name="conversion-functions"></a>

### <a name="conversion-functions"></a>Konverteringsfunktioner

Om du vill ändra ett värde typ eller format kan du använda dessa konverteringsfunktioner. Du kan till exempel ändra ett värde från ett booleskt värde till ett heltal. Information om hur Logic Apps hanterar innehållstyper vid konvertering finns [hantera innehållstyper](../logic-apps/logic-apps-content-type.md). Fullständig referens om varje funktion finns i [alfabetisk referensartikeln](../logic-apps/workflow-definition-language-functions-reference.md).

| Funktion för datumkonvertering | Aktivitet | 
| ------------------- | ---- | 
| [matris](../logic-apps/workflow-definition-language-functions-reference.md#array) | Returnera en matris från en enda angivna indata. Flera inmatningar finns [createArray](../logic-apps/workflow-definition-language-functions-reference.md#createArray). | 
| [Base64](../logic-apps/workflow-definition-language-functions-reference.md#base64) | Returnera den base64-kodade versionen för en sträng. | 
| [base64ToBinary](../logic-apps/workflow-definition-language-functions-reference.md#base64ToBinary) | Returnera den binära versionen för en base64-kodad sträng. | 
| [base64ToString](../logic-apps/workflow-definition-language-functions-reference.md#base64ToString) | Returnera strängversionen för en base64-kodad sträng. | 
| [Binär](../logic-apps/workflow-definition-language-functions-reference.md#binary) | Returnera den binära versionen för ett indatavärde. | 
| [bool](../logic-apps/workflow-definition-language-functions-reference.md#bool) | Returnera booleskt versionen för ett indatavärde. | 
| [createArray](../logic-apps/workflow-definition-language-functions-reference.md#createArray) | Returnera en matris från flera inmatningar. | 
| [dataUri](../logic-apps/workflow-definition-language-functions-reference.md#dataUri) | Returnera data-URI för ett indatavärde. | 
| [dataUriToBinary](../logic-apps/workflow-definition-language-functions-reference.md#dataUriToBinary) | Returnera den binära versionen för en data-URI. | 
| [dataUriToString](../logic-apps/workflow-definition-language-functions-reference.md#dataUriToString) | Returnera strängversionen för en data-URI. | 
| [decodeBase64](../logic-apps/workflow-definition-language-functions-reference.md#decodeBase64) | Returnera strängversionen för en base64-kodad sträng. | 
| [decodeDataUri](../logic-apps/workflow-definition-language-functions-reference.md#decodeDataUri) | Returnera den binära versionen för en data-URI. | 
| [decodeUriComponent](../logic-apps/workflow-definition-language-functions-reference.md#decodeUriComponent) | Returnerar en sträng som ersätter escape-tecken med avkodade versioner. | 
| [encodeUriComponent](../logic-apps/workflow-definition-language-functions-reference.md#encodeUriComponent) | Returnera en sträng som ersätter URL-osäkra tecken med escape-tecken. | 
| [flyttal](../logic-apps/workflow-definition-language-functions-reference.md#float) | Returnerar ett flyttal peka nummer för ett indatavärde. | 
| [int](../logic-apps/workflow-definition-language-functions-reference.md#int) | Returnera heltal versionen för en sträng. | 
| [JSON](../logic-apps/workflow-definition-language-functions-reference.md#json) | Returnera JavaScript Object Notation (JSON) TYPVÄRDE eller objekt för en sträng eller XML. | 
| [Sträng](../logic-apps/workflow-definition-language-functions-reference.md#string) | Returnera strängversionen för ett indatavärde. | 
| [uriComponent](../logic-apps/workflow-definition-language-functions-reference.md#uriComponent) | Returnera den URI-kodad versionen för ett indatavärde genom att ersätta URL-osäkra tecken med escape-tecken. | 
| [uriComponentToBinary](../logic-apps/workflow-definition-language-functions-reference.md#uriComponentToBinary) | Returnera den binära versionen för en URI-kodad sträng. | 
| [uriComponentToString](../logic-apps/workflow-definition-language-functions-reference.md#uriComponentToString) | Returnera strängversionen för en URI-kodad sträng. | 
| [xml](../logic-apps/workflow-definition-language-functions-reference.md#xml) | Returnera XML-version för en sträng. | 
||| 

<a name="math-functions"></a>

### <a name="math-functions"></a>Matematiska funktioner

Du kan använda dessa matematiska funktioner för att fungera med heltal och flyttal. Fullständig referens om varje funktion finns i [alfabetisk referensartikeln](../logic-apps/workflow-definition-language-functions-reference.md).

| Math funktion | Aktivitet | 
| ------------- | ---- | 
| [Lägg till](../logic-apps/workflow-definition-language-functions-reference.md#add) | Returnera resultatet från att lägga till två tal. | 
| [div](../logic-apps/workflow-definition-language-functions-reference.md#div) | Returnera resultatet från divisionen av två tal. | 
| [max](../logic-apps/workflow-definition-language-functions-reference.md#max) | Returnera det högsta värdet från en uppsättning tal eller en matris. | 
| [Min](../logic-apps/workflow-definition-language-functions-reference.md#min) | Returnera det lägsta värdet från en uppsättning tal eller en matris. | 
| [MOD](../logic-apps/workflow-definition-language-functions-reference.md#mod) | Returnera resten från att dela två tal. | 
| [mul](../logic-apps/workflow-definition-language-functions-reference.md#mul) | Returnera produkten från multiplicera två tal. | 
| [SLUMP](../logic-apps/workflow-definition-language-functions-reference.md#rand) | Returnera ett slumpmässigt heltal från ett visst intervall. | 
| [intervallet](../logic-apps/workflow-definition-language-functions-reference.md#range) | Returnera en heltalsmatris som startar från en angiven heltal. | 
| [Sub](../logic-apps/workflow-definition-language-functions-reference.md#sub) | Returnera resultatet från att subtrahera det andra talet från det första talet. | 
||| 

<a name="date-time-functions"></a>

### <a name="date-and-time-functions"></a>Datum- och tidsfunktioner

Du kan använda dessa funktioner för datum och tid om du vill arbeta med datum och klockslag.
Fullständig referens om varje funktion finns i [alfabetisk referensartikeln](../logic-apps/workflow-definition-language-functions-reference.md).

| Funktionen för datum och tid | Aktivitet | 
| --------------------- | ---- | 
| [addDays](../logic-apps/workflow-definition-language-functions-reference.md#addDays) | Lägga till ett antal dagar i en tidsstämpel. | 
| [addHours](../logic-apps/workflow-definition-language-functions-reference.md#addHours) | Lägga till flera timmar att en tidsstämpel. | 
| [addMinutes](../logic-apps/workflow-definition-language-functions-reference.md#addMinutes) | Lägga till ett antal minuter i en tidsstämpel. | 
| [Lägg_till_sekunder](../logic-apps/workflow-definition-language-functions-reference.md#addSeconds) | Lägga till ett antal sekunder i en tidsstämpel. |  
| [addToTime](../logic-apps/workflow-definition-language-functions-reference.md#addToTime) | Lägga till ett antal tidsenheter i en tidsstämpel. Se även [getFutureTime](../logic-apps/workflow-definition-language-functions-reference.md#getFutureTime). | 
| [convertFromUtc](../logic-apps/workflow-definition-language-functions-reference.md#convertFromUtc) | Konvertera en tidsstämpel från Universal Coordinated för tid (UTC) till Målets tidszon. | 
| [convertTimeZone](../logic-apps/workflow-definition-language-functions-reference.md#convertTimeZone) | Konvertera en tidsstämpel från käll-tidszonen till Målets tidszon. | 
| [convertToUtc](../logic-apps/workflow-definition-language-functions-reference.md#convertToUtc) | Konvertera en tidsstämpel från källtidszonen till universell tid UTC (UTC). | 
| [Dag i månaden](../logic-apps/workflow-definition-language-functions-reference.md#dayOfMonth) | Returnera dagen i månadskomponenten från en tidsstämpel. | 
| [DayOfWeek](../logic-apps/workflow-definition-language-functions-reference.md#dayOfWeek) | Returnera dagen i veckodagskomponenten från en tidsstämpel. | 
| [DayOfYear](../logic-apps/workflow-definition-language-functions-reference.md#dayOfYear) | Returnera dagen på året komponenten från en tidsstämpel. | 
| [FormatDateTime](../logic-apps/workflow-definition-language-functions-reference.md#formatDateTime) | Returnera datumet från en tidsstämpel. | 
| [getFutureTime](../logic-apps/workflow-definition-language-functions-reference.md#getFutureTime) | Returnera aktuell tidsstämpel plus de angivna tidsenheterna. Se även [addToTime](../logic-apps/workflow-definition-language-functions-reference.md#addToTime). | 
| [getPastTime](../logic-apps/workflow-definition-language-functions-reference.md#getPastTime) | Returnera aktuell tidsstämpel minus angivna enheterna. Se även [subtractFromTime](../logic-apps/workflow-definition-language-functions-reference.md#subtractFromTime). | 
| [startOfDay](../logic-apps/workflow-definition-language-functions-reference.md#startOfDay) | Returnera början på dagen för en tidsstämpel. | 
| [startOfHour](../logic-apps/workflow-definition-language-functions-reference.md#startOfHour) | Returnera början på timmen för en tidsstämpel. | 
| [startOfMonth](../logic-apps/workflow-definition-language-functions-reference.md#startOfMonth) | Returnera början på månaden för en tidsstämpel. | 
| [subtractFromTime](../logic-apps/workflow-definition-language-functions-reference.md#subtractFromTime) | Ta bort ett antal tidsenheter från en tidsstämpel. Se även [getPastTime](../logic-apps/workflow-definition-language-functions-reference.md#getPastTime). | 
| [skalstreck](../logic-apps/workflow-definition-language-functions-reference.md#ticks) | Returnera den `ticks` egenskapsvärdet för en angiven tidsstämpel. | 
| [utcNow](../logic-apps/workflow-definition-language-functions-reference.md#utcNow) | Returnera aktuell tidsstämpel som en sträng. | 
||| 

<a name="workflow-functions"></a>

### <a name="workflow-functions"></a>Funktioner för arbetsflöde

Med hjälp av dessa funktioner i arbetsflödet kan du:

* Få information om en arbetsflödesinstans vid körning. 
* Arbeta med indata används för en instans skapades av logikappar.
* Referera utdata från utlösare och åtgärder.

Du kan till exempel referera till utdata från en åtgärd och använda dessa data i en senare åtgärd. Fullständig referens om varje funktion finns i [alfabetisk referensartikeln](../logic-apps/workflow-definition-language-functions-reference.md).

| Funktionen för arbetsflöde | Aktivitet | 
| ----------------- | ---- | 
| [Åtgärden](../logic-apps/workflow-definition-language-functions-reference.md#action) | Returnera den aktuella åtgärden utdata i körningen eller värden från andra JSON namn och värde-par. Se även [åtgärder](../logic-apps/workflow-definition-language-functions-reference.md#actions). | 
| [actionBody](../logic-apps/workflow-definition-language-functions-reference.md#actionBody) | Returnera en åtgärd `body` utdata vid körning. Se även [brödtext](../logic-apps/workflow-definition-language-functions-reference.md#body). | 
| [actionOutputs](../logic-apps/workflow-definition-language-functions-reference.md#actionOutputs) | Returnera utdata för en åtgärd vid körning. Se [åtgärder](../logic-apps/workflow-definition-language-functions-reference.md#actions). | 
| [åtgärder](../logic-apps/workflow-definition-language-functions-reference.md#actions) | Returnera utdata för en åtgärd i körningen eller värden från andra JSON namn och värde-par. Se även [åtgärd](../logic-apps/workflow-definition-language-functions-reference.md#action).  | 
| [Brödtext](#body) | Returnera en åtgärd `body` utdata vid körning. Se även [actionBody](../logic-apps/workflow-definition-language-functions-reference.md#actionBody). | 
| [formDataMultiValues](../logic-apps/workflow-definition-language-functions-reference.md#formDataMultiValues) | Skapa en matris med de värden som matchar ett nyckelnamn i *formulärdata* eller *formulär-kodade* åtgärd utdata. | 
| [formDataValue](../logic-apps/workflow-definition-language-functions-reference.md#formDataValue) | Returnerar ett värde som matchar ett nyckelnamn i en åtgärd *formulärdata* eller *formulär-kodade utdata*. | 
| [Objektet](../logic-apps/workflow-definition-language-functions-reference.md#item) | Returnera det aktuella objektet i matrisen under åtgärdens aktuella iteration när inuti en upprepande åtgärd över en matris. | 
| [Objekt](../logic-apps/workflow-definition-language-functions-reference.md#items) | När du är i ett för varje eller vill tills slinga, returnera det aktuella objektet från den angivna loopen.| 
| [listCallbackUrl](../logic-apps/workflow-definition-language-functions-reference.md#listCallbackUrl) | Returnera ”återanrop URL: en” som anropar en utlösare eller åtgärd. | 
| [multipartBody](../logic-apps/workflow-definition-language-functions-reference.md#multipartBody) | Returnera innehållet för en viss del i utdata för en åtgärd som har flera delar. | 
| [parameters](../logic-apps/workflow-definition-language-functions-reference.md#parameters) | Returnera värdet för en parameter som beskrivs i din app logik-definition. | 
| [Utlösare](../logic-apps/workflow-definition-language-functions-reference.md#trigger) | Returnera en utlösare utdata vid körning, eller från andra JSON namn och värde-par. Se även [triggerOutputs](#triggerOutputs) och [triggerBody](../logic-apps/workflow-definition-language-functions-reference.md#triggerBody). | 
| [triggerBody](../logic-apps/workflow-definition-language-functions-reference.md#triggerBody) | Returnera en utlösare `body` utdata vid körning. Se [utlösaren](../logic-apps/workflow-definition-language-functions-reference.md#trigger). | 
| [triggerFormDataValue](../logic-apps/workflow-definition-language-functions-reference.md#triggerFormDataValue) | Returnera ett enstaka värde som matchar ett nyckelnamn i *formulärdata* eller *formulär-kodade* utlösa utdata. | 
| [triggerMultipartBody](../logic-apps/workflow-definition-language-functions-reference.md#triggerMultipartBody) | Returnera innehållet för en viss del i en utlösare multipart utdata. | 
| [triggerFormDataMultiValues](../logic-apps/workflow-definition-language-functions-reference.md#triggerFormDataMultiValues) | Skapa en matris som har värden som matchar ett nyckelnamn i *formulärdata* eller *formulär-kodade* utlösa utdata. | 
| [triggerOutputs](../logic-apps/workflow-definition-language-functions-reference.md#triggerOutputs) | Returnera en utlösare utdata i körningen eller värden från andra JSON namn och värde-par. Se [utlösaren](../logic-apps/workflow-definition-language-functions-reference.md#trigger). | 
| [variabler](../logic-apps/workflow-definition-language-functions-reference.md#variables) | Returnera värdet för en specifik variabel. | 
| [Arbetsflöde](../logic-apps/workflow-definition-language-functions-reference.md#workflow) | Returnera alla detaljer om själva arbetsflödet vid körning. | 
||| 

<a name="uri-parsing-functions"></a>

### <a name="uri-parsing-functions"></a>URI parsning av funktioner

Du kan använda dessa URI parsning funktioner för att arbeta med (uniform resource Identifier) och hämta olika egenskapsvärden för dessa URI: er. Fullständig referens om varje funktion finns i [alfabetisk referensartikeln](../logic-apps/workflow-definition-language-functions-reference.md).

| URI parsning funktion | Aktivitet | 
| -------------------- | ---- | 
| [uriHost](../logic-apps/workflow-definition-language-functions-reference.md#uriHost) | Returnera den `host` värde för en uniform resource identifier (URI). | 
| [uriPath](../logic-apps/workflow-definition-language-functions-reference.md#uriPath) | Returnera den `path` värde för en uniform resource identifier (URI). | 
| [uriPathAndQuery](../logic-apps/workflow-definition-language-functions-reference.md#uriPathAndQuery) | Returnera den `path` och `query` värden för en uniform resource identifier (URI). | 
| [uriPort](../logic-apps/workflow-definition-language-functions-reference.md#uriPort) | Returnera den `port` värde för en uniform resource identifier (URI). | 
| [uriQuery](../logic-apps/workflow-definition-language-functions-reference.md#uriQuery) | Returnera den `query` värde för en uniform resource identifier (URI). | 
| [UriScheme](../logic-apps/workflow-definition-language-functions-reference.md#uriScheme) | Returnera den `scheme` värde för en uniform resource identifier (URI). | 
||| 

<a name="manipulation-functions"></a>

### <a name="json-and-xml-functions"></a>JSON- och XML-funktioner

Om du vill arbeta med JSON-objekt och XML-noder kan du använda dessa funktioner för datahantering. Fullständig referens om varje funktion finns i [alfabetisk referensartikeln](../logic-apps/workflow-definition-language-functions-reference.md).

| Datahanteringsfunktion | Aktivitet | 
| --------------------- | ---- | 
| [addProperty](../logic-apps/workflow-definition-language-functions-reference.md#addProperty) | Lägg till en egenskap och dess värde eller namn / värde-par till ett JSON-objekt och returnerar det uppdaterade objektet. | 
| [Slå samman](../logic-apps/workflow-definition-language-functions-reference.md#coalesce) | Returnera det första icke-null-värdet från en eller flera parametrar. | 
| [removeProperty](../logic-apps/workflow-definition-language-functions-reference.md#removeProperty) | Ta bort en egenskap från ett JSON-objekt och returnerar det uppdaterade objektet. | 
| [setProperty](../logic-apps/workflow-definition-language-functions-reference.md#setProperty) | Ange värdet för egenskapen för ett JSON-objekt och returnerar det uppdaterade objektet. | 
| [XPath](../logic-apps/workflow-definition-language-functions-reference.md#xpath) | Kontrollera XML-noder eller värden som matchar ett XPath (XML Path Language)-uttryck och returnerar matchande noder eller värden. | 
||| 

## <a name="next-steps"></a>Nästa steg

* Lär dig mer om [språk i Arbetsflödesdefinitionen för åtgärder och utlösare](../logic-apps/logic-apps-workflow-actions-triggers.md)
* Lär dig mer om att skapa och hantera logikappar med programmässigt den [arbetsflöde REST API](https://docs.microsoft.com/rest/api/logic/workflows)
