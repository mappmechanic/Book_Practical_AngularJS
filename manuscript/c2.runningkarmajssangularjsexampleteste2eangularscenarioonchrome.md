##  Running KarmaJS's AngularJS example test/e2e/angular-scenario (on Chrome) 

To learn and get an idea of how [Karma](http://karma-runner.github.io/0.8/index.html) (the _'Spectacular Test Runner for JavaScript'_) works, and how it can be used to create browser automations tests, here are the steps I took to get the [test/e2e/angular-scenario](https://github.com/karma-runner/karma/tree/master/test/e2e/angular-scenario) example to work.

It all started with a clone of: [git@github.com:karma-runner/karma.git](mailto:git@github.com:karma-runner/karma.git)  
  
[![image](images/image_thumb1.png)](http://lh6.ggpht.com/-Su0zh4YfFKQ/Ub8MwggUbUI/AAAAAAAAN-w/wdpY1GnT1Mo/s1600-h/image%25255B2%25255D.png)

With this KarmaJS test example (see below), being the one that we are going to use:

[![image](images/image_thumb_25255B1_25255D1.png)](http://lh5.ggpht.com/-CpJ4aM01XIk/Ub8MyM192yI/AAAAAAAAN_A/gjXbkFMGGuM/s1600-h/image%25255B5%25255D.png)

I then opened an nodejs command prompt and navigated to the folder shown above:

[![image](images/image_thumb_25255B2_25255D1.png)](http://lh5.ggpht.com/--sFgDmhkGqc/Ub8Mzv7j1mI/AAAAAAAAN_Q/HJSFWoiZBjI/s1600-h/image%25255B8%25255D.png)

... used **_node server.js _**to start a local webserver

[![image](images/image_thumb_25255B3_25255D1.png)](http://lh6.ggpht.com/-DzTU1R-VIBo/Ub8M1TxNFmI/AAAAAAAAN_g/Se4kHQ1sHzA/s1600-h/image%25255B11%25255D.png)

... on port 8000:

[![image](images/image_thumb_25255B4_25255D1.png)](http://lh3.ggpht.com/-0hhEE4RZqLE/Ub8M3C_PfkI/AAAAAAAAN_w/H4qlQw3okT8/s1600-h/image%25255B14%25255D.png)

The test case we are using (on KarmaJS's **_test/e2e/angular-scenario_**) is a simple AngularJS example, which just consumes the angular-min.js file model attribute:

[![image](images/image_thumb_25255B11_25255D.png)](http://lh6.ggpht.com/-Ox_6hAM76wk/Ub8M5GKWoeI/AAAAAAAAOAA/UJIhBdEqsu4/s1600-h/image%25255B29%25255D.png)

... and uses angular to populate the **_{{yourName}}_** value dynamically (wired to the input field via the **_ng-model="yourName"_**)

[![image](images/image_thumb_25255B10_25255D1.png)](http://lh5.ggpht.com/-BAz-d5U_tTI/Ub8M6t_LKdI/AAAAAAAAOAQ/EsRFbeeY_TU/s1600-h/image%25255B28%25255D.png)

Next we are going to run this [Jasmine](http://pivotal.github.io/jasmine/) ('_Behavior-**D**riven **D**evelopment framework for testing JavaScript code'_) test using KarmaJS

[![image](images/image_thumb_25255B13_25255D.png)](http://lh5.ggpht.com/-XYKofQfHiw0/Ub8M8re3XBI/AAAAAAAAOAg/jZeMTmiqFdk/s1600-h/image%25255B35%25255D.png)

... which _should_ work with just: **karma start karma.conf.js**  

[![image](images/image_thumb_25255B12_25255D.png)](http://lh4.ggpht.com/-NXG2elPLdYQ/Ub8M-qumTXI/AAAAAAAAOAw/eS9ba0TN6Hs/s1600-h/image%25255B32%25255D.png)

... but it didn't

There is a module dependency missing, which in this case can be resolved by running this command from the root of the karma repository:

[![image](images/image_thumb_25255B14_25255D.png)](http://lh4.ggpht.com/-l0rpvw2OZuM/Ub8NAufq0NI/AAAAAAAAOBA/c1F1ltaZcLs/s1600-h/image%25255B38%25255D.png)

__UPDATE: the issue above was caused by the fact that I had an the official released version of karma installed globally which is the one that was running when I tried it on the 'test/e2e/angular-scenario' folder'__

And now (based on an option from the karma.conf.js) a Chrome window opened up:

[![image](images/image_thumb_25255B17_25255D1.png)](http://lh5.ggpht.com/-yUvFhlpBFtk/Ub8NC1XyQhI/AAAAAAAAOBQ/0MrOqpoKo_Q/s1600-h/image%25255B47%25255D.png)

Clicking on the **_Debug_** button:

[![image](images/image_thumb_25255B18_25255D1.png)](http://lh5.ggpht.com/-OgRKksx-X_U/Ub8NEWxM2zI/AAAAAAAAOBg/ga_XmdUtmFc/s1600-h/image%25255B50%25255D.png)

... we can more details on the test that failed:

[![image](images/image_thumb_25255B19_25255D.png)](http://lh3.ggpht.com/-qRulQPjwhjQ/Ub8NGJjzE7I/AAAAAAAAOBw/Himm3X0Zq4w/s1600-h/image%25255B53%25255D.png)

In this case the problem is that the 'proxy mapping' that karma does is not correct

If we look at the karma.config.js file

[![image](images/image_thumb_25255B20_25255D1.png)](http://lh4.ggpht.com/-PU_L5SrCqSs/Ub8NH5UF3QI/AAAAAAAAOCA/AdZ81gW_Vgo/s1600-h/image%25255B56%25255D.png)

.... and the unit test **_brower().navigateTo_** command

[![image](images/image_thumb_25255B21_25255D1.png)](http://lh6.ggpht.com/-G2EZ8QC2CoI/Ub8NK45_WDI/AAAAAAAAOCQ/xDlefWvR7-U/s1600-h/image%25255B59%25255D.png)

... we can see that karma will try to open **_/index.html_** from _**http://localhost:8000/test/e2e/angular-scenario/index.html**_

That is not going to work since the page we want it is on [http://localhost:8000/index.html](http://localhost:8000/index.html) (which happened because we started **_node server.js _**on the** .../test/e2e/angular-scenario **folder)

[![image](images/image_thumb_25255B22_25255D1.png)](http://lh4.ggpht.com/-5EyFv12Jiuk/Ub8NNRQHogI/AAAAAAAAOCg/xfgBZsXJCWw/s1600-h/image%25255B62%25255D.png)

One way to solved this, is to change the 'proxies' mapping to **_'/' : 'http://localhost:8000/'_**  
**_  
_**[![image](images/image_thumb_25255B23_25255D1.png)](http://lh3.ggpht.com/-82nTLnUjM9k/Ub8NO7ZQ5eI/AAAAAAAAOCw/QqEpcTCbbgA/s1600-h/image%25255B65%25255D.png)

... and after stopping and starting the karma server:

[![image](images/image_thumb_25255B24_25255D.png)](http://lh6.ggpht.com/-ZC62qBcvW24/Ub8NQRX993I/AAAAAAAAODA/M1J_9bEH7CM/s1600-h/image%25255B68%25255D.png)

all tests pass :)

[![image](images/image_thumb_25255B25_25255D1.png)](http://lh6.ggpht.com/-8-fsEC2rI6M/Ub8NR3qXnqI/AAAAAAAAODQ/yimGbonZqnk/s1600-h/image%25255B71%25255D.png)   

**Changing and executing tests in real time**  

What is really cool with this set-up is that (as long as the karma process is running), because the **autoWatch** value (in k_**arma.config.j**_s) was set to true, if I make a change to the test file:

[![image](images/image_thumb_25255B26_25255D.png)](http://lh4.ggpht.com/-FXo-OsOejKk/Ub8NUKfMyPI/AAAAAAAAODg/5hHFD8UcU0w/s1600-h/image%25255B74%25255D.png)

... the karma runner will detect the changes and rerun the tests (note that there are 2 tests executed now)

[![image](images/image_thumb_25255B27_25255D1.png)](http://lh4.ggpht.com/-MJOGXw9-0Zw/Ub8NWPXgYGI/AAAAAAAAODw/z0WZYZ9djRk/s1600-h/image%25255B77%25255D.png)

This 2nd test shows an interesting behaviors since it will make the test wait for 15 seconds (with the browser window opened):

[![image](images/image_thumb_25255B28_25255D.png)](http://lh5.ggpht.com/-xG6KcdbXRbQ/Ub8NYLxBbkI/AAAAAAAAOEA/zeNx7EkMnpg/s1600-h/image%25255B80%25255D.png) 

Note how the time execution time for the 2nd test was ~15 secs

[![image](images/image_thumb_25255B29_25255D.png)](http://lh6.ggpht.com/-F4B1f47KAAE/Ub8NbircN3I/AAAAAAAAOEQ/IIl9uEton6U/s1600-h/image%25255B83%25255D.png)

And if we modify the 2nd test to:

[![image](images/image_thumb_25255B31_25255D.png)](http://lh3.ggpht.com/-gkDEc4WGDes/Ub8NdrPhFtI/AAAAAAAAOEg/PFtlSMVfgSI/s1600-h/image%25255B89%25255D.png)

... the execution test UI will look like this (note that the execution was triggered when I saved the test file :)

[![image](images/image_thumb_25255B30_25255D.png)](http://lh4.ggpht.com/-x4k0MDozSsM/Ub8NfUa7O4I/AAAAAAAAOEw/HuXUH2ZSw68/s1600-h/image%25255B86%25255D.png)

**NOTE 1: **to solve the ENOENT error shown the first screenshot of localhost:8000, we just needed to a favicon.ico file to the **lib/nodeserver **folder

[![image](images/image_thumb_25255B6_25255D1.png)](http://lh4.ggpht.com/-uVb20k9Os0g/Ub8NhXGHylI/AAAAAAAAOFA/CNolCGRIws0/s1600-h/image%25255B20%25255D.png)

... and now the error doesn't happen anymore

[![image](images/image_thumb_25255B5_25255D1.png)](http://lh4.ggpht.com/-E_LS0fXHFAk/Ub8NjF5cXTI/AAAAAAAAOFU/1K08_x-gmYc/s1600-h/image%25255B17%25255D.png)   

**Note 2: **when trying to run Karma for the first time, I had a prob with grunt where it was failing with an **_Error: spawn ENOENT_**:

[![image](images/image_thumb_25255B33_25255D.png)](http://lh5.ggpht.com/-u5XxRHPIw_k/Ub8Nk2hG9CI/AAAAAAAAOFk/XNqHwo61gNM/s1600-h/image%25255B95%25255D.png)

... this was resolved by installed the 32bit version of nodeJS and running **_npm install_** on the karma folder (after cloning it)

[![image](images/image_thumb_25255B32_25255D.png)](http://lh5.ggpht.com/-54uYSEboBII/Ub8Nmnsv3gI/AAAAAAAAOF0/xNXP8s-JjB4/s1600-h/image%25255B92%25255D.png)
