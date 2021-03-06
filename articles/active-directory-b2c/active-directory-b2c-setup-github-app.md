---
title: GitHub identitet providerkonfigurationen - Azure AD B2C | Microsoft Docs
description: Ange registrering och inloggning för kunder med GitHub-konton i dina program som skyddas av Azure Active Directory B2C.
services: active-directory-b2c
documentationcenter: ''
author: davidmu
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 02/06/2017
ms.author: davidmu
ms.openlocfilehash: 17ec4ebd8406f56c431666340ca8834c5ccf9670
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/23/2018
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-github-accounts"></a>Azure Active Directory B2C: Ange registrering och inloggning för konsumenter med GitHub-konton

> [!NOTE]
> Den här funktionen är i förhandsgranskningen.
> 

Den här artikeln visar hur du aktiverar inloggning för användare med GitHub-konto.

## <a name="create-a-github-oauth-application"></a>Skapa ett GitHub OAuth-program

Om du vill använda GitHub som en identitetsleverantör i Azure AD B2C måste du skapa en GitHub OAuth-app och ange rätt parametrar.

1. Gå till den [GitHub Developer inställningar](https://github.com/settings/developers) när du loggar in på GitHub.
1. Klicka på **ny OAuth-App**
1. Ange en **programnamn** och **webbsida URL**.
1. För den **auktorisering återanrop URL**, ange `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp`. Ersätt **{klient}** med din Azure AD B2C-klient namn (till exempel contosob2c.onmicrosoft.com).

    >[!NOTE]
    >Värdet för ”klient” måste vara gemener i den **inloggnings-URL**.

1. Klicka på **registrera program**.
1. Spara värdena för **klient-ID** och **Klienthemlighet**. Behöver du både i nästa avsnitt.

## <a name="configure-github-as-an-identity-provider-in-your-azure-ad-b2c-tenant"></a>Konfigurera GitHub som en identitetsleverantör i din Azure AD B2C-klient

1. Följ dessa steg för att [gå till B2C-funktionsbladet](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) på Azure-portalen.
1. Klicka på B2C-funktionsbladet **identitetsleverantörer**.
1. Klicka på **+Lägg till** överst på bladet.
1. Ange ett eget **namn** för providerkonfigurationen identitet. Till exempel ange ”GitHub”.
1. Klicka på **identitet providertyp**väljer **GitHub**, och klicka på **OK**.
1. Klicka på **ställa in den här identitetsleverantör** och ange klient-ID och klienthemlighet för GitHub OAuth-program som du kopierade tidigare.
1. Klicka på **OK** och klicka sedan på **skapa** att spara din GitHub-konfiguration.

## <a name="next-steps"></a>Nästa steg

Skapa eller redigera en [inbyggd princip](active-directory-b2c-reference-policies.md) och Lägg till GitHub som en identitetsleverantör.