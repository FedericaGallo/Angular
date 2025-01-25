# Angular

- [Comandi base](#comandi)  
- [Introduzione](#introduzione)  
- [Binding](#binding)  
- [Direttive](#direttive)    
- [RxJS](#rxjs)
  
Verifica che Node.js sia installato
```bash
node -v
```
Verifica che npm sia installato
```bash
npm -v
```
Installa Angular
```bash
npm install -g @angular/cli
```
crea un nuovo progetto
```bash
ng new <project-name>
```
```bash
cd project-name
npm start
```
#### generare un componente
```bash
ng generate component nome-componente
```
```bash
ng g c nome-componente
```
```bash
ng g c nome-componente --skip-tests
```
## Introduzione
### componente.ts
I componenti sono i mattoni fondamentali di qualsiasi applicazione Angular. Ogni componente ha tre parti:
- classe TypeScript
- template HTML
- Stili CSS
```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-example',
  templateUrl: './example.component.html',
  styleUrls: ['./exemple.component.css']
}),

export class ExampleComponent {}

```
### utilizzo componente 
```html
<app-example></app-example>
```
La proprietà selector della configurazione del componente fornisce un nome da usare quando si fa riferimento al componente in un altro template. 
## Binding
### data binding
In Angular, un binding crea una connessione dinamica tra il template di un componente e i suoi dati. Questa connessione garantisce che le modifiche ai dati del componente aggiornino automaticamente il template renderizzato.
```javascript
export class AppComponent {
  message = 'Hello, Angular!';
}
```
```html
<p>{{ message }}</p>
```
### string interpolation
Puoi associare testo dinamico nei template utilizzando le doppie parentesi graffe, che indicano ad Angular che è responsabile dell'espressione al loro interno e che deve assicurarne l'aggiornamento corretto. Questo viene chiamato interpolazione del testo.
```javascript
@Component({
  template: `
    <p>Your color preference is {{ theme }}.</p>
  `,
  ...
})
export class AppComponent {
  theme = 'dark';
}
```
È possibile utilizzare questa sintassi anche per chiamare funzioni, scrivere espressioni e altro ancora.
### property binding 
Il binding delle proprietà in Angular consente di impostare i valori delle proprietà di elementi HTML, componenti Angular e altro.
```html
<img alt="photo" [src]="imageURL">
```
In questo esempio, il valore dell'attributo src sarà legato alla proprietà della classe imageURL. Il valore di imageURL sarà impostato come attributo src del tag img.

```typescript
import {Component} from '@angular/core';

@Component({
  selector: 'app-root',
  styleUrls: ['app.component.css'],
  template: `
    <div [contentEditable]="isEditable"></div>
  `,
})
export class AppComponent {
  isEditable = true;
}
```

### event binding
```html
<button (click)="onClick()">Clicca qui</button>
```
```typescript
export class AppComponent {
  onClick() {
    alert('Hai cliccato il bottone!');
  }
}
```
### two-way binding
Per utilizzare il two-way binding con i controlli dei form nativi, devi:

- Importare il FormsModule da @angular/forms.
- Usare la direttiva ngModel con la sintassi del two-way binding ([(ngModel)]).
- Assegnarle lo stato che desideri aggiornare (ad esempio, firstName).
```javascript
import { Component } from '@angular/core';
import { FormsModule } from '@angular/forms';
@Component({
  imports: [FormsModule],
  template: `
    <main>
      <h2>Hello {{ firstName }}!</h2>
      <input type="text" [(ngModel)]="firstName" />
    </main>
  `
})
export class AppComponent {
  firstName = 'Ada';
}
```
### Component Communication with @Input
è possibile passare dati da un componente padre a un componente figlio. Un componente specifica i dati che accetta dichiarando gli input:
__child component__
```typescript
import {Component, Input} from '@angular/core';

@Component({
  selector: 'app-user',
  template: `
    <p>The user's name is {{ name }}</p>
  `,
})
export class UserComponent {
  @Input() name = '';
}
```
__parent component__
```typescript
import {Component} from '@angular/core';
import {UserComponent} from './user.component';

@Component({
  selector: 'app-root',
  template: `
    <app-user name="Simran" />
  `,
  imports: [UserComponent],
})
export class AppComponent {}
```
### Two-way binding between components
Ogni binding bidirezionale per i componenti richiede i seguenti elementi:
Il componente figlio deve includere:
- Una proprietà @Input().
- Un corrispondente @Output() con lo stesso nome della proprietà @Input(), ma con "Change" aggiunto alla fine. L'emitter deve anche emettere lo stesso tipo della proprietà @Input().
- Un metodo che emette il valore aggiornato dell'@Input() attraverso l'evento @Output().
Il componente genitore deve:
- Avvolgere il nome della proprietà @Input() nella sintassi del binding bidirezionale.
- Specificare la proprietà corrispondente a cui verrà assegnato il valore aggiornato.   

__Parent component__
```javascript
import { Component } from '@angular/core';
import { CounterComponent } from './counter/counter.component';
@Component({
  selector: 'app-root',
  imports: [CounterComponent],
  template: `
    <main>
      <h1>Counter: {{ initialCount }}</h1>
      <app-counter [(count)]="initialCount"></app-counter>
    </main>
  `,
})
export class AppComponent {
  initialCount = 18;
}
```
__child component__  
@input serve per ricevere il valore iniziale dal contatore. La proprietà count riceve il valore initialCount dal genitore.  
@output serve per notificare il genitore quando il valore cambia. L'evento si chiama countChange e usa un EventEmitter per inviare il nuovo valore al genitore.
```javascript
import { Component, EventEmitter, Input, Output } from '@angular/core';
@Component({
  selector: 'app-counter',
  template: `
    <button (click)="updateCount(-1)">-</button>
    <span>{{ count }}</span>
    <button (click)="updateCount(+1)">+</button>
  `,
})
export class CounterComponent {
  @Input() count: number;
  @Output() countChange = new EventEmitter<number>();
  updateCount(amount: number): void {
    this.count += amount;
    this.countChange.emit(this.count);
  }
}
```

### Direttive
I diversi tipi di direttive in Angular sono i seguenti:  
- __componenti__ : Un componente è una direttiva con un template associato.
  ```html
  <app-greeting></app-greeting> 
  ```
- __direttive di attributo__ : modificano l'aspetto o il comportamento di un elemento esistente.
- __direttive strutturali__ :  

__Direttive attributo Built-in__    
Le direttive attributo ascoltano e modificano il comportamento di altri elementi HTML, attributi, proprietà e componenti.
Le direttive attributo più comuni sono le seguenti:  
-  __NgClass__ Aggiunge e rimuove un set di classi CSS
-  __NgStyle__ Aggiunge e rimuove un set di stili HTML
-  __NgModel__ Aggiunge il data binding bidirezionale a un elemento del modulo HTML.
  
__Usare NgClass con un espressione__ : in questo caso, isSpecial è un valore booleano impostato su true in app.component.ts. Poiché isSpecial è true, ngClass applica la classe special al div.
```html
<div [ngClass]="isSpecial ? 'special' : ''">This div is special</div>
```
ngClass applica la classe speciale dinamicamente a seconda del valore della proprietà special
```html
<div [ngClass]="{'special': isSpecial}">
  Questo div avrà la classe 'special' se isSpecial è true.
</div>

<button (click)="toggleSpecial()">Cambia stato</button>
```
__Usare NgClass con un metodo__
Nel seguente esempio, setCurrentClasses() imposta la proprietà currentClasses con un oggetto che aggiunge o rimuove tre classi in base allo stato vero o falso di altre tre proprietà del componente.
Ogni chiave dell'oggetto è un nome di classe CSS. Se una chiave è true, ngClass aggiunge la classe. Se una chiave è false, ngClass rimuove la classe.
```javascript
currentClasses: Record<string, boolean> = {};
...
  setCurrentClasses() {
    this.currentClasses = {
      saveable: this.canSave,
      modified: !this.isUnchanged,
      special: this.isSpecial,
    };
  }
```
```html
<div [ngClass]="currentClasses">
  Questo div avrà classi dinamiche!
</div>
```
__ng Style__
```html
<div [ngStyle]="{'background-color': isRed ? 'red' : 'blue'}">
  Questo div cambia colore!
</div>

<button (click)="toggleColor()">Cambia Colore</button>

```
__esempio ngStyle con metodo__
```html
<div [ngStyle]="currentStyles">
  This div is initially italic, normal weight, and extra large (24px).
</div>
```
```javascript
currentStyles: Record<string, string> = {};
...
  setCurrentStyles() {
  
    this.currentStyles = {
      'font-style': this.canSave ? 'italic' : 'normal',
      'font-weight': !this.isUnchanged ? 'bold' : 'normal',
      'font-size': this.isSpecial ? '24px' : '12px',
    };
  }
```
Per impostare gli stili quando il componente viene inizializzato, puoi chiamare setCurrentStyles() nel ciclo di vita del componente, ad esempio nel metodo ngOnInit.
```javascript
export class AppComponent implements OnInit {
  canSave = true;
  isUnchanged = false;
  isSpecial = true;
  currentStyles: Record<string, string> = {};

  ngOnInit() {
    this.setCurrentStyles();
  }
```
__ngModel__
```html
<label for="example-ngModel">[(ngModel)]:</label>
<input [(ngModel)]="currentItem.name" id="example-ngModel">
```
Per personalizzare la tua configurazione, scrivi la forma espansa, che separa il binding delle proprietà dal binding degli eventi. Usa il binding delle proprietà per impostare la proprietà e il binding degli eventi per rispondere ai cambiamenti. L'esempio seguente cambia il valore dell'input in maiuscolo:
```html
<input [ngModel]="currentItem.name" (ngModelChange)="setUppercaseName($event)">
```

__Direttive strutturali Built-in__

Le direttive strutturali sono responsabili del layout HTML. Esse modellano o rimodellano la struttura del DOM, solitamente aggiungendo, rimuovendo e manipolando gli elementi host ai quali sono attaccate.

- NgIf:	Crea o rimuove condizionalmente elementi dal template..
- NgFor:	Ripete un nodo per ogni elemento in una lista.
- NgSwitch:	Un insieme di direttive che passano tra viste alternative.

__NgIf__
Per usare NgIf, aggiungilo alla lista degli import del componente.
```javascript
import {NgIf} from '@angular/common';
...
@Component({
...
    NgIf, // <-- import into the component
...
  ],
})
export class AppComponent implements OnInit {
...
}
```
```html
<app-item-detail *ngIf="isActive" [item]="item"></app-item-detail>
```
Quando l'espressione isActive restituisce un valore true, NgIf aggiunge ItemDetailComponent al DOM. Quando l'espressione è false, NgIf rimuove ItemDetailComponent dal DOM e smaltisce il componente e tutti i suoi sottocomponenti.
__Ngfor__
```javascript
import {NgFor} from '@angular/common';
...
@Component({
...
    NgFor, // <-- import into the component
...
  ],
})
export class AppComponent implements OnInit {
...
}
```
```html
<div *ngFor="let item of items">{{ item.name }}</div>
```
Per ripetere un elemento componente, applica *ngFor al selettore. Nell'esempio seguente, il selettore è <app-item-detail>.
```html
<app-item-detail *ngFor="let item of items" [item]="item"></app-item-detail>
```
```javascript
items = [
  { name: 'Item 1', description: 'Description 1' },
  { name: 'Item 2', description: 'Description 2' }
];

```
```html
<div *ngFor="let item of items">{{ item.name }}</div>
<app-item-detail *ngFor="let item of items" [item]="item"></app-item-detail>
```
```html
<app-item-detail
  *ngFor="let item of items"
  [name]="item.name"
  [price]="item.price">
</app-item-detail>
```
```javascript
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-item-detail',
  template: `
    <h3>{{ name }}</h3>
    <p>Price: {{ price }}</p>
  `
})
export class ItemDetailComponent {
  @Input() name!: string; // Riceve il valore da [name]
  @Input() price!: number; // Riceve il valore da [price]
}
```

Riduci il numero di chiamate che la tua applicazione fa al server tracciando le modifiche a una lista di elementi. Utilizzando la proprietà *ngFor trackBy, Angular può modificare e ri-renderizzare solo gli elementi che sono cambiati, invece di ricaricare l'intera lista di elementi.
Aggiungi un metodo nel componente che restituisca il valore che Angular dovrebbe tracciare con NgFor. In questo esempio, il valore da tracciare è l'id dell'elemento. Se il browser ha già renderizzato quell'id, Angular lo tiene in memoria e non effettua una nuova richiesta al server per lo stesso id.

```typescript
Copia codice

trackByItems(index: number, item: Item): number {
  return item.id;
}
```
Nell'espressione abbreviata, imposta trackBy sul metodo trackByItems():

```html

<div *ngFor="let item of items; trackBy: trackByItems">
  ({{ item.id }}) {{ item.name }}
</div>
```
Cambiare gli id crea nuovi elementi con nuovi item.id. Nel seguente esempio sull'effetto di trackBy, premendo il pulsante "Reset items" si creano nuovi elementi con gli stessi item.id.

Senza trackBy, entrambi i pulsanti attivano la sostituzione completa degli elementi nel DOM.
Con trackBy, solo il cambiamento dell'id provoca la sostituzione degli elementi nel DOM.
Spiegazione del funzionamento
Senza trackBy
Quando usi un semplice *ngFor senza specificare un metodo di tracciamento, Angular considera ogni elemento della lista come un nuovo elemento ogni volta che la lista viene modificata. Questo significa che:

- Tutti gli elementi della lista vengono eliminati dal DOM.
- Viene generata una nuova lista di elementi.
- Angular effettua nuove operazioni per ricreare il DOM completo.
- Questo comportamento può risultare inefficiente se gli elementi non cambiano davvero, ma vengono solo riorganizzati o aggiornati parzialmente.

Con trackBy
Quando specifichi un metodo trackBy:

Angular usa il valore restituito dal metodo (es. item.id) per identificare ogni elemento univocamente.
Se Angular trova un elemento con lo stesso id nel DOM, lo lascia invariato e aggiorna solo le parti che sono effettivamente cambiate.
Solo gli elementi che hanno un nuovo id vengono sostituiti nel DOM.
Questo approccio migliora l'efficienza:

- Riduce il numero di operazioni sul DOM.
- Minimizza le chiamate al server.
- Mantiene lo stato di eventuali elementi non modificati (es. input, selezioni).


```typescript
export class ElementComponent implement onInit{
element: {type: string, name: string, content:string};
}
```
```html
<p *ngIf="element.type==='server'" style="color: red">{{element.content}}</p>
```
### RxJS
RxJS è una libreria per comporre programmi asincroni e basati su eventi utilizzando sequenze di osservabili. Fornisce un tipo centrale, l'Observable, tipi satellite (Observer, Scheduler, Subjects) e operatori ispirati ai metodi Array (map, filter, reduce, every, ecc.) per consentire la gestione di eventi asincroni come collezioni.  
I concetti essenziali di RxJS che risolvono la gestione asincrona degli eventi sono:

- __Observable__: rappresenta l'idea di una collezione invocabile di valori o eventi futuri.
- __Observer__: è un insieme di callback che sa come ascoltare i valori forniti dall'Observable.
- __Subscription__: rappresenta l'esecuzione di un Observable, è utile soprattutto per annullare l'esecuzione.
- __Operatori__: sono funzioni pure che consentono uno stile di programmazione funzionale per trattare le collezioni con operazioni come map, filter, concat, reduce, ecc.
- __Oggetto__: è equivalente a un Emettitore di eventi e rappresenta l'unico modo per trasmettere un valore o un evento a più Osservatori.
- __Scheduler__: sono dispatcher centralizzati per controllare la concorrenza, consentendo di coordinare il momento in cui avviene la computazione, ad esempio, su setTimeout o requestAnimationFrame o altri.

> "An observable is an object that produces and controls a stream of data"
```typescript
interval(1000).subscribe({
next: (val) => console.log(val),
complete: () => {console.log("fine")},
error: () => console.log
});
```
La libreria  RxJS offre molte funzioni per creare Observable.
La funzione interval crea un Observable che emette un numero ogni intervallo di tempo specificato in millisecondi, è necessario sottoscrivere ```subscribe()``` per dare il via all'osservabile. Questo metodo di sottoscrizione prende un cosiddetto oggetto osservatore, che è un oggetto che può implementare fino a tre metodi. Ad esempio una funzione __next__ che verrà attivata per ogni nuovo valore emesso.
```typescript
interval(1000).pipe(
map((val)=> val* 2)
).subscribe({
next: (val) => console.log(val)
});
```
è possibile chiamare ```pipe()``` prima di sottoscrivere. ```pipe()``` consente di aggiungere alcuni __operatori__, come ad esempio map. ```map()``` è una funzione che prende un'altra funzione come argomento e viene eseguita su ogni valore emesso dal'Observable. Il risultato viene passato ai subscriber.
```typescript
import {DestroyRef, inject, Component, onInit } from '@angulat/core';
```
__DestroyRef__ viene iniettato nel costruttore utilizzando la funzione inject
```typescript
ngOnInit(): void {
private destroyRef = inject(DestroyRef);
const subscription = interval(1000).subscribe({
next: (val) => console.log(val)
});
this.destroyRef.onDestroy(() => {
subscription.unsubscribe();
});
}
```
__BehaviorSubject__: È un tipo di Subject che richiede un valore iniziale e emette il valore corrente ai nuovi sottoscrittori immediatamente al momento della sottoscrizione.
```typescript

```
  ### HttpClient
  Angular fornisce un'API HTTP client per le applicazioni Angular, la service class HttpClient in @angular/common/http.
```typescript
  export const appConfig: ApplicationConfig = {
  providers: [
    provideHttpClient(),
  ]}; 
```
```typescript
@NgModule({
  providers: [
    provideHttpClient(),
  ],
  // ... other application configuration
})
```
```typescript
export class AppModule {}
@Injectable({providedIn: 'root'})
export class ConfigService {
  constructor(private http: HttpClient) {
    // This service can now make HTTP requests via `this.http`.
  }
}
```
HttpClient ha metodi corrispondenti ai diversi verbi HTTP usati per fare richieste, sia per caricare dati che per applicare mutazioni sul server. Ogni metodo restituisce un __RxJS Observable__ che, una volta sottoscritto, invia la richiesta e poi emette i risultati quando il server risponde.
Attraverso un oggetto options passato al metodo request, si possono regolare varie proprietà della richiesta e il tipo di risposta restituito.
Il recupero dei dati da un backend richiede spesso una richiesta GET, utilizzando il metodo HttpClient.get(). Questo metodo accetta due argomenti: la stringa dell'URL dell'endpoint da cui prelevare i dati e un oggetto opzionale options per configurare la richiesta.
```typescript
http.get<Config>('/api/config').subscribe(config => {
  // process the configuration.
});
```
```<Config>``` è un tipo generico che viene passato al metodo get(). Indica che la risposta che ci aspettiamo dal server (i dati che riceviamo) sarà di tipo Config.

```typescript
getDocenti() {
   const subscribtion = this.httpClient.get<{docenti : Docente[]}>('http://localhost:8080/docente/findAll', {observe: 'response'}).subscribe({
      next: (response) => {
        console.log(response.body?.docenti);
        console.log(response.status);
        }
      });
}
```
L'oggetto di configurazione passato come secondo argomento ```{observe: 'response'}``` accetta come valore 'response' ed 'events'. Nel primo caso ```next()``` riceverà l'intero HttpResponse object, con body, header, status; nel secondo caso la funzione ```next()``` verrà triggerata da diversi eventi.
```typescript
http.post<Config>('/api/config', newConfig).subscribe(config => {
  console.log('Updated config:', config);
});
```
```newConfig``` è il corpo della richiesta POST, ovvero i dati che vogliamo inviare al server.
Questo oggetto (newConfig) contiene le informazioni che indicano cosa vogliamo cambiare sul server.
