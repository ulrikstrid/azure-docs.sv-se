---
title: "Så här konfigurerar du ett nytt program för flera innehavare | Microsoft Docs"
description: "Hur du konfigurerar enkel inloggning för ett anpassat program som du utvecklar och registreras med Azure AD."
services: active-directory
documentationcenter: 
author: ajamess
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: c9654a9d03769bbf0e9d0e2c477ad26cefc05150
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-configure-a-new-multi-tenant-application"></a>Så här konfigurerar du ett nytt program för flera innehavare

Aktivera federerad enkel inloggning (SSO) i din app aktiveras automatiskt när federering via Azure AD för OpenID Connect, SAML 2.0 och WS-Fed. Om slutanvändarna har att logga in trots redan har en befintlig session med Azure AD, är det troligt att din app kan vara felkonfigurerad.

* Om du använder ADAL/MSAL, kontrollera att du har **PromptBehavior** inställd på **automatisk** snarare än **alltid**.

* Om du utvecklar en mobil app eventuellt ytterligare konfigurationer för att aktivera enkel inloggning asynkrona eller icke asynkron.

För Android, se [aktivera Cross App SSO i Android](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-android).<br>

För iOS, se [aktivera Cross App SSO i iOS](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios).

## <a name="next-steps"></a>Nästa steg

[Azure AD för enkel inloggning](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)<br>

[Aktivera mellan appen SSO i Android](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-android)<br>

[Aktivera Cross App SSO i iOS](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios)<br>

[Integrera appar till AzureAD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)<br>

[Medgivande och Permissioning för AzureAD v2.0 konvergerat appar](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[AzureAD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory)
