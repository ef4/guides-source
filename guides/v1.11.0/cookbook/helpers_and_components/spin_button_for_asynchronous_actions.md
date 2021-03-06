### Problem
You want a button component that spins to show asynchronous action till completion. Eg- Save Button.

### Solution
Write an Ember Component to change to loading state when action is taking place.

For example a button to save data could be as

```handlebars {data-filename=app/templates/application.hbs}
{{spin-button id="forapplication" isLoading = isLoading buttonText=buttonText action='saveData'}}
```

```handlebars {data-filename=app/templates/components/spin-button.hbs}

<button id={{id}} {{action 'showLoading'}}>
  {{#if isLoading}}
    <img src="http://i639.photobucket.com/albums/uu116/pksjce/spiffygif_18x18.gif">
  {{else}}
    {{buttonText}}
  {{/if}}
</button>
```

```javascript {data-filename=app/controllers/application.js}
export default Ember.Controller.extend({
    isLoading:false,
    buttonText:"Submit",
    actions:{
        saveData:function(){
            var self = this;

           //Do Asynchronous action here. Set "isLoading = false" after a timeout.
            Ember.run.later(function(){
                self.set('isLoading', false);
            }, 1000);
        }
    }
});
```

```javascript {data-filename=app/components/spin-button.js}
export default Ember.Component.extend({
	classNames: ['button'],
    buttonText:"Save",
    isLoading:false,
    actions:{
        showLoading:function(){
            if(!this.get('isLoading')){
                this.set('isLoading', true);
                this.sendAction('action');
            }
        }
    }
});
```

### Discussion

I have dumbed down the sample code to only change text within the button. One may add a loading image inside the button or change the button to a div styled like a button.
The component is in charge of setting isLoading = true and the base controller performing asynchronous action decides when the 'isLoading' becomes false again.
For safety and sanity of the component, one can add a settimeout of however much time and then set 'isLoading' back to false so that the components comes to initial state no matter the result of the asynchronous call. But I would prefer it was properly handled in the parent controller.
Also note that the component does not let multiple clicks get in the way of loading status.

<!---#### Example
<a class="jsbin-embed" href="http://jsbin.com/patikodeje/7/embed?live">JS Bin</a><script src="https://static.jsbin.com/js/embed.js"></script>-->
