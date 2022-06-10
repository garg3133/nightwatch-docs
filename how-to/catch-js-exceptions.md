<div class="page-header"><h2>Catch JS Exceptions</h2></div>

### Overview
Sometimes, during the run-time of your website or web-application, some JS expections might arise due to some bug in the code, which is usually hard to catch unless you are actively monitoring the DevTools console of your browser while testing your website, where all the JS exceptions gets logged.

To ease this process, Nightwatch now allows you to capture the JS exceptions happening during the run-time of your website and get them to you in your Nightwatch test run itself. This is made possible with the support of Chrome DevTools Protocol now available in Selenium 4.

<div class="alert alert-info">
  This command only works with Chromium based browsers such as Google Chrome and Microsoft Egde.
</div>

### Catch JS exceptions

This command allows you to capture all the JS exceptions happening during the test-run of your website, and send them back to you as an argument in the provided callback, without the need to inspect the DevTools console of the browser during the test run.

All you need to do is call the `browser.catchJsExceptions()` command with the required parameters before navigating to your website.

`catchJsExceptions()` accepts a callback function, which will receive an `event` object as an argument whenever a new console message is logged. The specifications of the received `event` object are as follows:

<table class="table table-bordered table-striped">
  <thead>
   <tr>
     <th style="width: 100px;">Name</th>
     <th style="width: 100px;">type</th>
     <th>description</th>
   </tr>
  </thead>
  <tbody>
    <tr>
      <td>`timestamp`</td>
      <td>number</td>
      <td>Time at which the JS expection was captured.</td>
    </tr>    
    <tr>
      <td>`exceptionDetails`<br></td>
      <td>object</td>
      <td>A JS object with all the details of the exception occured.<br>Specifications of the object can be read from [here](https://chromedevtools.github.io/devtools-protocol/tot/Runtime/#type-ExceptionDetails).</td>
    </tr>
  </tbody>
</table>

#### Example:

<div class="sample-test"><i>tests/catch-js-exceptions.js</i>
<pre class="line-numbers language-javascript"><code class="language-javascript">
describe('Catch JS exceptions', async function() {
  await browser.catchJsExceptions((event) => {
    console.log('Exception:', event);
  });
  await browser.navigateTo('https://www.google.com');
  const aboutLink = await browser.element('link text', 'About');
  await browser.executeScript(function(aboutLink) {
    aboutLink.setAttribute('onclick', 'throw new Error("Hello world!")');
  }, [aboutLink]);
  await browser.click(aboutLink);
});
</code></pre></div>

Output of the example above:

```
  Running Catch JS exceptions:
───────────────────────────────────────────────────────────────────────────────────────────────────
{
  exceptionDetails: {
    exceptionId: 1,
    text: 'Uncaught',
    lineNumber: 0,
    columnNumber: 6,
    scriptId: '55',
    url: 'https://www.google.com/',
    stackTrace: { callFrames: [Array] },
    exception: {
      type: 'object',
      subtype: 'error',
      className: 'Error',
      description: 'Error: Hello world!\n' +
        '    at HTMLAnchorElement.onclick (https://www.google.com/:1:7)',
      objectId: '6711588812373266697.1.1',
      preview: [Object]
    },
    executionContextId: 1
  },
  timestamp: 2022-06-10T13:14:52.722Z
}
No assertions ran.
```
