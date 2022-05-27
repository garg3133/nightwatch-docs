## Test your websites from different locations, virtually!

Do you ever wish if you could test, siting right at your location, how your website looks at different locations around the world? That too, without using any VPN? Well, you can now do so with Nightwatch.

With the power of Chrome DevTools protocol now in Nightwatch, you can just sit at a place and test how you website behaves at different geographical locations across the world. That is, you can mock the geolocation of your browser before loading your application and then when it loads, it would be as per the location set by you.

This is specially beneficial if you website offers different features and functionalities across different location like different offers, currencies, date/time format, etc.

To see the magic yourself, follow below!

## Mocking Geolocation

Mocking the geolocation of the browser allows you to override the location sent by your browser to the website, so that it responds with a version of the website applicable for that location, and all this without the use of any VPN.

**Note:** This command only works with Chromium based browsers like Google Chrome and Microsoft Egde.

All you need to do is call the `browser.setGeolocation()` command with the required parameters before navigating to your website and see the magic.

`setGeolocation()` accepts an object as its first argument. The specifications of the object are as follows:

key | required | type | description
---|---|---|---
latitude | yes | number | Latitude of the geolocation to be set
longitude | yes | number | Longitude of the geolocation to be set
accuracy | no, default: 100 | number | Accuracy required while mocking the geolocation

### Example:

```js
// Set the geolocation to Tokyo, Japan
browser
  .setGeolocation({
    latitude: 35.689487,
    longitude: 139.691706,
    accuracy: 100
  })
  .navigateTo('https://www.gps-coordinates.net/my-location')
  .pause(3000);
```


## Reset Geolocation back to original

After overriding the geolocation of your website, if you now want to reset the geolocation of the browser back to normal during the same test run, you can do so by using the command `browser.setGeolocation()` again, this time without any arguments.

### Example:

```js
browser
  .setGeolocation({
    latitude: 35.689487,
    longitude: 139.691706,
    accuracy: 100
  })  // sets the geolocation to Tokyo, Japan
  .navigateTo('https://www.gps-coordinates.net/my-location')
  .pause(3000)
  .setGeolocation()  // resets the geolocation
  .navigateTo('https://www.gps-coordinates.net/my-location')
  .pause(3000);
```

Enjoy testing your websites, virtually!
