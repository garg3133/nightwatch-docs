## Edge Driver

#### Overview
[Microsoft Edge Driver](https://developer.microsoft.com/en-us/microsoft-edge/tools/webdriver/) is a standalone server which implements the WebDriver protocol for the Edge browser. It is supported by Windows 10 and onwards and is now available for Mac and Linux platforms also.

#### Download

Follow the [Download Microsoft Edge Driver](https://docs.microsoft.com/en-us/microsoft-edge/webdriver-chromium/?tabs=c-sharp#download-microsoft-edge-webdriver) section on the official Microsoft Edge documentation to download the Edge Driver.

After the download completes, extract the `msedgedriver` to your preferred location inside the project and follow the next steps on this page.

More details about installation and usage documentation are available on the official [Microsoft WebDriver homepage](https://developer.microsoft.com/en-us/microsoft-edge/tools/webdriver/).

#### Nightwatch Usage

Nightwatch can manage the EdgeDriver service automatically, as with other WebDriver services, such as GeckoDriver. To use EdgeDriver directly, configure Nightwatch as below and set the `server_path` property of `webdriver` to the location of the extracted `msedgedriver` in your project:

<pre><code class="language-javascript">{
  <strong>"test_settings"</strong>: {
    <strong>"edge"</strong>: {
      "desiredCapabilities": {
        "browserName": "MicrosoftEdge",
        "ms:edgeOptions": {
          "w3c": true,
          "args": [
            //"--headless"
          ]
        }
      },

      "webdriver": {
        "start_process": true,
        "server_path": "<add-msedgedriver-location-here>",
        "cli_args": [
          // "--verbose"
        ]
      }
    }
  }
}
</code></pre>

and then run the tests using the following command:

<pre><code class="language-bash">$ npx nightwatch path/to/tests --env edge</code></pre>

#### Selenium Server Usage

If you're using Microsoft EdgeDriver through Selenium Server, simply set the cli argument `"webdriver.edge.driver"` to point to the location of the binary file. E.g.:

<pre><code class="language-javascript">{
  <strong>"test_settings"</strong>: {
    <strong>"selenium_server"</strong>: {
      "selenium": {
        "start_process": true,
        "port": 4444,
        "server_path": "",
        "command": "standalone",
        "cli_args": {
          "webdriver.edge.driver": "<path/to/msedgedriver>"
        }
      },
      "webdriver": {
        "start_process": false,
        "default_path_prefix": "/wd/hub"
      }
    },
    <strong>"selenium.edge"</strong>: {
      "extends": "selenium_server",
      "desiredCapabilities": {
        "browserName": "MicrosoftEdge",
        "acceptSslCerts": true
      },
    }
  }
}
</code></pre>

and then run the tests using the following command:

<pre><code class="language-bash">$ npx nightwatch path/to/tests --env selenium.edge</code></pre>

**Note:** The above code-block assumes that you are using `@nightwatch/selenium-server` package with Selenium 4. If you are not selenium-server standalone jar file instead, set the `server_path` property of `selenium` to point to the location of the jar file. And if you are using Selenium 3, remove the `command` property from `selenium`.


#### Standalone Usage

If you're only running your tests against Edge, running the EdgeDriver standalone can be slightly faster. Also there is no dependency on Java.

This requires a bit of configuration and you will need to start/stop the EdgeDriver:<br><br>

##### 1) Configure Nightwatch as below:

EdgeDriver runs by default on port 9515.

<pre><code class="language-javascript">{
  <strong>"test_settings"</strong>: {
    <strong>"edge"</strong>: {
      "desiredCapabilities": {
        "browserName": "MicrosoftEdge",
        "ms:edgeOptions": {
          "w3c": true,
          "args": [
            //"--headless"
          ]
        }
      },

      "webdriver": {
        "start_process": false,
        "port": 9515,
        "server_path": "",
        "cli_args": [
          // "--verbose"
        ]
      }
    }
  }
}
</code></pre>

##### 2) Start the EdgeDriver server
From your terminal window, simply CD to the folder where the `msedgedriver` binary is located and run:

<pre><code class="language-bash">$ ./msedgedriver
Starting MSEdgeDriver 98.0.1108.62 (86b4ba0c0a320a2c0c88adba983ad3b5ce15710f) on port 9515
Only local connections are allowed.
Please see https://chromedriver.chromium.org/security-considerations for suggestions on keeping MSEdgeDriver safe.
MSEdgeDriver was started successfully.
</code></pre>

##### 3) Run your tests against Edge

<pre><code class="language-bash">$ npx nightwatch path/to/tests --env edge</code></pre>


##### Full command line usage:

<pre><code>$ ./mdedgedriver -h
Usage: ./msedgedriver [OPTIONS]

Options
  --port=PORT                     port to listen on
  --adb-port=PORT                 adb server port
  --log-path=FILE                 write server log to file instead of stderr, increases log level to INFO
  --log-level=LEVEL               set log level: ALL, DEBUG, INFO, WARNING, SEVERE, OFF
  --verbose                       log verbosely (equivalent to --log-level=ALL)
  --silent                        log nothing (equivalent to --log-level=OFF)
  --append-log                    append log file instead of rewriting
  --replayable                    (experimental) log verbosely and don't truncate long strings so that the log can be replayed.
  --version                       print the version number and exit
  --url-base                      base URL path prefix for commands, e.g. wd/url
  --readable-timestamp            add readable timestamps to log
  --enable-chrome-logs            show logs from the browser (overrides other logging options)
  --allowed-ips=LIST              comma-separated allowlist of remote IP addresses which are allowed to connect to MSEdgeDriver
  --allowed-origins=LIST          comma-separated allowlist of request origins which are allowed to connect to MSEdgeDriver. Using `*` to allow any host origin is dangerous!
</code></pre>
