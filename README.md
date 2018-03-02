# Angular5_inter_questions

For answers, please, create separate branch like: "luke_skywalker" 

**1. Q: I want a component to listen the “double click” event only if it is ran from a computer. On a tablet or smartphone the component is the same, but it doesn’t respond to this event. How can I do it?**

*A: We should use a regexp (from [detectmobilebrowsers.com](http://detectmobilebrowsers.com/)) to detect if user’s navigator is a mobile browser. If so, we can return false. We can create a helper, which will mutate on mobile devices and do nothing, and on desktop it will run passed function.*

```
let check = false;
if(regexpFromDetectMobileBrowsers.test(a.substr(0,4))) check = true;
((a)=> {
    if(regexpFromDetectMobileBrowsers.test(a.substr(0,4))) check = true;
    console.log('executed')
})();
```

**2. Q: I’ve filled out a form, but before sending it I want to visit another page of the application. How can I save the state information of the form before changing the route? When I come back to its page, how does the form restore its data?**

*A: We will use localStorage to save data when the route will change. For instance:*

```
import 'rxjs/add/operator/pairwise';
import { Router } from '@angular/router;
import { NgForm } from '@angular/forms';

export class AppComponent {
  constructor(private router: Router, private form: ngForm) {
    let prevValuesString = localStorage.getItem('some-super-unique-key');
    if (prevValuesString) this.form.value = JSON.parse(prevValuesString);
    this.router.events.pairwise().subscribe((event) => {
      localStorage.setItem('some-super-unique-key', JSON.stringify(this.form.value));
    });
  };
  
  alsoSomeFormSumbitHandlerToClearLocalStorageWhenWeDontNeedDataAnymore(){
    localStorage.clearItem('some-super-unique-key');
    // + some submit handler logic here
  }
}
```

**3. Q: I want two different component to communicate, the user interaction on one changes the state of the other one and vice versa. How can I do it? If I remove (NOT HIDE, all listeners and scopes are deleted) one of the two components, what do I need to do so that there are no errors for the users? And, when it re-appends, how can I know it and update the data?**

*A: Events, or (even better, if possible) common parent component. There is different ComputerScience-based approaches to save consistency of the data in the case if one component will be destroyed and then resumed back. In second case (common parent) we can just rely on it’s state. 
Below I will implement common parent and two child components which will communicate one with another. One component can trigger a value-change in another component and vice-versa.*

```
<parent>
  <child [(someModel)]/> 
  <child [(someModel)]/>
</parent>
```

*Now, a change in one child will trigger a change in other child. This is a two-way data binding, but we can split it to model and modelChange event to implement a different component-specific behavior.*

**4. Q: Please, create an operating Angular component from HTML that produces a DOM, containing another working Angular components.**

*A: If I understand your task well, we can use ng-transclude (or ng-content in terms of Angular Next) for our needs:*

```
import { Component } from '@angular/core';
@Component({ 
selector: 'my-component',
template: ` <div> <ng-content></ng-content> </div> `
})
export class MyComponent {}
```	

So this component will render the html (including other components) which is passed into it’s body like `<my-component> Some components or content I want to parse and get rendered in the ng-content section of my component </my-component>`

