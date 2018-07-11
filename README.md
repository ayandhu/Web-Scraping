
# GMI Script Development Guidelnies

Please follow the script development guidelines below when you're developing,

## Logging 

Add proper logging in as much relevant places as possible. This will help debug the issues if the script downloads fail in the future.  Do not repeat the same log statements in more than one place, i.e log statements should be unique with relevant information enough to pin point to a specific line in the script, For example, in many scripts, the following log statement is present

      Line 25 : console.log('URL after login failure: ' + this.getCurrentUrl());

      Line 50 :  console.log('URL after login failure: ' + this.getCurrentUrl());

      Line 75 : console.log('URL after login failure: ' + this.getCurrentUrl());

The logs above won’t help much in debugging and only create confusion, since we don’t really know which line the logs are from.

Better way to add logs would be in the following way,

    Failure logs :
       Method Name :: Operation :: Cause of failure :: Other reasons if any

    Checkpoint logs :
       Method Name :: Operation :: Checkpoint name :: any additional info

This way, if the script fails in future, we can easily pinpoint, or even find the solution to the problem by just looking through the logs.

It is also not necessary to add logs in less significant places in the script to cause unnecessary attention. 


## Comments
Proper comments must be added in relevant places with enough information for others to understand. This will help reduce the time it takes the next developer to understand and debug your code easily. 

#### Example 1
``` php

     /**
	 * Set key/pair value in session storage
	 *
	 * @param	string	$key
	 * @param	string	$value
	 * @return	mixed
	 */
	public function setSessionStorage($key, $value)
```


#### Example 2
``` php
// Enable this only after successfull login, otherwise no need to process restart
$this->support_restart = true;

```




## No Hardcoding of URLs

* Avoid hardcoding URLs within multiple places in the script. It is better to keep those URLs in a constant.
* Although, it’s usually easy and preferable to store less complex URLs in a constant, prefer to reach to a page, Invoice page for example, by navigating through links and clicking on menus, than by keeping the long URLs in a constant. 
URLs that seem like they have dynamic content in them, should never be hard coded. Although they might work at the moment, they will eventually break in future,


        Okay to store in constant : http://join.me/invoices/past

        Not okay to store in constant : http://join.me/current_user/invoices/jsession_id=348jkj34ui



## Choosing selectors

Try to maintain all page selectors, at least the recurring ones in a constant. This way, if the site changes, very minimal change will get the script back to working.


When choosing selectors, remember the following, 

  The site may update it’s DOM anytime soon which will cause our script to break, so make your
 selector as reliable and flexible and at the same time specific enough as possible. 
For example, when inspecting an anchor tag for logout link with following markup, 
``` html
<a href=”/logout.php?itm=0127897343” class=”logoutlink”/>Abmelden</a>
```

   At first glance, the following selector seems appropriate,
``` javascript 
document.querySelector(“a.logoutlink[href=’/logout.php?itm=0127897343’]”)
```
But it’s a bad practice and the script is prone to break even without the site updating it’s DOM.
A better css selector, among many others, would be 
```javascript
document.querySelector(“a[href*=’/logout.php’]”)
```
As much as flexibility, the selector you chose must also be specific enough to only target the right elements. 
            Identify and ignore the dynamic/unreliable part of the selectors as much as possible. 
Selectors like “table tbody > tr.row” should be avoided. Unless there is no other unique identifier,
 do not prefer such generic selectors as they’re not reliable.


## Error Handling
   When parsing strings, dates or performing DOM operations, always have a fallback mechanism that allows the script to continue with the next action without terminating. Use of try/catch blocks to parse and handle unexpected dates is one such way. You should also use try/catch blocks while iterating through a list of objects, this way, if a single object has errors, you can skip to the next object with proper logging.


## Function Responsibilities
Do not clutter a single function with multiple scattered responsibilities. For example, waitForLogin function must be limited to waiting for login to be successful and delegate remaining functionalities to a different method. It cannot have code that does a different task altogether.  This way, the code will be readable and easy to maintain.

## Misc

When comparing strings in your script, prefer robust comparison methods such as regex pattern matchers, instead of using == comparisons,
 for example, following regex can be used for eliminating special characters from number, instead of relying on string comparisons with relational operators. 
``` javascript    
var invoiceAmount = td.innerText.replace(/[^\d.,]/g).trim();
```


    


