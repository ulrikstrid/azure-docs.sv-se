---
title: 'Självstudier: Azure Active Directory-integrering med efterlevnad Tomtenisse | Microsoft Docs'
description: Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och kompatibilitet Tomtenisse.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 69c6efc3-54c7-49ec-b827-33177c09aa13
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/16/2018
ms.author: jeedes
ms.openlocfilehash: 32cb676393558079baad9bb0fc26d2c4daaa3519
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/19/2018
---
# <a name="tutorial-azure-active-directory-integration-with-compliance-elf"></a>Självstudier: Azure Active Directory-integrering med efterlevnad Tomtenisse

I kursen får lära du att integrera kompatibilitet Tomtenisse med Azure Active Directory (AD Azure).

Integrera Tomtenisse kompatibilitet med Azure AD ger dig följande fördelar:

- Du kan styra i Azure AD som har åtkomst till kompatibilitet Tomtenisse.
- Du kan aktivera användarna att automatiskt hämta loggat in på efterlevnad Tomtenisse (Single Sign-On) med sina Azure AD-konton.
- Du kan hantera dina konton i en central plats - Azure-portalen.

Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Förutsättningar

Om du vill konfigurera Azure AD-integrering med efterlevnad Tomtenisse behöver du följande:

- En Azure AD-prenumeration
- En kompatibilitet Tomtenisse enkel inloggning aktiverad prenumeration

> [!NOTE]
> Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.

Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till kompatibilitet Tomtenisse från galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-compliance-elf-from-the-gallery"></a>Att lägga till kompatibilitet Tomtenisse från galleriet
Du måste lägga till kompatibilitet Tomtenisse från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av kompatibilitet Tomtenisse i Azure AD.

**Utför följande steg för att lägga till kompatibilitet Tomtenisse från galleriet:**

1. I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon. 

    ![Azure Active Directory-knappen][1]

2. Gå till **företagsprogram**. Gå till **alla program**.

    ![Bladet Enterprise program][2]
    
3. Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.

    ![Knappen Nytt program][3]

4. I sökrutan skriver **kompatibilitet Tomtenisse**väljer **kompatibilitet Tomtenisse** resultatet-panelen klickar **Lägg till** för att lägga till programmet.

    ![Efterlevnad Tomtenisse i resultatlistan](./media/active-directory-saas-complianceelf-tutorial/tutorial_complianceelf_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med efterlevnad Tomtenisse baserat på en testanvändare som kallas ”Britta Simon”.

Azure AD måste du känna till motsvarande användaren i kompatibilitet Tomtenisse till en användare i Azure AD för enkel inloggning ska fungera. Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i kompatibilitet Tomtenisse upprättas.

I kompatibilitet Tomtenisse tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.

Om du vill konfigurera och testa Azure AD enkel inloggning med efterlevnad Tomtenisse, måste du utföra följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare kompatibilitet Tomtenisse](#create-a-compliance-elf-test-user)**  – har en motsvarighet för Britta Simon kompatibilitet Tomtenisse som är kopplad till Azure AD-representation av användaren.
4. **[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt kompatibilitet Tomtenisse program.

**Utför följande steg för att konfigurera Azure AD enkel inloggning med efterlevnad Tomtenisse:**

1. I Azure-portalen på den **kompatibilitet Tomtenisse** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning länk][4]

2. På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.

    ![Enkel inloggning dialogrutan](./media/active-directory-saas-complianceelf-tutorial/tutorial_complianceelf_samlbase.png)

3. På den **kompatibilitet Tomtenisse domän och URL: er** avsnittet, utför följande steg om du vill konfigurera programmet i **IDP** initierade läge:

    ![URL: er och kompatibilitet Tomtenisse domän med enkel inloggning information](./media/active-directory-saas-complianceelf-tutorial/tutorial_complianceelf_url.png)

    I den **identifierare** textruta Skriv en URL som: `https://sso.cordium.com`

4. Kontrollera **visa avancerade inställningar för URL: en** och utför följande steg om du vill konfigurera programmet i **SP** initierade läge:

    ![URL: er och kompatibilitet Tomtenisse domän enkel inloggning](./media/active-directory-saas-complianceelf-tutorial/tutorial_complianceelf_url1.png)

    I den **inloggnings-URL** textruta Skriv en URL med följande mönster: `https://<subdomain>.complianceelf.com`
    
    > [!NOTE] 
    > Det här värdet är inte verkliga. Uppdatera den här värden med den faktiska inloggnings-URL. Kontakta [kompatibilitet Tomtenisse supportteamet](mailto:support@complianceelf.com) att hämta det här värdet.

5. På den **SAML-signeringscertifikat** klickar du på kopieringsknappen för att kopiera **webbadress Federation Metadata** och klistra in den i anteckningar.
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-complianceelf-tutorial/tutorial_metadataurl.png)
     
6. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-complianceelf-tutorial/tutorial_general_400.png)

7. Konfigurera enkel inloggning på **kompatibilitet Tomtenisse** sida, måste du skicka den **webbadress Federation Metadata** till [kompatibilitet Tomtenisse supportteamet](mailto:support@complianceelf.com). De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD

Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.

   ![Skapa en testanvändare i Azure AD][100]

**Utför följande steg för att skapa en testanvändare i Azure AD:**

1. I Azure-portalen i den vänstra rutan klickar du på den **Azure Active Directory** knappen.

    ![Azure Active Directory-knappen](./media/active-directory-saas-complianceelf-tutorial/create_aaduser_01.png)

2. Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.

    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-complianceelf-tutorial/create_aaduser_02.png)

3. Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.

    ![Knappen Lägg till](./media/active-directory-saas-complianceelf-tutorial/create_aaduser_03.png)

4. I den **användaren** dialogrutan utför följande steg:

    ![Dialogrutan användare](./media/active-directory-saas-complianceelf-tutorial/create_aaduser_04.png)

    a. I den **namn** skriver **BrittaSimon**.

    b. I den **användarnamn** Skriv användarens Britta Simon e-postadress.

    c. Välj den **visa lösenordet** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** rutan.

    d. Klicka på **Skapa**.
  
### <a name="create-a-compliance-elf-test-user"></a>Skapa en testanvändare kompatibilitet Tomtenisse

I det här avsnittet kan du skapa en användare som kallas Britta Simon i kompatibilitet Tomtenisse. Arbeta med [kompatibilitet Tomtenisse supportteamet](mailto:support@complianceelf.com) att lägga till användare i kompatibilitet Tomtenisse-plattformen. Användare måste skapas och aktiveras innan du använder enkel inloggning.

### <a name="assign-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändare

I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till kompatibilitet Tomtenisse.

![Tilldela rollen][200] 

**Om du vill tilldela kompatibilitet Tomtenisse Britta Simon utför du följande steg:**

1. Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program **kompatibilitet Tomtenisse**.

    ![Länken kompatibilitet Tomtenisse i listan med program](./media/active-directory-saas-complianceelf-tutorial/tutorial_complianceelf_app.png)  

3. Klicka på menyn till vänster **användare och grupper**.

    ![Länken ”användare och grupper”][202]

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Fönstret Lägg till tilldelning][203]

5. På **användare och grupper** markerar **Britta Simon** på listan användare.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.

När du klickar på panelen kompatibilitet Tomtenisse på åtkomstpanelen du bör få automatiskt loggat in på ditt kompatibilitet Tomtenisse program.
Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-complianceelf-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-complianceelf-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-complianceelf-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-complianceelf-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-complianceelf-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-complianceelf-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-complianceelf-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-complianceelf-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-complianceelf-tutorial/tutorial_general_203.png

