---
title: Självstudiekurs om MongoDB, Angular och Node för Azure – del 3 | Microsoft Docs
description: Del 3 i självstudieserien om hur du skapar en MongoDB-app med Angular och Node i Azure Cosmos DB med exakt samma API:er som du använder för MongoDB.
services: cosmos-db
documentationcenter: ''
author: SnehaGunda
manager: kfile
editor: ''
ms.assetid: ''
ms.service: cosmos-db
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 09/05/2017
ms.author: sngun
ms.custom: mvc
ms.openlocfilehash: de645f46a889ba05fc54b1c5d2b9da64393d348e
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/16/2018
---
# <a name="create-a-mongodb-app-with-angular-and-azure-cosmos-db---part-3-build-the-ui-with-angular"></a>Skapa en MongoDB-app med Angular och Azure Cosmos DB – del 3: Utveckla användargränssnittet med Angular

Den här självstudiekursen i flera delar demonstrerar hur du skapar en ny [MongoDB API](mongodb-introduction.md)-app skriven i Node.js med Express och Angular och sedan kopplar den till din Azure Cosmos DB-databas.

Del 3 av självstudiekursen bygger vidare på [del 2](tutorial-develop-mongodb-nodejs-part2.md) och består av följande uppgifter:

> [!div class="checklist"]
> * Skapa Angular-användargränssnittet
> * Skapa rätt känsla och design med CSS
> * Testa appen lokalt

## <a name="video-walkthrough"></a>Videogenomgång

