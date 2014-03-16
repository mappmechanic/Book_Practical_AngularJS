##  AngularJS code editor using UI-Bootstrap and CodeMirror (done without using jQuery) 

I'm adding a number of [AngularJS](http://docs.angularjs.org/) views to [TeamMentor](http://blog.diniscruz.com/search/label/TeamMentor), and here is a simple HTML based source code editor inspired on the [How to Integrate Codemirror With Angular UI](http://neverstopbuilding.net/how-to-integrate-codemirror-with-angular-ui/) post and [ui-codemirror](https://github.com/angular-ui/ui-codemirror) repository.

In the end, these are the APIs I used:  


  * [http://angularjs.org/](http://angularjs.org/)
  * [http://angular-ui.github.io/bootstrap/](http://angular-ui.github.io/bootstrap/)
  * [http://twitter.github.io/bootstrap](http://twitter.github.io/bootstrap)
  * [http://codemirror.net/](http://codemirror.net/) (just the core bit)

And this is what it looks like:

[![image](images/image_thumb1.png)](http://lh6.ggpht.com/-2ed3vgaR-6g/UbsYAYxNfTI/AAAAAAAANrg/bEsGUqtjdcc/s1600-h/image%25255B2%25255D.png)

The source code editor is showing the contents of the current page (dynamically fetched using Angular **_$http.get_**) and the bottom yellow div is showing (in real-time) the contents of the source code editor:

[![image](images/image_thumb_25255B1_25255D1.png)](http://lh6.ggpht.com/-C86m0YcAMsE/UbsYCof8r5I/AAAAAAAANrw/ZgWGi5HUYu8/s1600-h/image%25255B5%25255D.png)

**What is nice about this example is that I didn't use jQuery at all!**

The great posts [http://stackoverflow.com/a/15012542](http://stackoverflow.com/a/15012542) and [AngularJS for jQuery Developers](http://blog.artlogic.com/2013/03/06/angularjs-for-jquery-developers/) explain why learning to code in Angular without JQuery is so important.

Basically, it's better not  have jQuery available, since them, the only way to solve a problem, is the 'AngularJS way' :)

**How it works:**  

Here is a brief explanation of the code behind this PoC (included in full at the end of this page):

First we load the **Javascript** and **CSS:**  

[![image](images/image_thumb_25255B2_25255D1.png)](http://lh4.ggpht.com/-tEcuVEP1D80/UbsYEoXOcjI/AAAAAAAANsA/HWHRdWIOT18/s1600-h/image%25255B8%25255D.png)

... them set up **_AngularJS_** module and **_codeMirror_** value

[![image](images/image_thumb_25255B3_25255D1.png)](http://lh3.ggpht.com/-lyj5LUqcA94/UbsYGN5zcsI/AAAAAAAANsM/AgHTQeAAkp8/s1600-h/image%25255B11%25255D.png)

... use a **_controller_** to get the code to show (using **_$http.get_**) and assign it to the the**_ $scope.code_** variable

[![image](images/image_thumb_25255B4_25255D1.png)](http://lh3.ggpht.com/-b-GAtysPY7M/UbsYJdayGQI/AAAAAAAANsg/m5lJUWFHO-A/s1600-h/image%25255B14%25255D.png)

... configure **angularJS** in the HTML by setting the **_textarea_** element to be a **_codemirror_** (linked to the **$scope.code **model)

[![image](images/image_thumb_25255B5_25255D1.png)](http://lh5.ggpht.com/-J3XesF0TAmA/UbsYK4HOBII/AAAAAAAANsw/ZsquvRAHFtg/s1600-h/image%25255B17%25255D.png)

... finally show the current value of **$scope.code**  in side an bootstrap **_alert_** element

[![image](images/image_thumb_25255B7_25255D1.png)](http://lh4.ggpht.com/-aDNK4MBsNAE/UbsYMt2KWiI/AAAAAAAANtA/BEybxPt6wEk/s1600-h/image%25255B23%25255D.png)

  
**Complete Source code:**  
    
    <!DOCTYPE html>
    
    <html>  
        <head>  
            <title>CodeMirror with AngularJS</title>  
            <link href="/Content/bootstrap.min.css" rel="stylesheet">  
            <script src="/Scripts/angular.min.js" type="text/javascript"></script>  
            <script src="/Scripts/angular-ui.js" type="text/javascript"></script>  
            <script src="/Scripts/ui-bootstrap-tpls-0.3.0.min.js" type="text/javascript"></script>

            <link href="/Content/codemirror-3.01/codemirror.css" rel="stylesheet"/>  
            <link href="/Content/codemirror-3.01/theme/rubyblue.css" rel="stylesheet"/>  
            <script src="/Scripts/codemirror-3.01/codemirror.js" type="text/javascript"></script>  
            <script src="/Scripts/codemirror-3.01/mode/javascript.js" type="text/javascript"></script>

  
            <script type="text/javascript">  
                var myApp = angular.module('myApp', ['ui', 'ui.bootstrap']);
    
                myApp.value('ui.config',  
                    {  
                        codemirror:  
                            {  
                                mode: 'javascript',  
                                lineNumbers: true,  
                                matchBrackets: true,  
                                theme: 'rubyblue'  
                            }  
                    });

                function codeCtrl($scope,$http)  
                {  
                    $scope.docLocation = document.location.href;  
                    $http.get($scope.docLocation)  
                         .success(function (data)  
                            {  
                                $scope.code = data;  
                            });  
                    //$scope.code = "var a = 'somecode'; \n//which also shows above</h1>";  
                }  
            </script>  
        </head>  
    <body ng-app="myApp">  
        <div class="well well-large">  
            <div class="container">  
                <h2>CodeMirror working with AngularJS and Bootstrap</h2></div>  
            </div>  
            <div ng-controller="codeCtrl">  
                <div class="container">

                <h4>Code Editor:</h4>  
                <p>With the the contents of this page (i.e.: {{docLocation}} )</p>

                <textarea ui-codemirror ng-model="code"></textarea>

                <br/><hr/><br/>

                <h4>Bootstrap alert style</h4>  
                <p>  
                    Showing in real time the contents of the code shown above (make a change to try it)  
                </p>  
                <alert type="success">{{code}}</alert>  
            </div>  
        </div>  
    </body>  
</html>  

