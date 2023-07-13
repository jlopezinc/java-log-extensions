<a href="https://opensource.newrelic.com/oss-category/#community-plus"><picture><source media="(prefers-color-scheme: dark)" srcset="https://github.com/newrelic/opensource-website/raw/main/src/images/categories/dark/Community_Plus.png"><source media="(prefers-color-scheme: light)" srcset="https://github.com/newrelic/opensource-website/raw/main/src/images/categories/Community_Plus.png"><img alt="New Relic Open Source community plus project banner." src="https://github.com/newrelic/opensource-website/raw/main/src/images/categories/Community_Plus.png"></picture></a>


# New Relic logging extensions

The New Relic logging plugins are extensions for common Java logging frameworks. They are designed to capture app,
transaction trace, and span information as part of your application log messages.

For the latest information, please see the [New Relic Documentation](https://docs.newrelic.com/docs/logs/new-relic-logs/enable-logs-context/enable-logs-context-java).

We support:

* [java.util.logging](jul/README.md)
* [Apache Log4j 2.13.2 or higher](log4j2/README.md)
  * We strongly suggest that log4j 2.15.x or higher be used to avoid [CVE-2021-44228](https://nvd.nist.gov/vuln/detail/CVE-2021-44228)
* [Apache Log4j 1.x](log4j1/README.md)
* [Logback 1.2](logback/README.md)
* [Logback 1.1](logback11/README.md)
* [Dropwizard 1.3](dropwizard/README.md)

## Configuration Options
The following config options apply to all supported logging frameworks, with some noted exceptions.

### Mapped Diagnostic Context (MDC)
Not all logging frameworks provide an MDC mechanism (e.g. JUL) but for those that do you can configure
the logging extension to add the MDC data in addition to the trace context data from New Relic.

Default for adding MDC is `false`, change this to `true` if you wish to include MDC data on log events. It can be configured by
environment variable (`NEW_RELIC_LOG_EXTENSION_ADD_MDC=boolean`) or system property (`-Dnewrelic.log_extension.add_mdc=boolean`).

Note that, if set to `true` all MDC data will be added to log events. Currently, there is no filtering capability for excluding specific MDC entries.

MDC data will be added to log events with a `context.` prefix to distinguish it as user defined context data as well as to prevent potential
collisions with New Relic specific context keys.

### Exception Stack Trace Size
You can configure the logging extension to control the max stack trace size for exceptions added to log events.

Default max stack trace size is `300`. It is recommended that you do not exceed this value or data could be dropped or truncated as well as
lead to higher log event ingest costs. Max stack trace size can be configured by environment variable (`NEW_RELIC_LOG_EXTENSION_MAX_STACK_SIZE=integer`)
or system property (`-Dnewrelic.log_extension.max_stack_size=integer`).

### Include full exception stack trace
<b>Only supported on logback</b>

You can configure the logging extension to include in `error.stack` the full stack trace just like the downstream library does.
This includes the caused by chain.

As an example, this snippet:
```
Exception inner2 = new IllegalAccessException("test caused by 2");
Exception inner1 = new RuntimeException("test caused by 1", inner2);
LOGGER.error("failed for test!", new RuntimeException("test exception", inner1));
```
Will set the `error.stack` as
```
java.lang.RuntimeException: test exception
    at (regular stacktrace)
    at ...
Caused by: java.lang.RuntimeException: test caused by 1
	... 49 common frames omitted
Caused by: java.lang.IllegalAccessException: test caused by 2
	... 49 common frames omitted
```

Default is `false` for retro-compatibility.

This setting can be enabled via environment variable (`NEW_RELIC_LOG_EXTENSION_INCLUDE_FULL_ERROR_STACK=true`) or system variable (`-Dnewrelic.log_extension.include_full_error_stack=true`).

Be aware that enabling this setting will use the downstream library's stacktrace generation, not taking in  consideration 
the value of [Exception Stack Trace Size](#exception-stack-trace-size).

## Support

Should you need assistance with New Relic products, you are in good hands with several diagnostic tools and support channels.

If the issue has been confirmed as a bug or is a Feature request, please file a Github issue.

**Support Channels**

* [New Relic Documentation](https://docs.newrelic.com/docs/logs/enable-log-management-new-relic/logs-context-java/configure-logs-context-java): Comprehensive guidance for using our platform
* [New Relic Community](https://discuss.newrelic.com/tags/logs): The best place to engage in troubleshooting questions
* [New Relic University](https://learn.newrelic.com/): A range of online training for New Relic users of every level
* [New Relic Technical Support](https://support.newrelic.com/) 24/7/365 ticketed support. Read more about our [Technical Support Offerings](https://docs.newrelic.com/docs/licenses/license-information/general-usage-licenses/support-plan). 

## Contributing

We encourage your [contributions](CONTRIBUTING.md) to improve the log extensions. Keep in mind when you submit your pull request, you'll need to sign the CLA via the click-through using CLA-Assistant. You only have to sign the CLA one time per project.
If you have any questions, or to execute our corporate CLA, required if your contribution is on behalf of a company, please drop us an email at opensource@newrelic.com.

**A note about vulnerabilities**

As noted in our [security policy](https://github.com/newrelic/java-log-extensions/security/policy), New Relic is committed to the privacy and security of our customers and their data. We believe that providing coordinated disclosure by security researchers and engaging with the security community are important means to achieve our security goals.

If you believe you have found a security vulnerability in this project or any of New Relic's products or websites, we welcome and greatly appreciate you reporting it to New Relic through [HackerOne](https://hackerone.com/newrelic).
