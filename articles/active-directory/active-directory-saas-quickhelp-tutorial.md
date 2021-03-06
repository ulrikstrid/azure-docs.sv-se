---
title: 'Självstudier: Azure Active Directory-integrering med QuickHelp | Microsoft Docs'
description: Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och QuickHelp.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 655c9ad3-2076-4e2c-8e47-9ed3bf04be56
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2017
ms.author: jeedes
ms.openlocfilehash: feb51a61ebe67f583b47ad516ddb40cd5b84b905
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/04/2018
---
# <a name="tutorial-azure-active-directory-integration-with-quickhelp"></a>Självstudier: Azure Active Directory-integrering med QuickHelp

I kursen får lära du att integrera QuickHelp med Azure Active Directory (AD Azure).

Integrera QuickHelp med Azure AD ger dig följande fördelar:

- Du kan styra i Azure AD som har åtkomst till QuickHelp
- Du kan aktivera användarna att automatiskt hämta loggat in på QuickHelp (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - Azure-portalen

Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Förutsättningar

För att konfigurera Azure AD-integrering med QuickHelp, behöver du följande:

- En Azure AD-prenumeration
- En QuickHelp enkel inloggning aktiverad prenumeration

> [!NOTE]
> Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.

Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till QuickHelp från galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-quickhelp-from-the-gallery"></a>Att lägga till QuickHelp från galleriet
Du måste lägga till QuickHelp från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av QuickHelp i Azure AD.

**Utför följande steg för att lägga till QuickHelp från galleriet:**

1. I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Gå till **företagsprogram**. Gå till **alla program**.

    ![Program][2]
    
3. Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.

    ![Program][3]

4. I sökrutan skriver **QuickHelp**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_search.png)

5. Välj i resultatpanelen **QuickHelp**, och klicka sedan på **Lägg till** för att lägga till programmet.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med QuickHelp baserat på en testanvändare som kallas ”Britta Simon”.

Azure AD måste du känna till användaren i QuickHelp motsvarighet till en användare i Azure AD för enkel inloggning ska fungera. Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i QuickHelp upprättas.

I QuickHelp, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.

Om du vill konfigurera och testa Azure AD enkel inloggning med QuickHelp, måste du utföra följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare QuickHelp](#creating-a-quickhelp-test-user)**  – du har en motsvarighet för Britta Simon i QuickHelp som är kopplad till Azure AD-representation av användaren.
4. **[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt QuickHelp program.

**Utför följande steg för att konfigurera Azure AD enkel inloggning med QuickHelp:**

1. I Azure-portalen på den **QuickHelp** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_samlbase.png)

3. På den **QuickHelp domän och URL: er** avsnittet, utför följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_url.png)

    a. I den **inloggnings-URL** textruta Skriv en URL med följande mönster: `https://quickhelp.com/<ROUTEURL>`

    b. I den **identifierare** textruta, ange ett URL-Adressen: `https://auth.quickhelp.com`

    > [!NOTE] 
    > Inloggnings-URL-värdet är inte verkliga. Uppdatera värdet med det faktiska inloggnings-URL. Kontakta din organisations QuickHelp eller din kreativitet Client lyckade Manager för att hämta värdet.
 
4. På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-quickhelp-tutorial/tutorial_general_400.png) 

6. Inloggning på webbplatsen QuickHelp företag som administratör.

7. Klicka på menyn högst upp **Admin**.
   
    ![Konfigurera enkel inloggning][21]

8. I den **QuickHelp Admin** -menyn klickar du på **inställningar**.
   
    ![Konfigurera enkel inloggning][22]

9. Klicka på **autentiseringsinställningar**.

10. På den **autentiseringsinställningar** utför följande steg
   
    ![Konfigurera enkel inloggning][23]
   
    a. Som **SSO typen**väljer **WSFederation**.
   
    b. Om du vill överföra din hämtade Azure metadatafil klickar du på **Bläddra**, navigera till filen, sista Klicka **överföra Metadata**.
   
    c. I den **e-post** textruta typen `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.
   
    d. I den **Förnamn** textruta `type http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.
   
    e. I den **efternamn** textruta `type http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.
   
    f. I den **Åtgärdsfältet**, klickar du på **spara**.

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**Utför följande steg för att skapa en testanvändare i Azure AD:**

1. I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_01.png) 

2. Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_02.png) 

3. Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_03.png) 

4. På den **användaren** dialogrutan utför följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_04.png) 

    a. I den **namn** textruta typen **BrittaSimon**.

    b. I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och anteckna värdet för den **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-quickhelp-test-user"></a>Skapa en testanvändare QuickHelp

Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i QuickHelp.
För enkel inloggning ska fungera, måste Azure AD motsvarighet användaren i QuickHelp till en användare i Azure AD är okänt. Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i QuickHelp upprättas.

QuickHelp stöder just-in-time-etablering. Det innebär, om det behövs ett konto skapas automatiskt i QuickHelp och kontot som är kopplad till Azure AD-kontot.

Det finns ingen åtgärd objekt i det här avsnittet.

### <a name="assigning-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändare

I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till QuickHelp.

![Tilldela användare][200] 

**Om du vill tilldela QuickHelp Britta Simon utför du följande steg:**

1. Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program **QuickHelp**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_app.png) 

3. Klicka på menyn till vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** på listan användare.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.  

När du klickar på panelen QuickHelp på åtkomstpanelen du bör få automatiskt loggat in på ditt QuickHelp program.


## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_203.png
[21]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_05.png
[22]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_06.png
[23]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_07.png