> [!VIDEO https://www.youtube.com/embed/MnxHuqcJVoM]

## <a name="prerequisites"></a>Nödvändiga komponenter

Utför stegen i [del 2](tutorial-develop-mongodb-nodejs-part2.md) av självstudiekursen innan du påbörjar den här delen.

> [!TIP]
> Den här självstudiekursen beskriver steg för steg hur du skapar programmet. Om du vill hämta det färdiga projektet kan du ladda ned programmet från [angular-cosmosdb-databasen](https://github.com/Azure-Samples/angular-cosmosdb) på GitHub.

## <a name="build-the-ui"></a>Skapa användargränssnittet

1. Klicka på stoppknappen i Visual Studio Code ![Stoppknapp i Visual Studio Code](./media/tutorial-develop-mongodb-nodejs-part3/stop-button.png) för att stoppa Node-appen.

2. Generera en heroes-komponent genom att skriva följande kommando i kommandotolken i Windows eller i terminalfönstret på en Mac. I den här koden är g = generera, c = komponent och heroes = namnet på komponenten. Koden har en platt filstruktur (--flat) så att ingen undermapp skapas för den.

    ```
    ng g c heroes --flat 
    ```

    En bekräftelse av de nya komponenterna visas i terminalfönstret.

    ```bash
    installing component
      create src\client\app\heroes.component.ts
      update src\client\app\app.module.ts 
    ```

    Nu ska vi titta på filerna som skapats och uppdaterats. 

3. Gå till den nya **src\client\app**-mappen i **Explorer**-fönstret i Visual Studio Code och öppna den nya **heroes.component.ts**-filen som skapades i steg 2. Den här TypeScript-komponentfilen skapades med det föregående kommandot.

    > [!TIP]
    > Om mappen inte visas i Visual Studio Code trycker du på CMD + SKIFT P på en Mac-dator eller på CTRL + SKIFT + P i Windows för att öppna kommandopaletten och hämtar sedan systemändringen genom att skriva *Reload Window*.

    ![Öppna filen heroes.component.ts](./media/tutorial-develop-mongodb-nodejs-part3/open-folder.png)

4. Öppna filen **app.module.ts** i samma mapp och notera att `HeroesComponent` har lagts till i deklarationerna på rad 5 och att den även har importerats på rad 10.

    ![Öppna filen app-module.ts](./media/tutorial-develop-mongodb-nodejs-part3/app-module-file.png)

    Nu när heroes-komponenten är klar skapar du en ny fil för heroes-komponentens HTML-kod. Eftersom vi skapade en minimal app ville vi lägga HTML-koden i samma fil som TypeScript-filen, men nu ska vi extrahera den och skapa en separat fil.

5. Högerklicka på mappen **app** i **Explorer**-fönstret, klicka på **Ny fil** och ge den nya filen namnet *heroes.component.html*.

6. Ta bort raderna 5 till och med 9 i filen **heroes.component.ts** 

    ```ts
    template: `
        <p>
          heroes Works!
        </p>
      `,
      ```
      och ersätt dem med
  
    ```ts
    templateUrl: './heroes.component.html',
    ```

    för att referera till den nya HTML-filen.
 
    > [!TIP]
    > Du kan använda John Papas Angular Essentials-tillägg och -kodfragment för Visual Studio Code för att påskynda distributionen. 
    > 1. Klicka på knappen **Tillägg** ![Knappen Tillägg i Visual Studio Code](./media/tutorial-develop-mongodb-nodejs-part3/extensions-button.png).
    > 2. Skriv *angular essentials* i sökrutan.
    > 3. Klicka på **Installera**. 
    > 4. Klicka på knappen **Läs in igen** för att använda de nya tilläggen,
    > eller ladda ned från [http://jpapa.me/angularessentials](http://jpapa.me/angularessentials). 
    > ![Angular Essentials-tillägg](./media/tutorial-develop-mongodb-nodejs-part3/angular-essentials-extension.png)

7. Gå tillbaka till filen **heroes.component.html** och kopiera in den här koden. `<div>` är behållaren för hela sidan. Behållaren innehåller en lista med heroes-komponenter som vi måste skapa, så att du när du klickar på en komponent kan markera den och redigera eller ta bort den i användargränssnittet. I HTML-koden använder vi en del formatering så att du vet vilken komponent som har markerats. Det finns också ett redigeringsområde så att du kan lägga till en ny hero-komponent eller redigera en befintlig. 

    ```html
    <div>
      <ul class="heroes">
        <li *ngFor="let hero of heroes" (click)="onSelect(hero)" [class.selected]="hero === selectedHero">
          <button class="delete-button" (click)="deleteHero(hero)">Delete</button>
          <div class="hero-element">
            <div class="badge">{{hero.id}}</div>
            <div class="name">{{hero.name}}</div>
            <div class="saying">{{hero.saying}}</div>
          </div>
        </li>
      </ul>
      <div class="editarea">
        <button (click)="enableAddMode()">Add New Hero</button>
        <div *ngIf="selectedHero">
          <div class="editfields">
            <div>
              <label>id: </label>
              <input [(ngModel)]="selectedHero.id" placeholder="id" *ngIf="addingHero" />
              <label *ngIf="!addingHero" class="value">{{selectedHero.id}}</label>
            </div>
            <div>
              <label>name: </label>
              <input [(ngModel)]="selectedHero.name" placeholder="name" />
            </div>
            <div>
              <label>saying: </label>
              <input [(ngModel)]="selectedHero.saying" placeholder="saying" />
            </div>
          </div>
          <button (click)="cancel()">Cancel</button>
          <button (click)="save()">Save</button>
        </div>
      </div>
    </div>
    ```

8. Nu när HTML-koden är på plats måste vi lägga till den i **heroes.component.ts**-filen så att vi kan interagera med mallen. Den nya koden i **heroes.component.ts**-filen nedan lägger till mallen i komponentfilen. En konstruktor har lagts till som hämtar några heroes-komponenter och initierar hero-tjänstkomponenten för att hämta alla data. Den här koden lägger också till alla nödvändiga metoder för hanteringen av händelser i användargränssnittet. Du kan kopiera följande kod över den befintliga koden i **heroes.component.ts**. 

    ```ts
    import { Component, OnInit } from '@angular/core';

    @Component({
      selector: 'app-heroes',
      templateUrl: './heroes.component.html'
    })
    export class HeroesComponent implements OnInit {
      addingHero = false;
      heroes: any = [];
      selectedHero: Hero;
    
      constructor(private heroService: HeroService) {}
    
      ngOnInit() {
       this.getHeroes();
      }

      cancel() {
        this.addingHero = false;
        this.selectedHero = null;
      }

      deleteHero(hero: Hero) {
        this.heroService.deleteHero(hero).subscribe(res => {
          this.heroes = this.heroes.filter(h => h !== hero);
          if (this.selectedHero === hero) {
            this.selectedHero = null;
          }
        });
      }

      getHeroes() {
        return this.heroService.getHeroes().subscribe(heroes => {
          this.heroes = heroes;
        });
      }

      enableAddMode() {
        this.addingHero = true;
        this.selectedHero = new Hero();
      }

      onSelect(hero: Hero) {
        this.addingHero = false;
        this.selectedHero = hero;
      }

      save() {
        if (this.addingHero) {
          this.heroService.addHero(this.selectedHero).subscribe(hero => {
            this.addingHero = false;
            this.selectedHero = null;
            this.heroes.push(hero);
          });
        } else {
          this.heroService.updateHero(this.selectedHero).subscribe(hero => {
            this.addingHero = false;
            this.selectedHero = null;
          });
        }
      }
    }
    ```

9. Öppna filen **app/app.module.ts** i **Explorer** och uppdatera raderna 13 (lägg till ett kommatecken) och 14 för att lägga till en import för en `FormsModule`. Importavsnittet bör se ut så här nu:

    ```
    imports: [
      BrowserModule,
      FormsModule
    ],
    ```

10. Lägg till en import för den nya FormsModule-modulen på rad 3. 

    ```
    import { BrowserModule } from '@angular/platform-browser';
    import { NgModule } from '@angular/core';
    import { FormsModule } from '@angular/forms';
    ```

## <a name="use-css-to-set-the-look-and-feel"></a>Skapa rätt känsla och design med CSS

1. Öppna filen **src/client/styles.scss** i Explorer-fönstret.

2. Kopiera följande kod till filen **styles.scss** genom att ersätta det befintliga innehållet i filen.

    ```css
    /* You can add global styles to this file, and also import other style files */

    * {
      font-family: Arial;
    }
    h2 {
      color: #444;
      font-weight: lighter;
    }
    body {
      margin: 2em;
    }
    
    body,
    input[text],
    button {
      color: #888;
      // font-family: Cambria, Georgia;
    }
    button {
      font-size: 14px;
      font-family: Arial;
      background-color: #eee;
      border: none;
      padding: 5px 10px;
      border-radius: 4px;
      cursor: pointer;
      cursor: hand;
      &:hover {
        background-color: #cfd8dc;
      }
      &.delete-button {
        float: right;
        background-color: gray !important;
        background-color: rgb(216, 59, 1) !important;
        color: white;
        padding: 4px;
        position: relative;
        font-size: 12px;
      }
    }
    div {
      margin: .1em;
    }

    .selected {
      background-color: #cfd8dc !important;
      background-color: rgb(0, 120, 215) !important;
      color: white;
    }

    .heroes {
      float: left;
      margin: 0 0 2em 0;
      list-style-type: none;
      padding: 0;
      li {
        cursor: pointer;
        position: relative;
        left: 0;
        background-color: #eee;
        margin: .5em;
        padding: .5em;
        height: 3.0em;
        border-radius: 4px;
        width: 17em;
        &:hover {
          color: #607d8b;
          color: rgb(0, 120, 215);
          background-color: #ddd;
          left: .1em;
        }
        &.selected:hover {
          /*background-color: #BBD8DC !important;*/
          color: white;
        }
      }
      .text {
        position: relative;
        top: -3px;
      }
      .saying {
        margin: 5px 0;
      }
      .name {
        font-weight: bold;
      }
      .badge {
        /* display: inline-block; */
        float: left;
        font-size: small;
        color: white;
        padding: 0.7em 0.7em 0 0.5em;
        background-color: #607d8b;
        background-color: rgb(0, 120, 215);
        background-color:rgb(134, 183, 221);
        line-height: 1em;
        position: relative;
        left: -1px;
        top: -4px;
        height: 3.0em;
        margin-right: .8em;
        border-radius: 4px 0 0 4px;
        width: 1.2em;
      }
    }

    .header-bar {
      background-color: rgb(0, 120, 215);
      height: 4px;
      margin-top: 10px;
      margin-bottom: 10px;
    }

    label {
      display: inline-block;
      width: 4em;
      margin: .5em 0;
      color: #888;
      &.value {
        margin-left: 10px;
        font-size: 14px;
      }
    }

    input {
      height: 2em;
      font-size: 1em;
      padding-left: .4em;
      &::placeholder {
          color: lightgray;
          font-weight: normal;
          font-size: 12px;
          letter-spacing: 3px;
      }
    }

    .editarea {
      float: left;
      input {
        margin: 4px;
        height: 20px;
        color: rgb(0, 120, 215);
      }
      button {
        margin: 8px;
      }
      .editfields {
        margin-left: 12px;
      }
    }
    ``` 
3. Spara filen. 

## <a name="display-the-component"></a>Visa komponenten

Nu när vi har komponenten, hur gör vi så att den visas på skärmen? Vi ska ändra standardkomponenterna i **app.component.ts**.

1. Öppna **client/app/app.component.ts** i Explorer-fönstret.

2. Ändra rubriken till Heroes på raderna 6 till och med 8 och lägg sedan till namnet på komponenten som vi skapade i **heroes.components.ts** (app-heroes) så att det refererar till den nya komponenten. Mallavsnittet bör se ut så här nu: 

    ```ts
    template: `
      <h1>Heroes</h1>
      <div class="header-bar"></div>
      <app-heroes></app-heroes>
    `,
    ```

3. Det finns andra komponenter i **heroes.components.ts** som vi refererar till, t.ex. Hero-komponenten, så vi måste skapa även detta. Kör följande kommando i kommandotolken i Angular CLI för att skapa en hero-modell och en fil med namnet **hero.ts**, där g = generera, cl = klass och hero = namnet på klassen.

    ```bash
    ng g cl hero
    ```

    En bekräftelse av den nya klassen visas i terminalfönstret.

    ```bash
    installing class
    create src\client\app\hero.ts
    ```

4. Öppna **src\client\app\hero.ts** i Explorer-fönstret.

5. I **hero.ts** ersätter du innehållet i filen med följande kod, som lägger till en Hero-klass med ett id, ett name och en saying. 

    ```ts
      export class Hero {
      id: number;
      name: string;
      saying: string;
    }
    ```

6. Gå tillbaka till **heroes.components.ts** och observera att `Hero` på raden `selectedHero: Hero;` (rad 10) har en röd understrykning. 

7. Vänsterklicka på termen `Hero` så visas en ikon av en glödlampa i Visual Studio till vänster om kodblocket. 

    ![Glödlampa i Visual Studio Code](./media/tutorial-develop-mongodb-nodejs-part3/light-bulb.png)

8. Klicka på glödlampan och sedan på **Importera Hero från ”client/app/hero”** eller **Importera Hero från ”./hero”.** (Meddelandet varierar beroende på din konfiguration)

    En ny rad med kod visas på rad 2. Om rad 2 refererar till client/app/hero ändrar du den så att den refererar till hero-filen från den lokala mappen (./hero). Rad 2 bör se ut så här:

   ```
   import { Hero } from "./hero";
   ``` 

    Nu är modellen på plats, men vi måste fortfarande skapa tjänsten.

## <a name="create-the-service"></a>Skapa tjänsten

1. Skriv följande kommando i kommandotolken i Angular CLI för att skapa en hero-tjänst i **app.module.ts**, där g = generera, s = tjänst, hero = namnet på tjänsten och -m = lägg till i app.module.

    ```bash
    ng g s hero -m app.module
    ```

    Meddelandet som returneras anger att **hero.service.ts** har skapats och att **app.module.ts** har uppdaterats.
  
    ```bash
    installing service
      create src\client\app\hero.service.ts
      update src\client\app\app.module.ts
    ```
    
    I app.module.ts lades följande rader med kod till (raderna 6 och 17):
    
    ```typescript
    import { HeroService } from './hero.service';
    ...
        providers: [HeroService],
    ```

2. Öppna **hero.service.ts** i Visual Studio Code och kopiera följande kod genom att ersätta innehållet i filen.

    ```ts
    import { Injectable } from '@angular/core';
    import { HttpClient } from '@angular/common/http';
    
    import { Hero } from './hero';
    
    const api = '/api';

    @Injectable()
    export class HeroService {
      constructor(private http: HttpClient) {}

      getHeroes() {
        return this.http.get<Array<Hero>>(`${api}/heroes`)
      }

      deleteHero(hero: Hero) {
        return this.http.delete(`${api}/hero/${hero.id}`);
      }

      addHero(hero: Hero) {
        return this.http.post<Hero>(`${api}/hero/`, hero);
      }

      updateHero(hero: Hero) {
        return this.http.put<Hero>(`${api}/hero/${hero.id}`, hero);
      }
    }
    ```

    Den här koden använder den senaste versionen av HttpClient som Angular tillhandahåller, som är en modul som du måste ange. Och det ska vi göra nu.

3. Öppna **app.module.ts** i Visual Studio Code och importera HttpClientModule genom att uppdatera importavsnittet så att det innehåller HttpClientModule.

    ```ts
    imports: [
      BrowserModule,
      FormsModule,
      HttpClientModule
    ],
    ```

4. I **app.module.ts** lägger du till HttpClientModule-importinstruktionen till listan med importer.

    ```ts
    import { HttpClientModule } from '@angular/common/http';
    ```

5. Gå tillbaka till **heroes.components.ts** i Visual Studio Code. Observera att `HeroService` på raden `constructor(private heroService: HeroService) {}` (rad 13) har en röd understrykning. Klicka på `HeroService` så visas glödlampan till vänster om kodblocket. Klicka på glödlampan och sedan på **Importera HeroService från ”./hero.service ”** eller på **Importera HeroService från ”client/app/hero.service”.**

    När du klickar på glödlampan infogas en ny rad med kod på rad 2. Om rad 2 refererar till mappen client/app/hero.service ändrar du den så att den refererar till hero-filen från den lokala mappen (./hero.serivce). Rad 2 bör se ut så här:
    
    ```javascript
    import { HeroService } from "./hero.service"
    ```

6. Spara alla filer i Visual Studio Code.

## <a name="build-the-app"></a>Skapa appen

1. Skapa Angular-programmet genom att skriva följande kommando i kommandotolken. 

    ```bash
    ng b
    ``` 

    Om det uppstår problem visas information om de filer som behöver åtgärdas. När appen har skapats läggs de nya filerna till i mappen **dist**. Du kan granska de nya filerna i mappen **dist** om du vill.

    Nu ska vi köra appen.

2. Klicka på knappen **Felsök** ![Felsökningsikon i Visual Studio Code](./media/tutorial-develop-mongodb-nodejs-part2/debug-button.png) till vänster i Visual Studio Code och klicka sedan på knappen **Starta felsökning** ![Felsökningsikon i Visual Studio Code](./media/tutorial-develop-mongodb-nodejs-part3/start-debugging-button.png).

3. Öppna en webbläsare och gå till **localhost:3000** och se när appen körs lokalt.

     ![Hero-programmet körs lokalt](./media/tutorial-develop-mongodb-nodejs-part3/azure-cosmos-db-mongodb-mean-app.png)

## <a name="next-steps"></a>Nästa steg

I den här delen av självstudiekursen har du gjort följande:

> [!div class="checklist"]
> * Skapat Angular-användargränssnittet
> * Testat appen lokalt

Fortsätt till nästa del av självstudiekursen och skapa ett Azure Cosmos DB-konto.

> [!div class="nextstepaction"]
> [Skapa ett Azure Cosmos DB-konto med hjälp av Azure CLI](tutorial-develop-mongodb-nodejs-part4.md)
