<div class="page-header"><h2>Test on different device dimensions</h2></div>

### Overview

As the variety of devices users use to access the internet are increasing day-by-day and there is an overall shift from desktop devices to hand-held devices at a much faster rate, it becomes really important to ensure that your website or web-application looks and works as expected across all devices with varying screen sizes that people might use to access it.

Almost everyone who build websites must have used DevTools feature of the browsers at some point of time, to test the responsiveness of their website by changing the screen dimension or switching to device toolbar and seleting from the available mobile devices with varying screen dimensions.

With Chrome DevTools Protocol support now available in Selenium 4, Nightwatch now supports testing websites on varying screen dimensions. You can override the values of the screen dimensions which your website will use to load in, with just a single command. Along with that, you can also emulate a mobile device for loading your website, and change the device-scale-factor/device-pixel-ratio of your website as well.

<div class="alert alert-info">
  This command only works with Chromium based browsers such as Google Chrome and Microsoft Egde.
</div>

### Override device dimensions

Overriding the device dimensions while running your tests allows you to test how your website loads on different screen dimensions, without actually testing them on devices with different screen sizes. This is especially useful while testing the responsiveness of your website.

All you need to do is call the `browser.setDeviceDimensions()` command with the required parameters before navigating to your website.

`setDeviceDimensions()` accepts an object as its first argument. The specifications of the object are as follows:

<table class="table table-bordered table-striped">
  <thead>
   <tr>
     <th style="width: 100px;">key</th>
     <th style="width: 100px;">type</th>
     <th style="width: 50px;">default</th>
     <th>description</th>
   </tr>
  </thead>
  <tbody>
    <tr>
      <td>`width`</td>
      <td>number</td>
      <td>0</td>
      <td>Overriding width value in pixels (minimum 0, maximum 10000000). 0 disables the override.</td>
    </tr>
    <tr>
      <td>`height`</td>
      <td>number</td>
      <td>0</td>
      <td>Overriding height value in pixels (minimum 0, maximum 10000000). 0 disables the override.</td>
    </tr>    
    <tr>
      <td>`deviceScaleFactor`<br><span class="optional">optional</span></td>
      <td>number</td>
      <td>0</td>
      <td>Overriding device scale factor value. 0 disables the override.</td>
    </tr>
    <tr>
      <td>`mobile`<br><span class="optional">optional</span></td>
      <td>boolean</td>
      <td>false</td>
      <td>Whether to emulate mobile device. This includes viewport meta tag, overlay scrollbars, text autosizing and more.</td>
    </tr>
  </tbody>
</table>

#### Example:

<div class="sample-test"><i>tests/modify-device-dimensions.js</i>
<pre class="line-numbers language-javascript"><code class="language-javascript">
describe('modify device dimensions', function() {
  browser
    .setDeviceDimensions({
      width: 400,
      height: 600,
      deviceScaleFactor: 50,
      mobile: true
    })
    .navigateTo('https://www.google.com');
});
</code></pre></div>

### Reset device dimensions

To reset the device dimensions back to original, you can again call the `browser.setDeviceDimensions()` command, but without any arguments this time.

#### Example:

<div class="sample-test"><i>tests/modify-and-reset-device-dimensions.js</i>
<pre class="line-numbers language-javascript"><code class="language-javascript">
describe('modify and reset device dimensions', function() {
  browser
    .setDeviceDimensions({
      width: 400,
      height: 600,
      deviceScaleFactor: 50,
      mobile: true
    })
    .navigateTo('https://www.google.com')
    .pause(1000)
    .setDeviceDimensions()  // resets the device dimensions
    .navigateTo('https://www.google.com')
    .pause(1000);
});
</code></pre></div>
