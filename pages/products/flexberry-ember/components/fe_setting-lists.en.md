---
title: setting up lists
sidebar: flexberry-ember_sidebar
keywords: Flexberry Ember, OLV, setting
toc: true
permalink: en/fe_setting-lists.html
lang: en
autotranslated: true
hash: 99753d8c508e209d35fe692a11255f1fe3907ed517432f57e4f29a06d89c51b6
summary: setting up the control panel, custom buttons, tools, work with objects on the list, lock individual cells, hierarchical list of computable properties, the establishment by
---

Lists for different systems require different, often customized, approach. For these purposes, in the component Flexberry Objectlistview has implemented a number of mechanisms, allowing to nastraivat as the control panel and individual lines, cells, or appearance.

## Configuring control panel for lists

Control panel (toolbar) [Flexberry Objectlistview](fe_object-list-view.html) based on technological control `olv-toolbar`.

The configuration of the control panel occurs via the component `Flexberry Objectlistview`.

### Button create a new record

```hbs
{% raw %}
{{flexberry-objectlistview
  createNewButton = true
}}{% endraw %}
```

`createNewButton` - a flag that determines whether to display the button to create on the control panel.

### Creating a new entry based on

```hbs
{% raw %}
{{flexberry-objectlistview
  showPrototypeButtonInRow = true
  showPrototypeMenuItemInRow = true
}}{% endraw %}
```

`showPrototypeButtonInRow` - a flag that determines whether to display the button to create on the basis of the line.

`showPrototypeMenuItemInRow` - a flag that determines whether to display the button to create on the basis of the menu line.

You also need to specify the representation that will create a copy.

1.You can specify a view for a specific form - to new the router to set the `prototypeProjection` (view name string)

2.You can specify the view for the copy default for the entire model - the right model to set a property `prototypeProjection` (view name string)

### The refresh button

```hbs
{% raw %}
{{flexberry-objectlistview
  refreshButton = true
}}{% endraw %}
```

`refreshButton` - a flag that determines whether to display the refresh button on the control panel.

### The delete button selected records

```hbs
{% raw %}{{flexberry-objectlistview
  componentName = "..."
  deleteButton = true
  showCheckBoxInRow = true
  ...
}}{% endraw %}
```

In order to start to function the delete button selected records, you must define the following properties:

