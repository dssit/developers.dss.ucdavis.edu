---
layout: post
title:  AngularJS Searchable Multi-select Dropdown Directive
date:   2015-05-04
description: A simple multi-select dropdown AngularJS directive with search functionality.
categories:
- AngularJS
- Directives
- Bootstrap-UI
permalink: angularjs-serachable-multiselect-dropdown-directive
author: obada
---

A simple multi-select dropdown **AngularJS** directive with search functionality.

___

### Purpose

In a project I am working on right now, I needed a UI element to allow the user select more than one item from a big selection. I liked how the Github widget for selecting issue labels looked. So, that was the inspiration:
![Goal Image](/assets/images/posts/angularjs-searchable-multiselect/github.png "Inspiration wiget from Github")


> Dom Manipulations should not exist in controllers, services or anywhere else but in directives.
> - Angular Best Practices


### Building the directive

Desirably, the directive should be as customizable as possible. With that in mind, these are the properties that the directive can take:

- **display-attr:** The object attribute that needs to be displayed
- **selected-items:** Items that are selected (Should be a subset of all-items)
- **all-items:** All the items that will be part of the dropdown
- **add-item:** A call-back function when clicking on an *unselected* item
- **remove-item:** A call-back function when clicking on an *selected* item

{% highlight html %}
<searchable-multiselect display-attr="name"
  selected-items="user.languages" all-items="allLanguages"
  add-item="addLanguageToUser(item,user)"
  remove-item="removeLanguageFromUser(item,user)" >
</searchable-multiselect>
{% endhighlight %}

Hence, the directive initially looks like this:

{% highlight javascript %}
app.directive("searchableMultiselect", this.searchableMultiselect = function($timeout) {
  return {
    templateUrl: 'searchableMultiselect.html',
    restrict: 'E',
    scope: {
      displayAttr: '@',
      selectedItems: '=',
      allItems: '=',
      addItem: '&',
      removeItem: '&'
    },
    link: function(scope, element, attrs) {
    }
  }
});
{% endhighlight %}

And the template is a bootstrap-ui dropdown element with a custom search input filter:

{% highlight html %}
<div class="dropdown searchable-multi-select"
dropdown on-toggle="toggled(open)">
  <a class="dropdown-toggle btn btn-default"
  data-toggle="dropdown"
  dropdown-toggle data-target="#"
  tooltip="{% raw %}{{ commaDelimitedSelected() }}{% endraw %}"
  ng-class="{'disabled': readOnly}">
    <span class="limit-ellipses"
    ng-style="{ 'width' : width }">
      <small>{% raw %}{{ commaDelimitedSelected() }}{% endraw %}</small>
      <b class="caret" ng-if="!readOnly && allItems.length"></b>
    </span>
  </a>
  <ul ng-if="!readOnly && allItems.length"
  class="dropdown-menu dropdown-menu-form form-control"
  role="menu">
    <li>
      <input type="text"
      class="form-control"
      ng-model="searchQuery"
      placeholder="Search"></input>
    </li>
    <li ng-repeat="item in ::allItems track by $index"
    ng-hide="searchQuery.length && item[displayAttr].toLowerCase().indexOf(searchQuery.toLowerCase()) < 0">
      <label class="checkbox clickable"
      ng-click="updateSelectedItems(item)"
      ng-class="{'text-success': isItemSelected(item) }">
        {% raw %} {{ item[displayAttr] }} {% endraw %}
        <span class="glyphicon glyphicon-remove pull-right clickable"
          ng-show="isItemSelected(item)"></span>
      </label>
    </li>
  </ul>
</div>
{% endhighlight %}

As you might have noticed, I used **ng-hide** instead of **ng-repeat filter**, that is because the filter redraws the DOM on everykey-stroke which is an overkill.

The widget shows a comma-delimitted list of the *selected* items or 'Nothing Selected' string, this is done using a simple scope method:

{% highlight javascript %}
scope.commaDelimitedSelected = function() {
  var list = "";
  angular.forEach(scope.selectedItems, function (item, index) {
    list += item[scope.displayAttr];
    if (index < scope.selectedItems.length - 1) list += ', ';
  });
  return list.length ? list : "Nothing Selected";
}
{% endhighlight %}

Since this string might not fit inside the widget, I also have it displayed as tooltip.

Clicking an item tiggers **updateSelectedItem()** method, which first looks if the item is selected or not, then calls the proper call-back method on the controller

{% highlight javascript %}
scope.updateSelectedItems = function(obj) {
  var selectedObj;
  for (i = 0; typeof scope.selectedItems !== 'undefined' && i < scope.selectedItems.length; i++) {
    if (scope.selectedItems[i][scope.displayAttr].toUpperCase() === obj[scope.displayAttr].toUpperCase()) {
      selectedObj = scope.selectedItems[i];
      break;
    }
  }
  if ( typeof selectedObj === 'undefined' ) {
    scope.addItem({item: obj});
  } else {
    scope.removeItem({item: selectedObj});
  }
};
{% endhighlight %}

The last scope method I needed was one that returns a boolean indicating whether a given item is selected or not. This is used to set the proper class on the widget

{% highlight javascript %}
scope.isItemSelected = function(item) {
  if ( typeof scope.selectedItems === 'undefined' ) return false;

  var tmpItem;
  for (i=0; i < scope.selectedItems.length; i++) {
    tmpItem = scope.selectedItems[i];
    if ( typeof tmpItem !== 'undefined'
    &&  typeof tmpItem[scope.displayAttr] !== 'undefined'
    &&  typeof item[scope.displayAttr] !== 'undefined'
    &&  tmpItem[scope.displayAttr].toUpperCase() === item[scope.displayAttr].toUpperCase() ) {
      return true;
    }
  }

  return false;
};
{% endhighlight %}

The end result looks like this:
![Result Image](/assets/images/posts/angularjs-searchable-multiselect/searchableMultiselect.png "Final directive result")


A full working demo is available on [Plunker][plunkerDemo]

Hope this is useful to you.

---

[plunkerDemo]: http://plnkr.co/edit/O3T7MJuwSvcG04CRvVWB
