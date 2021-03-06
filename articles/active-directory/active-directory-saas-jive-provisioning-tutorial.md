---
title: "Självstudier: Konfigurera Jive för automatisk användaretablering med Azure Active Directory | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Jive."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 6fbfdbe7-d66c-4305-9fea-76d6a6a92830
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: 4190b1cbc7e32b1471795e29a211f7a7e5831648
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 02/09/2018
---
# <a name="tutorial-configure-jive-for-automatic-user-provisioning"></a>Självstudier: Konfigurera Jive för automatisk användaretablering

Syftet med den här kursen är att visa de steg som du behöver göra i Jive och Azure AD till automatiskt etablera och avinstallation etablera användarkonton från Azure AD till Jive.

## <a name="prerequisites"></a>Förutsättningar

Det scenario som beskrivs i den här kursen förutsätter att du redan har följande objekt:

*   En Azure Active directory-klient.
*   En Jive enkel inloggning för aktiverade prenumerationen.
*   Ett användarkonto i Jive Team administratörsbehörigheter.

## <a name="assigning-users-to-jive"></a>Tilldela användare till Jive

Azure Active Directory använder ett begrepp som kallas ”tilldelningar” för att avgöra vilka användare ska få åtkomst till valda appar. I samband med automatisk konto användaretablering, synkroniseras de användare och grupper som har ”tilldelats” till ett program i Azure AD.

Innan du konfigurerar och aktiverar tjänsten etablering, måste du bestämma vilka användare och/eller grupper i Azure AD representerar de användare som behöver åtkomst till appen Jive. När bestämt, kan du tilldela dessa användare i appen Jive genom att följa anvisningarna här:

[Tilldela en användare eller grupp till en enterprise-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-jive"></a>Viktiga tips för att tilldela användare till Jive

*   Vi rekommenderar att en enda Azure AD-användare tilldelas Jive att testa allokering konfigurationen. Ytterligare användare och/eller grupper kan tilldelas senare.

*   När du tilldelar en användare Jive, måste du välja en giltig användarroll. Rollen ”standard åtkomst” fungerar inte för etablering.

## <a name="enable-user-provisioning"></a>Aktivera etablering av användare

Det här avsnittet hjälper dig att ansluta din Azure AD till Jives användarkonto API-etablering och konfigurera tjänsten etablering för att skapa, uppdatera och inaktivera tilldelade användarkonton i Jive baserat på tilldelning av användare och grupper i Azure AD.

> [!TIP]
> Du kan också välja att aktivera SAML-baserade enkel inloggning för Jive, följer du instruktionerna som anges i [Azure-portalen](https://portal.azure.com). Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.

### <a name="to-configure-user-account-provisioning"></a>Konfigurera användaretablering för kontot:

Syftet med det här avsnittet är att beskriva hur du aktiverar användaretablering för Active Directory-användarkonton till Jive.
Som en del av den här proceduren måste måste du tillhandahålla en säkerhetstoken för användare som behöver begära från Jive.com.

1. I den [Azure-portalen](https://portal.azure.com), bläddra till den **Azure Active Directory > Företagsappar > alla program** avsnitt.

2. Om du redan har konfigurerat Jive för enkel inloggning, söka efter din instans av Jive med hjälp av sökfältet. Annars väljer **Lägg till** och Sök efter **Jive** i programgalleriet. Välj Jive i sökresultatet och lägga till den i listan med program.

3. Välj din instans av Jive och sedan den **etablering** fliken.

4. Ange den **Etableringsläge** till **automatisk**. 

    ![etablering](./media/active-directory-saas-jive-provisioning-tutorial/provisioning.png)

5. Under den **administratörsautentiseringsuppgifter** och ange följande inställningar:
   
    a. I den **Jive administratörsanvändarnamnet** textruta typen en Jive kontonamn som har den **systemadministratören** profil i Jive.com som tilldelats.
   
    b. I den **Jive adminlösenord** textruta skriver du lösenordet för det här kontot.
   
    c. I den **Jive klient URL** textruta anger Jive klient-URL.
      
      > [!NOTE]
      > Jive klient-URL är URL som används av din organisation för att logga in på Jive.  
      > Normalt URL har följande format: **www.\< organisation\>. jive.com**.          

6. I Azure-portalen klickar du på **Testanslutningen** så Azure AD kan ansluta till din Jive app.

7. Ange e-postadressen för en person eller grupp som ska få meddelanden om etablering fel i den **e-postmeddelande** fält och markera kryssrutan nedan.

8. Klicka på **spara.**

9. Välj under avsnittet mappningar **synkronisera Azure Active Directory-användare Jive.**

10. I den **attributmappning** avsnittet kan du granska användarattribut som synkroniseras från Azure AD till Jive. De attribut som valts som **matchande** egenskaper som används för att matcha användarkonton i Jive för uppdateringsåtgärder. Välj knappen Spara för att genomföra ändringarna.

11. Om du vill aktivera Azure AD-tjänsten för Jive-etablering, ändra den **Status för etablering** till **på** i avsnittet Inställningar

12. Klicka på **spara.**

Startar den första synkroniseringen av användare och/eller grupper som tilldelas till Jive i avsnittet användare och grupper. Den första synkroniseringen tar längre tid än efterföljande synkroniseringar som sker ungefär var 40 minuter så länge som tjänsten körs. Du kan använda den **synkroniseringsinformation** avsnittet för att övervaka förloppet och följ länkarna till att etablera aktivitetsloggar som beskriver alla åtgärder som utförs av tjänsten etablering Jive appen.

Mer information om hur du tolkar Azure AD-etablering loggar finns [rapportering om automatisk konto användaretablering](active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Hantera användare konto-etablering för företag-appar](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurera enkel inloggning](active-directory-saas-jive-tutorial.md)