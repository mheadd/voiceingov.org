---
id: 23
title: Voice Tax Calculator
date: 2005-06-19T17:14:28+00:00
author: mheadd
layout: page
guid: http://www.voiceingov.org/blog/?page_id=23
original_post_id:
  - "23"
---
**Tax Returns: Accuracy Matters!**

Every year, as taxpayers begin the annual rite of filling out tax forms, governments struggle with ways to streamline the processing and improve the accuracy of tax returns. Tax returns that contain errors are bad for citizens &#8211; it can mean the difference between getting a big, or not so big tax refund. It can also dramatically impact how fast refunds are issued. Inaccurate returns are also bad for governments &#8211; inaccurate tax returns require more resources to process, particularly if those returns are filed the old fashioned way: on paper.

To be sure, the Internet era has ushered in a host of different options for taxpayers that can help governments better ensure the accuracy of tax return data. Online tax filing, which is [growing in popularity](http://www.zwire.com/site/news.cfm?newsid=7970440&BRD=1643&PAG=461&dept_id=10486&rfi=6), can automatically correct mistakes on tax returns and automatically calculate a tax liability &#8211; dramatically reducing the chance for errors. In addition, software tools used by professional tax preparers can generate error free returns quickly and easily.

However, concerns about online filing [continue to linger](http://www.gcn.com/vol1_no1/daily-updates/22679-1.html). Governments need to have multi-tiered strategies to enable the submission of accurate tax returns, even for those who still insist on filing paper returns. By using VoiceXML and JavaScript, governments can provide one such mechanism in a way that is simple, cheap and easy to deploy.

**A Simple VoiceXML Tax Calculator**

Building a simple tax calculator that is available through any standard telephone is one way that governments can help ensure greater accuracy on tax returns. Who hasn&#8217;t gotten to the end of their tax return at one time or another and thought &#8211; &#8220;Gee, did I calculate that right?&#8221;

**JavaScript Code**

To begin, we&#8217;ll need to assemble some JavaScript code to do a tax calculation. [Most states](http://www.taxadmin.org/fta/rate/ind_inc.html) and the federal government use graduated tax rates to calculate a tax liability. In addition, pretax deductions (which reduce taxable income) and post-tax credits (which provide a dollar for dollar reduction in tax liability) are common features. As such, we&#8217;ll use the State of Delaware in this example since all of these features are used for Delaware tax purposes. However, we&#8217;ll take care in building the &#8220;guts&#8221; of our voice tax calculator to make it easy to modify for other governments that have a similar tax structure.

To begin with, lets declare some variables that we&#8217;ll use to calculate a tax liability. We&#8217;ll do this using the JavaScript keyword &#8220;var&#8221;:

<pre>/* A variable to hold our AGI amount */
var agi_amt;

/* A variable to hold our deduction amount */
var deduct_amt;

/* A variable to hold the number of post-tax credits */
var credit_num;

/* These variables will hold some interim tax calculations.
We'll initialize them now by setting their value equal to zero */

var amt_1=0;
var amt_2=0;
var amt_3=0;
var amt_4=0;
var amt_5=0;
var amt_6=0;
var amt_7=0;

/* Set income tax brackets */
var bracket_1_low = 0;
var bracket_1_high = 2000;<br />
var bracket_2_low = 2001;<br />
var bracket_2_high = 5000;<br />
var bracket_3_low = 5001;<br />
var bracket_3_high = 10000;<br />
var bracket_4_low = 10001;<br />
var bracket_4_high = 20000;<br />
var bracket_5_low = 20001;<br />
var bracket_5_high = 25000;<br />
var bracket_6_low = 25001;<br />
var bracket_6_high = 60000;<br />
var bracket_7 = 60001;<br />

/* Declare income tax rates */<br />
var rate_1 = 0;<br />
var rate_2 = .022;<br />
var rate_3 = .039;<br />
var rate_4 = .048;<br />
var rate_5 = .052;<br />
var rate_6 = .055;<br />
var rate_7 = .0595;<br />

/* Declare personal credit and standard deduction amounts */
var pers_credit = 110;<br />
var standard_deduct = 3250;<br />
</pre>

OK &#8211; we now have all of the pieces that we&#8217;ll need to calculate a tax liability. To carry out this calculation, we&#8217;ll use a [JavaScript function](http://www.juicystudio.com/tutorial/javascript/functions.asp) that we can call rom the appropriate part of our VoiceXML document (we haven&#8217;t built that part yet, but we&#8217;ll get there shortly). We do this using the JavaScript &#8220;function&#8221; keyword.

<pre>/* Tax Calculation Function<br />
a = adjusted gross income; b = deduction amount; c = number of
personal credits. */<br />
function liability (a, b, c) {<br />
<br />
/* Deduction amount must be at least as much as standard
deduction. */<br />

if (b&gt;standard_deduct) { var deduct_amt=b; } else { var
deduct_amt=standard_deduct; }<br />

<br />
/*At least one personal credit must be taken*/<br />
if (c&lt;1) { var num_cred=1; } else { var num_cred=c; }  <br />
<br />
var tax_inc = (a-deduct_amt);<br />
if (tax_inc&gt;bracket_1_high) {
amt_1=((bracket_1_high-bracket_1_low)*rate_1); } else if
(tax_inc&gt;=bracket_1_low) {
amt_1=((tax_inc-bracket_1_low)*rate_1); } else { amt_1=0; }<br />

<br />
if (tax_inc&gt;bracket_2_high) {
amt_2=((bracket_2_high-bracket_2_low)*rate_2); } else if
(tax_inc&gt;=bracket_2_low) {
amt_2=((tax_inc-bracket_2_low)*rate_2); } else { amt_2=0; }<br />

<br />
if (tax_inc&gt;bracket_3_high) {
amt_3=((bracket_3_high-bracket_3_low)*rate_3); } else if
(tax_inc&gt;=bracket_3_low) {
amt_3=((tax_inc-bracket_3_low)*rate_3); } else { amt_3=0; }<br />
<br />
if (tax_inc&gt;bracket_4_high) {
amt_4=((bracket_4_high-bracket_4_low)*rate_4); } else if
(tax_inc&gt;=bracket_4_low) {
amt_4=((tax_inc-bracket_4_low)*rate_4); } else { amt_4=0; }<br />
<br />

if (tax_inc&gt;bracket_5_high) {
amt_5=((bracket_5_high-bracket_5_low)*rate_5); } else if
(tax_inc&gt;=bracket_5_low) {
amt_5=((tax_inc-bracket_5_low)*rate_5); } else { amt_5=0; }<br />
<br />
if (tax_inc&gt;bracket_6_high) {
amt_6=((bracket_6_high-bracket_6_low)*rate_6); } else if
(tax_inc&gt;=bracket_6_low) {
amt_6=((tax_inc-bracket_6_low)*rate_6); } else { amt_6=0; }<br />

<br />
if (tax_inc&gt;bracket_7) { amt_7=((tax_inc-bracket_7)*rate_7);}
else { amt_7=0; }<br />
<br />
var tax_amt = amt_1+amt_2+amt_3+amt_4+amt_5+amt_6+amt_7;<br />

var tax_liab = tax_amt-(num_cred*pers_credit);<br />
<br />
/* Tax liability can not be lower than zero */<br />
if (tax_liab &gt; 0) {<br />
return Math.round(tax_liab);<br />

}<br />
else { return 0;<br />
}</pre>

Although this looks complicated, the tax calculation is really very straightforward. There are probably more efficient ways of writing this function, but doing it this way makes the tax calculation more transparent &#8211; for you, and for future developers who might modify or build on your work.

Our function requires three parameters &#8211; defined generically as **a**, **b** and **c**. These values are passed to the function as &#8220;arguments&#8221; that are collected from the user in different parts of the VoiceXML application. These values will be used to calculate the tax liability. We first check to make sure that the deduction amount that is entered by the user is larger than the standard deduction amount. If it isn&#8217;t, we&#8217;ll assign the value of the standard deduction. This helps make sure that the user is not claiming less than the standard deduction amount &#8211; a common mistake on self prepared returns.

Similarly, we&#8217;ll make sure that at least one personal credit is taken. Every taxpayer gets at least one credit for themselves, and more if they can claim others as dependents. To avoid a common mistake, we&#8217;ll make sure that each user gets at least one credit. Next, we&#8217;ll use our deduction amount to calculate taxable income. This is the amount of income that is run through our rate structure.

Graduated tax structures apply a specified rate to a given range of income. The final tax liability is the sum of the amounts generated from applying the different rates to these &#8220;tax brackets.&#8221; This might help clarify why we declared some variables earlier to hold our interim calculations (i.e., amt\_1, amt\_2, etc.).

The interim liability for each tax bracket is equal to the the amount of taxable income in that bracket multiplied by the rate for that bracket. Employing an &#8220;If / Else&#8221; control structure, our JavaScript function uses the taxable income amount that we calculated and first checks to see if it is larger than the high end of the first bracket. If it is, then the tax amount for that bracket is equal to the high end of the bracket minus the low end of the bracket multiplied by that bracket&#8217;s rate. It assigns this value to our temporary variable amt_1. We then move on to the next bracket and repeat this exercise.

<pre>&lt;u>For example&lt;/u>:<br />
Interim amount = (High end of the bracket - low end of the
bracket) * bracket rate<br />
or<br />
amt_1=((bracket_1_high-bracket_1_low)*rate_1)
</pre>

In practice, we will reach a point where the amount of taxable income is exceeded by the high end of one of our brackets. When we reach that point, we want to take our taxable income amount and subtract the low end of the bracket. We then apply the final tax rate and sum up all of our temporary variables (i.e., amt\_1 through amt\_7, one for each of our tax brackets). This is the unadjusted tax liability amount.

Next we reduce our tax liability by the value of our personal credits. We make sure that the amount we calculate is not less than zero and then we round the result. Note &#8211; our check to ensure that the final liability is greater than zero is a function of our example state&#8217;s tax structure. Some states (and the federal government) use &#8220;refundable&#8221; credits which can actually result in a negative tax liability, which means that the taxpayer can get a check for the overage amount. A notable example of a refundable credit used by some states and the federal government is the [Earned Income Tax Credit](http://www.irs.gov/individuals/article/0,,id=96456,00.html).

**VoiceXML Code**

Blocks of JavaScript can be contained within VoiceXML documents using the <script> element. This isn&#8217;t the only place you can use JavaScript in a VoiceXML document (a number of VoiceXML elements can utilize JavaScript expressions through the &#8220;expr&#8221; attribute &#8211; as demonstrated below), but if you are reusing functions or variables in multiple parts of your document, its a good idea to keep your code self contained. Because several of the operators in JavaScript (<, >, &, &&) have special significance in XML, we&#8217;ll enclose our script in a CDATA section, like so:

<pre>&lt;script&gt;<br />

&lt;![CDATA[<br />
... JavaScript goes here ...<br />

]]&gt;<br />
&lt;/script&gt;
</pre>

Next we&#8217;ll build some VoiceXML dialogs to collect the necessary information from the caller to be used in our JavaScript tax calculator.

In a nutshell, we want to create dialogs to collect three items: the adjusted gross income amount, the deduction amount and the number of personal credits. Each of these values can be mapped to a specific line on a tax return, so it is relatively simple to create these parts of our VoiceXML document. We&#8217;ll just ask the user to enter the number on a specific line of the return. We also want to provide a dialog to read back the values that are entered by the user. This will help us avoid confusing taxpayers with inaccurate results from misinterpreted input.

After we have collected and verified the necessary information, we need to pass these values to our JavaScript function so that it can calculate our tax liability. We do this by &#8220;calling&#8221; our function from within our VoiceXML document and passing it the arguments it needs to return a tax calculation, like so:

<pre>&lt;prompt&gt;<br />
&lt;value expr="liability(agi_amt, deduct_amt, credit_num)"/&gt;<br />

&lt;/prompt&gt;
</pre>

We call our function by name &#8211; we previously named it &#8220;liability&#8221;, the word that appears immediately after the function keyword above &#8211; and include the arguments in parenthesis separated by commas. You may now better understand why the last statement or two in our function include the word &#8220;return&#8221; &#8211; we have provided our JavaScript function with some values and are asking it to return a calculation.

By inserting the <value> element between the <prompt></prompt> tags, we are telling the VoiceXML interpreter to speak the result of the function call. We use the &#8220;expr&#8221; attribute to assign the amount returned from the function to the <value> element. If the amount returned is, for example, $400 the result would be the same as if we put the words &#8220;four hundred&#8221; between the <prompt></prompt> tags, like so:

<pre>&lt;prompt&gt;<br />
Four hundred<br />
&lt;/prompt&gt;</pre>

Finally, we&#8217;ll offer a simple dialog for taxpayers that run into trouble. Users can access this section by uttering the word &#8220;help&#8221; at selected points in the application.

The completed VoiceXML/JavaScript tax checker application [can be obtained here](../xfiles/tutorials/tax_calc.zip). Note &#8211; this application is optimized to work on the BeVocal platform, but it could easily be modified to work on other VoiceXML implimentations.

**Conclusion**

By offering multiple options to taxpayers to better enable them to submit accurate tax returns, governments can cut processing costs, speed refunds and provide better customer service. Adding voice applications to the mix &#8211; either through the use of a simple, highly available tax checking application like the one discussed here, or through a full blown telephone tax filing application &#8211; can help achieve this goal.

<!-- Creative Commons License -->

<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/2.5/"><img alt="Creative Commons License" border="0" src="http://creativecommons.org/images/public/somerights20.gif" /></a>
  
This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/2.5/">Creative Commons Attribution-NonCommercial-ShareAlike 2.5 License</a>.

<!-- /Creative Commons License -->