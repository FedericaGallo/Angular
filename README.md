

- [Comandi base](#comandi)  
- [Introduzione](#angular)  
- [Introduzione](#angular)  
- [Introduzione](#angular)  
- [Introduzione](#angular)  

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
cd my-first-angular-app
npm start
```
#### generare un componente
```bash
ng generate component nome-componente
```
```bash
ng g c nome-componente
```
#### componente.ts
```javascript
import { Component } from '@angular/core';

@Component({
  selector: 'app-example',
  templateUrl: './example.component.html',
})
export class ExampleComponent {}

```
#### utilizzo componente 
```html
<app-example></app-example>
```
#### data binding
In Angular, un binding crea una connessione dinamica tra il template di un componente e i suoi dati. Questa connessione garantisce che le modifiche ai dati del componente aggiornino automaticamente il template renderizzato.
```javascript
export class AppComponent {
  message = 'Hello, Angular!';
}
```
```html
<p>{{ message }}</p>
```
#### string interpolation
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
#### event bindiding
```html
<button (click)="onClick()">Clicca qui</button>
```
```javascript
export class AppComponent {
  onClick() {
    alert('Hai cliccato il bottone!');
  }
}
```
#### two-way binding
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
#### Two-way binding between components
ogni binding bidirezionale per i componenti richiede i seguenti elementi:
Il componente figlio deve includere:
- Una proprietà @Input().
- Un corrispondente @Output() con lo stesso nome della proprietà @Input(), ma con "Change" aggiunto alla fine. L'emitter deve anche emettere lo stesso tipo della proprietà @Input().
- Un metodo che emette il valore aggiornato dell'@Input() attraverso l'evento @Output().
Il componente genitore deve:
- Avvolgere il nome della proprietà @Input() nella sintassi del binding bidirezionale.
- Specificare la proprietà corrispondente a cui verrà assegnato il valore aggiornato.
Parent component
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
child component  

@imput serve per ricevere il valore iniziale dal contatore. La proprietà count riceve il valore initialCount dal genitore.  
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

# Direttive
I diversi tipi di direttive in Angular sono i seguenti:  
- __componenti__ : Un componente è una direttiva con un template associato.
  ```html
  <app-greeting></app-greeting> 
  ```
- __direttive di attributo__ :   

Direttive attributo integrate    
Le direttive attributo ascoltano e modificano il comportamento di altri elementi HTML, attributi, proprietà e componenti.
Le direttive attributo più comuni sono le seguenti:  
-  __NgClass__ Aggiunge e rimuove un set di classi CSS
-  __NgStyle__ Aggiunge e rimuove un set di stili HTML
-  __NgModel__ Aggiunge il data binding bidirezionale a un elemento del modulo HTML. 
testo a caso  
testo a caso  
testo a caso  
testo a caso  
testo a caso  
testo a caso  
testo a caso  
testo a caso  
testo a caso  

# Angular




