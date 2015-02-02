# directiveslib

Custom angularjs directive for common use

## Check Unique
```
/*
* Use this directive to validate a unique value from server
* Usage: check-unique="/url/to/check/unique?name={{model}}" ignore-unique-value="{{model}}"
* Error: <label ng-show="createForm.name.$error.unique && !createForm.name.$error.required" class="error">Program name already exists, choose a different name</label>
* Note: use ingore attribute on modify page to ignore the current valid value
 */
.directive('checkUnique', ['$http', function($http) {
    return {
        require: 'ngModel',
        link: function(scope, ele, attrs, c) {
            var origonalVal = attrs.ignoreUniqueValue;

            scope.$watch(attrs.ngModel, function() {
                var postToUrl = attrs.checkUnique;
                if($.trim(ele.val()) == $.trim(origonalVal) && origonalVal != ''){
                    c.$setValidity('unique', true);
                    return;
                }

                $http({
                    method: 'GET',
                    url: postToUrl
                }).success(function(isUnique,status,headers,cfg) {
                    var iu = isUnique == 'true' ? true : false;
                    c.$setValidity('unique', iu);
                }).error(function(data,status,headers,cfg) {
                    c.$setValidity('unique', false);
                });
            });
        }
    }
}])
```
