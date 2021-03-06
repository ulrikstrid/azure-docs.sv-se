---
title: 'Azure Mobile Engagement felsökningsguide för - API: er'
description: 'Felsökning för Azure Mobile Engagement - API: er'
services: mobile-engagement
documentationcenter: ''
author: piyushjo
manager: erikre
editor: ''
ms.assetid: 3efc8a52-2b74-4917-b887-815ae8277474
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 10/04/2016
ms.author: piyushjo
ms.openlocfilehash: 8d51aaf5196eabe18d92b34d97e98f9f665f2527
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2018
---
# <a name="troubleshooting-guide-for-api-issues"></a>Felsökningsguide för API-problem
> [!IMPORTANT]
> Azure Mobile Engagement dras tillbaka den 31 mars 2018. Den här sidan tas bort strax efter det.
> 

Följande är möjliga problem som kan uppstå med hur administratörer interagerar med Azure Mobile Engagement via API: erna.

## <a name="syntax-issues"></a>Syntaxen problem
### <a name="issue"></a>Problem
* Syntaxfel med hjälp av API (eller oväntade resultat).

### <a name="causes"></a>Orsaker
* Problem med syntax:
  * Se till att kontrollera syntaxen för det specifika API: et som du använder för att bekräfta att alternativet är tillgängligt.
  * Ett vanligt problem med API-användning är försvåra Reach-API och Push-API (de flesta uppgifter ska utföras med nå API i stället för Push-API). 
  * Ett annat vanliga problem med SDK-integration och API-användning är försvåra SDK-nyckeln och API-nyckeln.
  * Skript som ansluter till API: erna måste skicka data minst var 10: e minut eller anslutningen kommer (särskilt vanligt i övervakaren API-skript som lyssnar efter data). För att förhindra timeout har skriptet skicka en XMPP ping var 10: e minut och hålla sessionen aktiv med servern.

### <a name="see-also"></a>Se också
* [API-dokumentationen][Link 4]
* [XMPP protokollet Info](http://xmpp.org/extensions/xep-0199.html)

## <a name="unable-to-use-the-api-to-perform-the-same-action-available-in-the-azure-mobile-engagement-ui"></a>Det går inte att använda API: et för att utföra samma åtgärd som är tillgängliga i Användargränssnittet för Azure Mobile Engagement
### <a name="issue"></a>Problem
* En åtgärd som fungerar från Azure Mobile Engagement-Gränssnittet fungerar inte från relaterade Azure Mobile Engagement-API.

### <a name="causes"></a>Orsaker
* Bekräfta att du kan utföra samma åtgärd från Gränssnittet för Azure Mobile Engagement visar att den här funktionen i Azure Mobile Engagement korrekt har integrerats med SDK.

### <a name="see-also"></a>Se också
* [UI-dokumentation][Link 1]

## <a name="error-messages"></a>Felmeddelanden
### <a name="issue"></a>Problem
* Felkoder med API: T som visas vid körning eller i loggarna.

### <a name="causes"></a>Orsaker
* Här är en sammansatt lista över vanliga API status koder för referens och preliminär Felsökning:
  
        200        Success.
        200        Account updated: device registered, associated, updated, or removed from the current account.
        200        Returns a list of projects as a JSON object or an authentication token generated and returned in the response’s body.
        201        Account created.
        400        Invalid parameter or validation exception (check payload for details). The parameters provided to the API or service are invalid. In this case, the HTTP response will embed more details. Make sure to test for the MIME type of the response as the payload can either be plain text or a JSON object.
        401        Authentication error. No user is currently authenticated or connected (check the AppID and SDK key).
        402        Billing lock. The application is either off its quotas or is currently in a bad billing state.
        403        The application is not enabled or the specific API is disabled for this application.
        403        Unauthorized access to the project or application, invalid access key (the key must match the one provided when created).
        403        Campaign specific errors: campaign must be finished (or has already been activated), the suspend action can only be performed on an scheduled campaign, cannot finish a campaign that is not currently “in progress”, campaign must be “in progress” and the campaign’s property named, manual Push must be set to true.
        403        The email address is already associated to another account (a super user for instance). No authentication token will be generated.
        404        Application, device, campaign, or project identifier not found.
        404        Query parameter is invalid JSON or has a field with an unexpected value.
        404        The email address is not associated with an account. Please create or update the account first.
        405        Invalid HTTP method (GET, POST, etc.) or trying to edit a read only segment (i.e. add or update or delete a criterion). A segment becomes read only after it has been computed for the first time.
        409        Name already associated to a different device ID or campaign.
        413        Too many device identifiers (current limit is 1,000), POST URL encoded entity is over 2MB, or the period is too large to be displayed (the server didn’t retrieve the analytics because the user request is for a period that is too large).
        503        Analytics not available yet (the requested information is not computed yet for an application).
        504        The server was not able to handle your request in a reasonable time (if you make multiple calls to an API very quickly, try to make one call at a time and spread the calls out over time).

### <a name="see-also"></a>Se också
* [API-dokumentationen – för detaljerade fel på varje specifika API: et][Link 4]

## <a name="silent-failures"></a>Tyst fel
### <a name="issue"></a>Problem
* API-åtgärd misslyckas inget felmeddelande visas vid körning eller i loggarna.

### <a name="causes"></a>Orsaker
* Många objekt som kommer kommer att inaktiveras i Användargränssnittet för Azure Mobile Engagement om de inte är integrerade på rätt sätt, men misslyckas automatiskt från API: et, så Kom ihåg att testa samma funktion från Gränssnittet för att se om det fungerar.
* Azure Mobile Engagement och många avancerade funktioner i Azure Mobile Engagement som du försöker använda, måste integreras individuellt i din app med SDK som separata steg innan du kan använda dem.

### <a name="see-also"></a>Se också
* [Felsökningsguide för - SDK][Link 25]

<!--Link references-->
[Link 1]: mobile-engagement-user-interface-home.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md

