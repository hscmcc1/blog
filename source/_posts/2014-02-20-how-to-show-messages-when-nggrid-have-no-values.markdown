---
author: hesicong
comments: true
date: 2014-02-20 12:15:36+00:00
layout: post
slug: how-to-show-messages-when-nggrid-have-no-values
title: How to show messages when ngGrid have no values
wordpress_id: 3293
categories:
- 其他
---

ngGrid is a powerful grid system by AngularJS. But when binded data is empty, there's no options to let ngGrid show any messages inside the grid.

I googled and found an issues here:
https://github.com/angular-ui/ng-grid/issues/496
He provides us a directive for us to use. I tried that but his code is not good. I wrote my own:

``` js
angular.module("grid.ext", []).directive('showEmptyMsg', function($compile, $timeout) {
    return {
        restrict : 'A',
        link : function(scope, element, attrs) {
            var msg = (attrs.showEmptyMsg) ? attrs.showEmptyMsg : 'Nothing to display';
            var template = "<div class='no-results-msg'>" + msg + "</div>";
            var tmpl = angular.element(template);
            $compile(tmpl)(scope);
            scope.$watch('gridData', function() {
                if (_.size(scope.gridData) === 0) {
                    element.find('.ngViewport').append(tmpl);
                } else {
                    element.find(tmpl).remove();
                }
            });
        },
        scope : {
            gridData : '='
        }
    };
})
```

And you could use it like this:

``` html
<div class="gridStyle" ng-grid="gridOptions" show-empty-msg="No Results Found" grid-data="myGridData">
</div>
```