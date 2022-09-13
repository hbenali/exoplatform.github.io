# Running eXo Platform in Debug and Dev modes

Dev and Debug modes are turned off by default and are not recommended in production because of performance impact. They should be turned on only for debugging in development environment.

If you are debugging against eXo Platform Tomcat, use the following start command:

``` bash
./start_eXo.sh --dev --debug
```

This section will introduce more details about [Debug Mode](#debug-mode) and [Dev Mode](#dev-mode) and their effects.

## Debug mode

The Debug mode in eXo Platform is generally like other Java applications using [JDWP](http://docs.oracle.com/javase/7/docs/technotes/guides/jpda/jdwp-spec.html) that enables debugging by IDE.

In eXo Platform Tomcat, the Debug mode is turned on by appending
`--debug` to the startup command:

``` bash
./start_eXo.sh --debug
```

This parameter adds the following JVM option:

``` bash
-agentlib:jdwp=transport=dt_socket,address=8000,server=y,suspend=n
```

## Dev mode

The Dev mode is useful for debugging container configuration, CSS and JavaScript.

In eXo Platform, the Dev mode is turned on by appending `--dev` to the startup command:

``` bash
./start_eXo.sh --dev
```

This parameter will add the following system properties:

-   **-Dorg.exoplatform.container.configuration.debug**
-   **-Dexo.product.developing=true**

**Effects of Dev mode**

Hereafter are effects of the Dev mode:

- **JavaScript and CSS debug** - For optimizing performance, eXo Platform merges, minifies and compresses all CSS/Stylesheet resources into one at the startup. This reduces requests to the server, so performance will be improved, but this causes developers to restart the server for any CSS resource modification to take effect (similar to JavaScript). So in the Dev mode, the feature (JavaScript/CSS compressor) will be disabled for easy debugging.
- **Hot re-loading of configuration** : You can modify container configuration without restarting the server. The hot re-loading can be done via JMX clients, such as JConsole, as follows:
  - Connect JConsole to the eXo Platform process.
  - Find an MBean with object name: `exo:container=root`.
  - Run the `reload()` operation.

![Reload Configuration](/img/debug-dev-mode/reload_configuration.png)

-   **Unpacking .war files (Tomcat)** - Particularly to eXo Platform Tomcat in the Dev mode, the startup scripts also set `EXO_TOMCAT_UNPACK_WARS=true` that results in decompressing .war archives in webapps folder.
