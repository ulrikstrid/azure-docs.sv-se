---
title: "Användare som flaggats i säkerhetsrapporten i Azure Active Directory | Microsoft Docs"
description: "Lär dig mer om användare som flaggats i säkerhetsrapporten i Azure Active Directory-portalen"
services: active-directory
author: MarkusVi
manager: femila
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/24/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: ed6201e9edcef39b14b948b6b2f6e0b5da01ec60
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/05/2018
---
# <a name="remediate-users-flagged-for-risk-in-the-azure-active-directory-portal"></a>Åtgärda användare som flaggats för risk i Azure Active Directory-portalen

Med hjälp av säkerhetsrapporterna i Azure Active Directory (Azure AD) kan du bedöma risken för att användarkonton i din miljö har komprometterats. En [användare som har flaggats för risk](active-directory-identityprotection.md#users-flagged-for-risk) indikerar att ett användarkonto kan ha komprometterats.

Det är viktigt för Microsoft att hålla dina miljöer säkra. Som en del av detta åtagande övervakar Microsoft kontinuerligt för att hitta aktiviteter som är ovanliga eller som följer kända angreppsmönster. 


Om ovanliga aktiviteter identifieras som kan indikera obehörig åtkomst till några av dina användares konton får du meddelanden så att du kan vidta åtgärder. Att du får meddelanden behöver inte betyda att Microsofts egna system har komprometterats.
 

## <a name="azure-active-directory-report-access"></a>Åtkomst till Azure Active Directory-rapport

Du kan granska användare som har flaggats för risk med en Azure Active Directory-onlinerapport. Om du inte prenumererar på Azure kan du gå igenom prenumerationsprocessen kostnadsfritt på [http://aka.ms/AccessAAD](http://aka.ms/AccessAAD).  
När det är klart kan du använda dina Office 365-autentiseringsuppgifter för att få åtkomst till Azure Admin Center. Lägg märke till att mängden detaljer som tillhandahålls är begränsade på Basic-nivån. Ytterligare data och analyser är tillgängliga för Azure Premium-prenumeranter. Lär dig mer om [användare som flaggats i säkerhetsrapporten i Azure Active Directory-portalen](active-directory-reporting-security-user-at-risk.md).

När du har aktiverat din åtkomst till Azure AD omdirigeras du till [Azure AD-portalen](https://portal.azure.com). Om du vill gå direkt till rapporten går du till följande URL: [https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/UsersAtRisk](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/UsersAtRisk).

**Så här får du åtkomst till rapporter över användarna som har flaggats för risk i Office 365-administrationscentret:**

1.  På navigeringsmenyn till vänster klickar du på **Administrationscenter**. 
2.  Klicka på **Azure AD**.
3.  Logga in på **Azure Active Directory-administratörcentret**.
4.  Om en banderoll visas överst på sidan med texten **Check out the new portal** (Kolla in den nya portalen) klickar du på länken.
4.  På navigeringsmenyn till vänster klickar du på **Azure Active Directory**. 
5.  I navigeringsfönstret under **Säkerhet** klickar du på **Users flagged for risk** (Användare som har flaggats för risk).

Läs informationen som visas. Du bör återställa lösenordet för konton som anges här. 

## <a name="remediation-actions"></a>Åtgärder

Gör följande för att åtgärda de berörda kontona och göra miljön säker:

1.  [Validera](http://aka.ms/MFAValid) rätt information för multifaktorautentisering och lösenordsåterställning via självbetjäning. 
2.  [Aktivera](http://aka.ms/MFAuth) Multi-Factor Authentication för alla användare. 
3.  Om du använder det här [åtgärdsskriptet](http://aka.ms/remediate) för alla berörda konton kan du utföra följande steg automatiskt: 

    a. Återställa lösenord för att göra kontot säkert och avsluta aktiva sessioner.

    b. Ta bort postlådedelegater.

    c. Inaktivera vidarebefordran av e-post för externa domäner.

    d. Ta bort egenskapen för global vidarebefordran av e-post i brevlådan.

    e. Aktivera MFA för användarens konto.

    f. Ange att lösenordskomplexiteten för kontot ska vara högt.

    g. Aktivera granskning av postlådan.

    h. Producera spårningslogg som administratören ska granska.

4. Undersök din Office 365-klient och övrig IT-infrastruktur, däribland en granskning av alla klientinställningar, användarkonton och konfigurationsinställningar per användare för eventuell ändring. Leta efter indikatorer på beständighetsmetoder och på att en inkräktare kan ha skapat ett första fäste för att få tag i VPN-autentiseringsuppgifter eller få åtkomst till andra resurser i organisationen. 

5.  Överväg om du bör eller måste meddela myndigheter, däribland polisen och rättsväsendet.

Dessutom bör du:

- Läsa och implementera den här [vägledningen](http://aka.ms/fixaccount) om att uppmärksamma ovanliga aktiviteter. 
- [Aktivera granskningspipelinen](http://aka.ms/improvesecurity) för att analysera klientorganisationens aktivitet. När det är klar börjar granskningsarkivet fyllas i med alla aktivitetsloggar. Du kan då även använda [Security and Compliance Centers sökning och undersökning](http://aka.ms/sccsearch). 
- Använd det här [skriptet](http://aka.ms/mailboxaudit1) för att aktivera granskning av postlådan för alla konton. 
- Granska reglerna för delegeringsbehörigheter och vidarebefordran av e-post för alla brevlådor. Du kan använda det här [PowerShell-skriptet](http://aka.ms/delegateforwardrules) för at utföra den här uppgiften. 



## <a name="next-steps"></a>Nästa steg

- Mer information om Azure Active Directory Identity Protection finns i [Azure Active Directory Identity Protection](active-directory-identityprotection.md).

