---
title: "Importera ett SOAP-API och konvertera till RESTEN med hjälp av Azure portal | Microsoft Docs"
description: "Lär dig hur du importerar ett SOAP-API och konvertera det till RESTEN med API-hantering."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/22/2017
ms.author: apimpm
ms.openlocfilehash: 74935f11d5bf3c83aef46c1d41fccd81ad312acb
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/04/2017
---
# <a name="import-a-soap-api-and-convert-to-rest"></a>Importera ett SOAP-API och konvertera till REST

Den här artikeln visar hur du importerar ett SOAP-API och konvertera det till RESTEN. Artikeln beskriver också hur du testar APIM-API.

I den här artikeln får du lära dig hur du:

> [!div class="checklist"]
> * Importera ett SOAP-API och konvertera till REST
> * Testa API: et i Azure-portalen
> * Testa API: N i Developer-portalen

## <a name="prerequisites"></a>Krav

Slutför följande Snabbstart: [skapa en instans av Azure API Management](get-started-create-service-instance.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"></a>Import och publicera en backend-API

1. Välj **API: er** från under **API MANAGEMENT**.
2. Välj **WSDL** från den **lägga till ett nytt API** lista.

    ![SOAP-API](./media/restify-soap-api/wsdl-api.png)
3. I den **WSDL-specifikation**, ange Webbadressen till där SOAP-API finns.
4. Klicka på **SOAP resten** knappen. När du klickar på det här alternativet försöker APIM göra en automatisk omvandling mellan XML och JSON. I det här fallet bör konsumenter anropar API: et som en restful-API som returnerar JSON. APIM konverterar varje begäran i ett SOAP-anrop.

    ![SOAP till REST](./media/restify-soap-api/soap-to-rest.png)

5. Tryck på TABB.

    Följande fält hämta fyllas med information från SOAP-API: visningsnamn, namn, beskrivning.
6. Lägga till en URL för API-suffix. Suffixet är ett namn som identifierar den här specifika API: et i den här APIM-instansen. Det måste vara unikt i den här APIM-instansen.
9. Publicera API: et genom att associera API: et med en produkt. I det här fallet den ”*obegränsad*” produkten används.  Om du vill för API som publiceras och vara tillgänglig att utvecklare kan lägga till en produkt. Du kan göra det under skapande av API eller Ställ in den senare.

    Produkter är kopplingar till en eller flera API: er. Du kan innehålla ett antal API: er och erbjuda utvecklare via developer-portalen. Utvecklare måste först prenumerera på en produkt kan få åtkomst till API: et. När man prenumererar få de en prenumeration nyckel som är bra för API: er i produkten. Om du har skapat APIM instans är du administratör redan, så du prenumererar på varje produkt som standard.

    Som standard medföljer två exempelprodukter varje API Management-instans:

    * **Starter**
    * **Obegränsat**   
10. Välj **Skapa**.

## <a name="test-the-new-apim-api-in-the-azure-portal"></a>Testa den nya APIM API i Azure-portalen

Åtgärder kan anropas direkt från Azure-portalen, vilket ger ett bekvämt sätt att visa och testa driften av ett API.  

1. Välj API som du skapade i föregående steg.
2. Tryck på den **Test** fliken.
3. Välj en åtgärd.

    Sidan visar för frågeparametrar och fält för sidhuvudena. Ett av huvudena är ”Ocp-Apim-prenumeration-Key”, för nyckeln prenumeration av produkten som är associerad med detta API. Om du har skapat APIM instans är du administratör redan så nyckeln fylls i automatiskt. 
1. Tryck på **skicka**.

    Backend svarar med **200 OK** och vissa data.

## <a name="call-operation"> </a>Anropa en åtgärd från utvecklarportalen

Åtgärder kan också kallas **utvecklarportalen** att testa API: er. 

1. Välj API som du skapade i den ”Import och publicera en backend-API” steg.
2. Tryck på **utvecklarportalen**.

    Webbplatsen ”Developer portal” öppnas.
3. Välj den **API** som du skapade.
4. Klicka på åtgärden som du vill testa.
5. Tryck på **prova**.
6. Tryck på **skicka**.
    
    När en åtgärd har anropats visas **svarsstatus**, **svarshuvuden** och eventuellt **svarsinnehåll** på utvecklarportalen.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [Transformera och skydda publicerade API](transform-api.md)