* `componentName` is the name of the control (used to identify the component parts of control that communicate through the [place](http://emberjs.com/api/classes/Ember.inject.html#method_service)). For example, the values for the list of records of type "employee" can specify "employeesFlexberryObjectListView".
* `deleteButton` - a flag that determines whether to display the delete button on the control panel.
* `showCheckBoxInRow` - a flag that determines whether to display the checkbox in the row

{% include note.html content="If Flexberry Objectlistview, there was not one row, the delete button is not active. When there are marked records, the delete button becomes available." %}

Deleting information is sent to the server to save the changes.

## Custom buttons for lists

### Embedding custom buttons to the toolbar list

Embedded buttons in the controller form, we need to determine a number of properties:

```javascript
{
    buttonName: '...', // Displayed name of the button. 
    buttonAction: '...', // Action called by the controller when this button is clicked (should be specified in the template). 
    buttonClasses: '...', // Css class of the button. 
    buttonTitle: '...' // Signature. 
}
```

If you want to add a few buttons, then their properties are set in the array:

```javascript
[{ buttonName: ..., buttonAction: ..., buttonClasses: ... }, {...}, ...]
```

To add to the toolbar list custom button in the controller you need to define a method `customButtonsMethod`. For example:

```javascript
import Ember from 'ember';
import ListFormController from 'ember-flexberry/controllers/list-form';

export default ListFormController.extend({
...
customButtonsMethod: Ember.computed('i18n.locale', function() {
    let i18n = this.get('i18n');
    return [{
        buttonName: i18n.t('forms.components-examples.flexberry-objectlistview.toolbar-custom-buttons-example.custom-button-name'),
        buttonAction: 'userButtonActionTest',
        buttonClasses: 'test-click-button'
        }];
    })
});
```

Further, in the controller, you need to specify the event `buttonAction`

```javascript
...
clickCounter: 1,
messageForUser: undefined,

actions: {
    userButtonActionTest: function() {
    let i18n = this.get('i18n');
    let clickCounter = this.get('clickCounter');
    this.set('clickCounter', clickCounter + 1);
    this.set('messageForUser',
        i18n.t('forms.components-examples.flexberry-objectlistview.toolbar-custom-buttons-example.custom-message').string +
        ' ' + clickCounter);
    }
}
});
```

Certain methods and properties should be listed in the template list:

```hbs
{% raw %}{{flexberry-objectlistview
...
customButtons=customButtonsMethod
userButtonActionTest='userButtonActionTest'
}}
{% endraw %}
```

### Embedding custom buttons to the rows list

Custom buttons for the strings in the list are created on a similar principle. Event, as for the buttons in the toolbar can be set to a string. For example:

```javascript
...
actions: {
    userButtonInRowActionTest(model) {
      this.set('modelFromClickedRow', model);
    },
});
```

### Button custom settings

When the value of the attribute `colsConfigButton=true` on the control panel displays control buttons custom settings.

```hbs
{% raw %}{{flexberry-objectlistview
  componentName = "..."
  colsConfigButton=true
  ...
}}{% endraw %}
```

Details about the functionality of custom settings can be found in the article [Service user settings](ef_model-user-settings-service.html).

#### Add custom buttons to the control panel

On the control panel, you can add buttons that implement custom logic.
To add a custom button, perform the following steps:

1.In the relevant application [controller](ef_controller.html) to define a calculated property with an arbitrary name, for example, `сustomButtons`, which returns an array of structs of the form:

```javascript
{
 buttonName: '...',
 buttonAction: '...',
 buttonClasses: '...',
 disabled: true,
}
```

* `buttonName` - the name of the button on the user interface. If passed a null value, the button name is 'UserButton'.
* `buttonAction` - the name of the action (action), which will be called by the button (when the action is used [sendAction](http://emberjs.com/api/classes/Ember.Component.html#method_sendAction), so the handler can be defined as in [controller](ef_controller.html) and in the corresponding [the rout](ef_route.html)). If passed null, then the browser console displays error message. It is advisable to call actions with the prefix "userButtonAction" so you don't accidentally grind a property of the control when this action.
* `buttonClasses` classes that you want to add to the new custom button.
* `disabled` - Boolean property, responsible for the state of the button if `true` the button is inactive, otherwise active.

2.In the application controller (or [the rout](ef_route.html)) to define an event handler whose name was specified in `buttonAction`.

3.In the [template](ef_template.html) the appropriate form of the component `flexberry-objectlistview` to define properties:

```hbs
{% raw %}
{{flexberry-objectlistview
  // ... 
  customButtons=customButtons  
  userButtonAction1='userButtonAction1'
  userButtonAction2='userButtonAction2'
  // ... 
  userButtonActionN='userButtonActionN'
}}{% endraw %}
```

* `customButtons` - defining the property, where you can take an array.
* `userButtonAction1`, `userButtonAction2`, ... `userButtonActionN` - check action defined in a property `buttonAction` (without a "registration" action can not be called and Ember will not give error messages).

### An example of using custom buttons in the list

For example, you want to add a custom button that, when clicked, will display a message to the user.

1.In page template to set the title in a variable "header".

```hbs
{% raw %}<h3 class="ui header">Страница с пользовательскими кнопками</h3>
<div class="row">
  {{flexberry-objectlistview
    // ... 
    customButtons=customButtons
    userButtonAction1='userButtonAction1'
    userButtonAction2='userButtonAction2'
    // ... 
    userButtonActionN='userButtonActionN'
  }}
</div>
<div class="row">{{messageForUser}}</div>{% endraw %}
```

2.In the controller to determine the required variables, set the localizable property of the computable "customButtons", which will return an array of descriptions of the custom buttons (in this case, one button) and the action "userButtonActionTest", which will handle the click on the button.

```javascript
import Ember from 'ember';
import ListFormController from 'ember-flexberry/controllers/list-form';

export default ListFormController.extend({
  /** 
Property to count user clicks on a button. 

@property clickCounter 
@type Number 
@default 1 
*/
  clickCounter: 1,

  /** 
Property to show message after user click on user button. 

@property messageForUser 
@type String 
*/
  messageForUser: undefined,

  /** 
Property to form an array of special structures of custom user buttons. 

@property customButtons 
@type Array 
*/
  customButtons: Ember.computed('i18n.locale', function() {
    let i18n = this.get('i18n');
    return [{
      buttonName: i18n.t('forms.components-examples.flexberry-objectlistview.toolbar-custom-buttons-example.custom-button-name'),
      buttonAction: 'userButtonActionTest',
      buttonClasses: 'my-test-user-button test-click-button'
    }];
  }),

  actions: {
    /** 
Handler for user click on custom button. 

@method userButtonActionTest 
*/
    userButtonActionTest: function() {
      let i18n = this.get('i18n');
      let clickCounter = this.get('clickCounter');
      this.set('clickCounter', clickCounter + 1);
      this.set('messageForUser',
        i18n.t('forms.components-examples.flexberry-objectlistview.toolbar-custom-buttons-example.custom-message').string +
        ' ' + clickCounter);
    }
  }
});
```

3.In the template to specify a property to get custom buttons, and zaregistrovat a custom action.

```hbs
{% raw %}<h3>{{header}}</h3>

<div class="row">
  {{flexberry-objectlistview
    // ... 
    customButtons=customButtons
    userButtonActionTest='userButtonActionTest'
  }}
</div>{% endraw %}
```

## Tools work with objects on the lists

Button "Mark all on current page", "select all on all pages" and "Set default sort" are activated together with `checkbox` in the rows parameter `showCheckBoxInRow`.

* "Mark all on current page" - selects all objects on cranite, adds the marked objects in `slectRecords`.
* "Select all on all pages" - activates the parameter `allSeclect`, removal treatment when activated, this parameter is implemented in accordance with the requirements of a specific application in `action delete()` component.
* "Set default sort" - sets the sorting and the number of pages by default.

## Configuring a hierarchical list

If the list is hierarchical (the object has a reference to itself), the hierarchy for the list is set by default.

![](/images/pages/ABratchikova/Hierarchy folv.png)

If the hierarchy for the list you want to disable, then in the template you should register `disableHierarchicalMode = true`.

If in the model there are two or more links themselves (which might be hierarchy), then you need to specify the name of the `по which should be иерархия`: `hierarchyByAttribute = propertyName`.

## Locking individual cells in a list

On [the list](fe_object-list-view.html) there is a possibility to lock individual safe Deposit box to open the object [edit](ef_edit-form.html), while leaving active-click on the line.

For this to disable click on line you want to override the transition to the edit form using a parameter (params):

```javascript
params.goToEditForm = false;
```

Then call the `_super`.

In the processor settings click on the row there:

* the entry that was clicked
* column which is clicked (it has a property name column header on the form cellComponent this column)
* the index of the pressed column.

These settings are used to disable click handling on the line under certain conditions (i.e. when clicking on certain cells).

For example:

```javascript
actions: {
    objectListViewRowClick(record, params) {
      if (params.column && params.column.cellComponent.componentName === 'flexberry-file' && params.originalEvent.target.tagName.toLowerCase() !== 'td') {
        params.goToEditForm = false;
      }

      this._super(...arguments);
    }
```

Read more in the application code [dummy](https://github.com/Flexberry/ember-flexberry/blob/develop/tests/dummy/app/routes/components-examples/flexberry-objectlistview/downloading-files-from-olv-list.js#L45).

## Computable properties in getCellComponent

To create a computed property need to `controllers` in `getCellComponent` add property `computedProperties: { thisController: this }`:

```javascript
getCellComponent(attr, bindingPath, model) {
   let cellComponent = this._super(...arguments);
   if (attr.kind === 'belongsTo') {
     cellComponent.componentProperties = {
       choose: 'showLookupDialog',
       remove: 'removeLookupValue',
       displayAttributeName: 'name',
       required: true,
       relationName: 'author',
       projection: 'ApplicationUserL',
       autocomplete: true,
       computedProperties: { thisController: this },
       readonly: false,
      };
   }

   return cellComponent;
 },
```

Thus in the property `computedProperties` the current controller and will be `this` of [dynamic-properties](https://github.com/Flexberry/ember-flexberry/blob/develop/addon/mixins/dynamic-properties.js) with all your observer-AMI. Now to change any of the properties strimage component is sufficient to change the value in `computedProperties`:

```javascript
checkboxValue: false,

lookupReadonly: Ember.observer('checkboxValue', function() {
  if (!Ember.isNone(this.get('computedProperties.dynamicProperties.readonly'))) {
    if (this.get('checkboxValue')) {
      this.set('computedProperties.dynamicProperties.readonly', true);
    } else {
      this.set('computedProperties.dynamicProperties.readonly', false);
    }
  }

  return this.get('checkboxValue');
}),
```



{% include callout.html content="Переведено сервисом «Яндекс.Переводчик» <http://translate.yandex.ru>" type="info" %}