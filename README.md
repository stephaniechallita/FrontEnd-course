# FrAdministrationFront

Pour la partie TP du module programmation frontend web, nous développerons un frontend d'une application web, basé sur le 
framework `Angular`.
`Angular` est écrit en Typescript et permet un grand nombre d'automatisations et procure un ensemble d'outils qui rend le 
développement d'un frontend agréable et simple.

## Pré-requis

Il vous faut [installer `NodeJS`](https://nodejs.org/en/download/) pour pouvoir installer et utiliser `Angular`.
Suivez la procédure en fonction de votre système d'exploitation.

L'IDE à utiliser est [VisualCode Studio](https://code.visualstudio.com/). Une fois installé, vous pouvez le lancer depuis
un terminal en vous plaçant dans le répertoire de votre projet et en tapant la commande : 

```shell
code .
```

Afin de tester le frontend, il faudra ouvrir un navigateur web. Vous pouvez utiliser la fonction `console.log(message);` pour afficher des messages, qui seront visibles dans la console de votre navigateur.
Suivant le navigateur que vous utilisez, l'accès à cette console peut différer : voir plus d'info [ici](https://balsamiq.com/support/faqs/browserconsole/).

## Création d'un nouveau projet Angular (frontend en typescript) :

```sh
$ ng new fr-administration-front
? Do you want to enforce stricter type checking and stricter bundle budgets in the workspace?
  This setting helps improve maintainability and catch bugs ahead of time.
  For more information, see https://angular.io/strict (y/N)
  y
  y
  SCSS
```

S'il y a une erreur liée à `jasmine`, il suffit de lancer la commande : `npm install -g npm@6`, puis de refaire la génération avec la commande ci-dessus.

Maintenant, on peut lancer le front : 

```sh
$ ng serve --open
```

avec l'option `--open`, un browser se lancera et se connectera au front (par défault l'url est : `http://localhost:4200`).

## Exercice 1 : Liste d'objets avec Angular-Materials

Dans cet exercice, nous allons créer une page dédiée à une liste d'utilisateurs (`user`) en utilisant Materials.
Materials est une bibliothèque qui fournit un ensemble de composants Angular.

### Création d'un composant Angular

Pour créer un nouveau composant, on utilise la commande suivante :

```sh
$ ng g component users-list
CREATE src/app/users-list/users-list.component.scss (0 bytes)
CREATE src/app/users-list/users-list.component.html (25 bytes)
CREATE src/app/users-list/users-list.component.spec.ts (648 bytes)
CREATE src/app/users-list/users-list.component.ts (291 bytes)
UPDATE src/app/app.module.ts (647 bytes)
```

Ici, Angular nous génère plusieurs fichiers dans le répertoire `src/app/users-list` dont 
- `src/app/users-list/users-list.component.html` qui va contenir la structure du composant Angular ;
- `src/app/users-list/users-list.component.ts` qui va contenir le comportement du composant Angular.

L'un ne va pas sans l'autre.

Remarquez que Angular a aussi mis à jour le fichier `src/app/app.module.ts` avec un nouvel import.

### Routing

On va maintenant ajouter une nouvelle route pour associer une URL à notre nouveau composant Angular :

Dans `src/app/app-routing.module.ts` :

```diff
const routes: Routes = [
+  {
+    path: '',
+    component: UsersListComponent,
+  },
];
```
On relance le front avec `ng serve` et on observe notre page, en bas nous devrions voir `users-list works!` qui est du texte qui a été généré en même temps que le composant.

### Première édition

Éditez `app.component.html` pour qu'il ne contienne que la dernière ligne : `<router-outlet></router-outlet>`
On devrait voir que "users-list works!" maintenant sur une page blanche.
	
### Ajout de Angular Materials
 
Nous allons installer [`Materials`](https://material.angular.io/guide/getting-started):

```sh
$ ng add @angular/material
Installing packages for tooling via npm.
Installed packages for tooling via npm.
? Choose a prebuilt theme name, or "custom" for a custom theme: Indigo/Pink        [ Preview: https://material.angular.io?theme=indigo-pink ]
? Set up global Angular Material typography styles? No
? Set up browser animations for Angular Material? Yes
UPDATE package.json (1281 bytes)
✔ Packages installed successfully.
UPDATE src/app/app.module.ts (756 bytes)
UPDATE angular.json (4005 bytes)
UPDATE src/index.html (566 bytes)
UPDATE src/styles.scss (181 bytes)
```

### Ajout d'un module Material
 
Dans `src/app/app.module.ts`, ajouter le `MatTableModule` aux imports :
 
```diff
+import {MatTableModule} from '@angular/material/table'; 
...
imports: [
    BrowserModule,
    AppRoutingModule,
    BrowserAnimationsModule,
+    MatTableModule
],
...
```
S'il y a un problème, relancez le frontend avec `ng serve --open`.
 
### Création de la classe User et d'un tableau contenant des enregistrements

Pour simuler un backend, nous allons créer, en dur dans le front, un tableau avec des instances de `user`.
Ajoutez dans le fichier `src/app/users-list/users-list.component.ts` le code suivant : 
 
```ts
export class User {
  constructor(
    public id: number,
    public lastname: string,
    public firstname: string,
    public age: number,
  ) {}
}
const users: User[] = [
  new User(0, 'Doe', 'John', 23),
  new User(1, 'Doe', 'Jane', 32),
]
```

### Ajout du tableau

On va ajouter un composant `mat-table` dans `src/app/users-list/users-list.component.html`:


```html 
<table mat-table [dataSource]="dataSource" class="mat-elevation-z8">

    <ng-container matColumnDef="id">
      <th mat-header-cell *matHeaderCellDef> id </th>
      <td mat-cell *matCellDef="let user"> {{user.id}} </td>
    </ng-container>

    <ng-container matColumnDef="lastname">
      <th mat-header-cell *matHeaderCellDef> Lastname </th>
      <td mat-cell *matCellDef="let user"> {{user.lastname}} </td>
    </ng-container>
  
    <ng-container matColumnDef="firstname">
      <th mat-header-cell *matHeaderCellDef> Firstname </th>
      <td mat-cell *matCellDef="let user"> {{user.firstname}} </td>
    </ng-container>
  
    <ng-container matColumnDef="age">
      <th mat-header-cell *matHeaderCellDef> Age </th>
      <td mat-cell *matCellDef="let user"> {{user.age}} </td>
    </ng-container>
  
    <tr mat-header-row *matHeaderRowDef="displayedColumns"></tr>
    <tr mat-row *matRowDef="let row; columns: displayedColumns;"></tr>
</table>
```

et dans `src/app/users-list/users-list.component.ts`

```diff
export class UsersListComponent implements OnInit {
+  displayedColumns: string[] = ['id', 'lastname', 'firstname', 'age'];
+  dataSource = users;
  ...
```
  
un peu de css pour que ça fasse joli dans `src/app/users-list/users-list.component.scss` :

```css
table {
    margin: 10%;
    width: 75%;
}
```

Avec la Table de Materials, il y a plein de features cool à explorer : sorting, click log, etc... voir plus d'info [ici](https://material.angular.io/components/table/overview)

## Exercice 2 : page de login, et première communication avec le backend

### Création d'un nouveau composant pour le login.
 
Créez un nouveau composant pour le login :

```sh
ng g component login 
```
 
### Mise à jour des routes
 
On va maintenant ajouter et modifier les routes de notre frontend. 
Ici, on veut que le chemin par defaut nous mène vers le nouveau composant `login`, tandis que l'url `/users` nous redirige vers le component précèdemment créé.

Toujours dans le fichier `src/app/app-routing.module.ts` :

```diff
const routes: Routes = [
+  {
+    path: '',
+    component: LoginComponent,
+  },
  {
+    path: 'users',
    component: UsersListComponent,
  },
];
```

### Création de la page de login

Pour créer la page de login, on va procèder par étape et ajouter des fonctionnalités petit à petit.

#### Création des deux champs du login

On va ajouter deux champs input avec leur label dans `src/app/login/login.component.html`:

```html
<div>
  <label for="username">Email</label>
  <input type="text" id="username" placeholder="username" />
  <label for="password">Password</label>
  <input type="password" id="password" placeholder="password" />
  <a><button>Login</button></a>
</div>
```

#### Logique derrière le click

On va maintenant ajouter de la logique sur le click du button login :

```diff
+ <a (click)="login()"><button>Login</button></a>
```

Ici, la directive `(click)="` va associer l'évènement du click sur le button, à une fonction.
Pour rappel, le fichier HTML donne la structure, et le fichier typescript donne la logique.
Il faut donc implémenter la fonction `login()` dans notre fichier `src/app/login/login.component.ts` pour que cela fonctionne.
  
```ts
login(): void {
    console.log('click on login !')
}
```
  
Cliquez sur le button et observer dans la console (click droit inspect -> console) que le message s'affiche correctement.

#### Récupération des données des champs

On veut maintenant lire les données saisies par l'utilisateur dans les champs de la page login.

Dans les fichiers typescript des composants Angular, on a accès à la page HTML grâce à la variable globale `document`.
On peut ainsi manipuler la page HTML depuis notre fichier TS.
Plus particulièrement, on peut récupérer des éléments via leur id (au sens HTML/CSS) avec la function `getElementById(str)`.

Pour récupérer les informations, il suffit alors de faire :

```diff
login(): void {
-   console.log('click on login !')
+   const email: string = (document.getElementById('username') as HTMLInputElement).value;
+   const password: string = (document.getElementById('password') as HTMLInputElement).value;
+   console.log(email, password);
  }
```

On pourra alors observer dans la console les valeurs saisies par l'utilisateur.

#### Mise en place de la communication avec le backend

Récupérez l'archive `backend.zip`, décompressez-la et lancez le backend avec la commande suivante :

```sh
$ node dist/main.js
[Nest] 862716   - 10/22/2021, 6:25:42 PM   [NestFactory] Starting Nest application...
[Nest] 862716   - 10/22/2021, 6:25:42 PM   [InstanceLoader] AppModule dependencies initialized +175ms
[Nest] 862716   - 10/22/2021, 6:25:42 PM   [InstanceLoader] TypeOrmModule dependencies initialized +0ms
[Nest] 862716   - 10/22/2021, 6:25:42 PM   [InstanceLoader] PassportModule dependencies initialized +0ms
[Nest] 862716   - 10/22/2021, 6:25:42 PM   [InstanceLoader] JwtModule dependencies initialized +0ms
[Nest] 862716   - 10/22/2021, 6:25:42 PM   [InstanceLoader] AuthModule dependencies initialized +2ms
[Nest] 862716   - 10/22/2021, 6:25:42 PM   [InstanceLoader] TypeOrmCoreModule dependencies initialized +78ms
...
```

Le backend est en train de tourner.
  
On va maintenant ajouter à notre front, la capacité d'envoyer des requêtes.
Pour cela, ajoutez le module `HttpClientModule` dans les `imports` du fichier `src/app/app.module.ts`:

```diff
imports: [
    BrowserModule,
    AppRoutingModule,
    BrowserAnimationsModule,
    MatTableModule,
    MatSortModule,
+    HttpClientModule,
],
```

Dance ce module, la classe `HttpClient` nous offre des fonctions pour faire diverses requêtes sur notre backend: `get()`, `post()`, `put()`, `delete()`.
En fait, elle nous offre toutes les méthodes de requêtes dont on a besoin pour développer un service fullstack (front + back) REST.

#### Modification de la liste des utilisateurs

Pour commencer en douceur la communication, on va récupérer la liste des utilisateurs depuis le backend plutôt que d'utiliser une liste en dur dans le frontend.

Ajoutez un constructor dans le composant de la liste (`src/app/users-list/users-list.component.ts`) avec un `HttpClient`:

```ts
constructor(
    private http: HttpClient
) {}
```

Cela fonctionne comme l'injection de dépendances dans le backend vue l'an dernier.

On va maintenant modifier la fonction `ngOnInit` de notre `UsersListComponent`.
Cette fonction est appelée au moment où le composant est initialisé.

```
const resquest: Observable<any> = this.http.get('http://localhost:3000/users', { observe: 'response' });
resquest.toPromise().then(response => this.dataSource = response.body);
```

TODO: détailler un peu ce qui se passe.

Relancez le front, et connectez-vous à l'URL `https://localhost:4200/users`.

On peut maintenant supprimer le tableau users du front (car maintenant on le récupère depuis le backend).

```diff
-export class User {
-  constructor(
-    public id: number,
-    public lastname: string,
-    public firstname: string,
-    public age: number,
-  ) {}
-}

-const users: User[] = [
-  new User(0, 'Doe', 'John', 23),
-  new User(1, 'Doe', 'Jane', 32),
-]
...
- dataSource = users;
+ dataSource = [];
```

#### Requête auprès du backend pour le login

On va maintenant modifier la function `login()` de `src/app/login/login.component.ts` afin de faire une requête `POST` sur le backend.

On utilise ici l'api-helper, qui évite de gérer les requêtes dans les differents components. Récupérez le fichier `api-helper.service.ts` et copiez-le dans un nouveau folder `src/app/services`.

```diff
+constructor(
+      private api: ApiHelperService
+) {}

login(): void {
  const username: string = (document.getElementById('username') as HTMLInputElement).value;
  const password: string = (document.getElementById('password') as HTMLInputElement).value;
+  this.api.post({endpoint: '/auth/login', data: { username, password },}).then(response => console.log(response));
}

Retournez sur l'addresse `http://localhost:4200`, et renseignez les informations suivantes : `username = 1` et `password = password`, puis cliquez sur le bouton login.

Dans la console, vous devriez observer quelque chose comme cela :

```json
Object { access_token: "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6MSwiaWF0IjoxNjM1MzcxODUyLCJleHAiOjE2MzUzNzE5MTJ9.5GO-V4LXYdYzEZrqADODykldKKaXCpsQ-prEQjrahHM" }`
```

Qui est le JWT retourné par le backend en cas de login réussi.

#### Sauvegarde du JWT et interceptors

On veut maintenant que, une fois authentifié, notre frontend sauvegarde le token et l'envoie dans toutes les requêtes suivantes.
Pour ce faire, nous allons utiliser un `interceptor`, qui va intercepter toutes les requêtes pour y ajouter le token automatiquement, et ainsi sa gestion sera transparente pour le développement.

Créez le fichier `src/app/services/token-storage.service.ts` et y mettre :

```ts
import { Injectable } from '@angular/core';

const TOKEN_KEY = 'token';
const USERNAME_KEY = 'username';
const IS_LOGGED_IN = 'isLoggedIn';
const IS_LOGGED = 'true';

@Injectable({
  providedIn: 'root'
})
export class TokenStorageService {

  public clear(): void {
    localStorage.clear();
  }

  public save(token: string): void {
    localStorage.removeItem(TOKEN_KEY);
    localStorage.removeItem(USERNAME_KEY );
    localStorage.removeItem(IS_LOGGED_IN);
    localStorage.setItem(TOKEN_KEY, token);
    localStorage.setItem(IS_LOGGED_IN, IS_LOGGED);
  }

  public getToken(): string {
    const token = localStorage.getItem(TOKEN_KEY);
    return token === null ? '' : token;
  }

  public isLogged(): boolean {
    return (Boolean)(localStorage.getItem(IS_LOGGED_IN));
  }

}
```
TODO: expliquer le `localStorage`

Modifiez le fichier `src/app/login/login.component.ts` pour enregistrer le JWT dans le `TokenStorageService` :

```diff
constructor(
    private api: ApiHelperService,
+    private tokenStorageService: TokenStorageService,
  ) {}

  ngOnInit() {
  }

  login(): void {
    const username: string = (document.getElementById('username') as HTMLInputElement).value;
    const password: string = (document.getElementById('password') as HTMLInputElement).value;
    this.api.post({endpoint: '/auth/login', data: { username, password },})
-      .then(response => console.log(response));
+      .then(response => this.tokenStorageService.save(response.access_token));
  }
```

Créez le fichier `src/app/interceptors/token.interceptor.ts`:

```ts
import { Injectable } from '@angular/core';
import {
  HttpInterceptor,
  HttpRequest,
  HttpHandler,
  HttpEvent,
} from '@angular/common/http';
import { Observable } from 'rxjs';
import { tap } from 'rxjs/operators';
import { TokenStorageService } from '../services/token-storage.service';

@Injectable({
  providedIn: 'root',
})
export class TokenHttpInterceptor implements HttpInterceptor {

  constructor(
    private service: TokenStorageService
  ) {}

  // C'est dans la fonction intercept qu'on implémente la logique
  intercept(
    request: HttpRequest<any>,
    next: HttpHandler
  ): Observable<HttpEvent<any>> {
    // On récupère le token depuis le TokenStorageService
    const token = this.service.getToken();
    // s'il n'est pas initialisé, on envoie la requête telle qu'elle est
    if (!token) { 
      return next.handle(request);
    }
    // Si non, on va injecter le token dedans :
    const updatedRequest = request.clone({
      headers: request.headers.set('Authorization', `Bearer ${token}`),
    });
    // et envoyer la requête avec le token
    return next.handle(updatedRequest).pipe(
      tap(
        (event) => {},

        (error) => {}
      )
    );
  }
}
```

et modifiez `src/app/app.module.ts` :

```diff
+import { HttpClientModule, HTTP_INTERCEPTORS } from '@angular/common/http';
import { LoginComponent } from './login/login.component';
import { UsersListComponent } from './users-list/users-list.component';
import { BrowserAnimationsModule } from '@angular/platform-browser/animations';

import { MatTableModule } from '@angular/material/table'; 
import { MatSortModule } from '@angular/material/sort';
+import { TokenHttpInterceptor } from './interceptors/token.interceptor';

@NgModule({
  declarations: [
    AppComponent,
    LoginComponent,
    UsersListComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    HttpClientModule,
    BrowserAnimationsModule,
    MatTableModule,
    MatSortModule,
  ],
  providers: [
+    {
+      provide: HTTP_INTERCEPTORS,
+      useClass: TokenHttpInterceptor,
+      multi: true,
+    },
  ],
  bootstrap: [AppComponent]
})
```
