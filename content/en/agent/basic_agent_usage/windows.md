---
title: Basic Agent Usage for Windows
kind: documentation
description: "Basic functionality of the Datadog Agent on the Windows platform."
platform: Windows
aliases:
    - /guides/basic_agent_usage/windows/
further_reading:
- link: "logs/"
  tag: "Documentation"
  text: "Collect your logs"
- link: "graphing/infrastructure/process"
  tag: "Documentation"
  text: "Collect your processes"
- link: "tracing"
  tag: "Documentation"
  text: "Collect your traces"
---

## Overview

This page outlines the basic features of the Windows Datadog Agent. If you haven't installed the Agent yet, see the [Datadog Agent installation instructions][1] and the [Supported OS versions][2].

## Agent installation

**Starting with release `6.11.0`, the Core and APM/Trace components of the Windows Agent run under the `ddagentuser` account, created at install time, instead of running under the `LOCAL_SYSTEM` account, as was the case on prior versions. If enabled, the Live Process component still runs under the `LOCAL_SYSTEM` account.**

[Refer to the dedicated ddagentuser FAQ to learn more.][3]

To install the Agent, download and run the Datadog Agent MSI **as Administrator**.

Optionally, install the Agent with the command line to customize the settings and/or run an unattended installation. Each configuration item is added as a property to the command line. For instance, the following command installs and pre-configures the Agent with the `<DATADOG_API_KEY>`.

* cmd: `msiexec /i datadog-agent-6-latest.amd64.msi APIKEY="<DATADOG_API_KEY>"`
* Powershell: `Start-Process msiexec -ArgumentList '/i datadog-agent-6-latest.amd64.msi APIKEY="<DATADOG_API_KEY>"'`

**Note**: Use the `/qn /i` options instead of `/i` to run an unattended install. The unattended install may take a few minutes to complete.

The following configuration command line options are available when installing the Agent:

| Variable                   | Type   | Description                                                                                                                                                                                                                       |
|----------------------------|--------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `APIKEY`                   | String | Adds the Datadog API KEY to the configuration file.                                                                                                                                                                               |
| `TAGS`                     | String | Comma separated list of tags to assign in the configuration file. Example: `TAGS="key_1:val_1,key_2:val_2"`                                                                                                                       |
| `HOSTNAME`                 | String | Configures the hostname reported by the Agent to Datadog (overrides any hostname calculated at runtime).                                                                                                                          |
| `LOGS_ENABLED`             | String | Enable (`"true"`) or disable (`"false"`) the log collection feature in the configuration file. Logs are disabled by default.                                                                                                      |
| `APM_ENABLED`              | String | Enable (`"true"`) or disable (`"false"`) the APM Agent in the configuration file. APM is enabled by default.                                                                                                                      |
| `PROCESS_ENABLED`          | String | Enable (`"true"`) or disable (`"false"`) the Process Agent in the configuration file. The Process Agent is disabled by default.                                                                                                   |
| `CMD_PORT`                 | Number | A valid port number between 0 and 65534. The Datadog Agent exposes a command API on port 5001. If that port is already in use by another program, the default may be overridden here.                                             |
| `PROXY_HOST`               | String | If using a proxy, sets your proxy host. [Learn more about using a proxy with the Datadog Agent][4].                                                                                                                               |
| `PROXY_PORT`               | Number | If using a proxy, sets your proxy port. [Learn more about using a proxy with the Datadog Agent][4].                                                                                                                               |
| `PROXY_USER`               | String | If using a proxy, sets your proxy user. [Learn more about using a proxy with the Datadog Agent][4].                                                                                                                               |
| `PROXY_PASSWORD`           | String | If using a proxy, sets your proxy password. [Learn more about using a proxy with the Datadog Agent][4].                                                                                                                           |
| `DDAGENTUSER_NAME`         | String | Override the default `ddagentuser` username used during Agent installation _(v6.11.0+)_. [Learn more about the Datadog Windows Agent User][3]                                                                                     |
| `DDAGENTUSER_PASSWORD`     | String | Override the cryptographically secure password generated for the `ddagentuser` user during Agent installation _(v6.11.0+)_. Must be provided for installs on Domain Servers. [Learn more about the Datadog Windows Agent User][3] |
| `APPLICATIONDATADIRECTORY` | Path   | Override the directory to use for the configuration file directory tree. May only be provided on initial install; not valid for upgrades. Default: `c:\programdata\datadog`. _(v6.11.0+)_                                         |
| `PROJECTLOCATION`          | Path   | Override the directory to use for the binary file directory tree. May only be provided on initial install; not valid for upgrades. Default: `c:\Program Files\Datadog\Datadog Agent`. _(v6.11.0+)_                                |

**Note**: If a valid `datadog.yaml` is found and has an `API_KEY` configured, that file takes precedence over all specified command-line options.

## Agent Commands

The execution of the Agent is controlled by the Windows Service Control Manager.

{{< tabs >}}
{{% tab "Agent v6" %}}

There are a few major changes compared to Agent v5:

* The main executable name is now `agent.exe` (previously `ddagent.exe`).
* Commands are run with the command line `"C:\Program Files\Datadog\Datadog Agent\embedded\agent.exe" <command>`.
* The configuration GUI is now a browser based configuration application (for Windows 64-bit only).

The Agent has a new set of command-line options:

| Command         | Description                                                                 |
|-----------------|-----------------------------------------------------------------------------|
| check           | Runs the specified check.                                                    |
| diagnose        | Executes some connectivity diagnosis on your system.                         |
| flare           | Collects a flare and send it to Datadog.                                     |
| help            | Gets help about any command.                                                 |
| hostname        | Prints the hostname used by the Agent.                                       |
| import          | Imports and converts configuration files from previous versions of the Agent. |
| installservice  | Installs the Agent within the service control manager.                      |
| launch-gui      | Starts the Datadog Agent Manager.                                           |
| regimport       | Import the registry settings into `datadog.yaml`.                           |
| remove-service  | Removes the Agent from the service control manager.                         |
| restart-service | Restarts the Agent within the service control manager.                      |
| run             | Starts the Agent.                                                           |
| start           | Starts the Agent. (Being deprecated, but accepted. Use `run` as an alternative.)        |
| start-service   | Starts the Agent within the service control manager.                        |
| status          | Print the current status.                                                   |
| stopservice     | Stops the Agent within the service control manager.                         |
| version         | Prints the version info.                                                     |

{{% /tab %}}
{{% tab "Agent v5" %}}

Use the Datadog Agent Manager (available from the start menu).

{{< img src="agent/basic_agent_usage/windows/windows-start-menu.png" alt="windows Start Menu" responsive="true" style="width:75%;">}}

Use the `start`, `stop`, and `restart` commands in the Datadog Agent Manager:

{{< img src="agent/basic_agent_usage/windows/manager-snapshot.png" alt="Manager snapshot" responsive="true" style="width:75%;">}}

You can also use Windows Powershell, where available:
`[start|stop|restart]-service datadogagent`

{{% /tab %}}
{{< /tabs >}}

## Configuration

Use the Datadog Agent Manager (available in the start menu) to enable, disable, and configure checks. Restart the Agent for your changes to be applied.

{{< tabs >}}
{{% tab "Agent v6" %}}
The main Agent configuration file is located at:
`C:\ProgramData\Datadog\datadog.yaml`

Configuration files for [integrations][1] are in:
`C:\ProgramData\Datadog\conf.d\`
OR `C:\Documents and Settings\All Users\Application Data\Datadog\conf.d\`

**Note**: `ProgramData` is a hidden folder.

[1]: /integrations
{{% /tab %}}
{{% tab "Agent v5" %}}

The main Agent configuration file is located at:
`C:\ProgramData\Datadog\datadog.conf`

Configuration files for [integrations][1] are in:
`C:\ProgramData\Datadog\conf.d\`
OR `C:\Documents and Settings\All Users\Application Data\Datadog\conf.d\`

**Note**: `ProgramData` is a hidden folder.

[1]: /integrations
{{% /tab %}}
{{< /tabs >}}

## Troubleshooting
### Agent Status and Information

{{< tabs >}}
{{% tab "Agent v6" %}}

To verify the Agent is running, check if the `DatadogAgent` service in the Services panel is listed as *Started*. A process called *Datadog Metrics Agent* (`agent.exe`) should also exist in the Task Manager.

To receive more information about the Agent's state, start the Datadog Agent Manager:

- Right click on the Datadog Agent system tray icon -> Configure, or
- Run `& "C:\program files\datadog\datadog agent\embedded\agent.exe" launch-gui` from an admin Powershell prompt

Then, open the status page by going to *Status* -> *General*.
Get more information on running checks in *Status* -> *Collector* and *Checks* -> *Summary*.

The status command is available for Powershell:

```
& "C:\Program Files\Datadog\Datadog Agent\embedded\agent.exe" status
```

or cmd.exe:

```
"C:\Program Files\Datadog\Datadog Agent\embedded\agent.exe" status
```

{{% /tab %}}
{{% tab "Agent v5" %}}

To verify the Agent is running, check if the service status in the Services panel is listed as "Started". A process called `ddagent.exe` should also exist in the Task Manager.

Information about the Agent's state for Agent v5.2+ is available in the
*Datadog Agent Manager -> Settings -> Agent Status*:

{{< img src="agent/faq/windows_status.png" alt="Windows Status" responsive="true" style="width:50%;" >}}

For the status of Agent v3.9.1 to v5.1, navigate to `http://localhost:17125/status`.

The info command is available for Powershell:

```
& "C:\Program Files\Datadog\Datadog Agent\embedded\python.exe" "C:\Program Files\Datadog\Datadog Agent\agent\agent.py" info
```

or cmd.exe:

```
"C:\Program Files\Datadog\Datadog Agent\embedded\python.exe" "C:\Program Files\Datadog\Datadog Agent\agent\agent.py" info
```

{{% /tab %}}
{{< /tabs >}}

### Logs location

{{< tabs >}}
{{% tab "Agent v6" %}}

The Agent logs are located in `C:\ProgramData\Datadog\logs\agent.log`.
**Note**: `ProgramData` is a hidden folder.

Need help? Contact [Datadog support][1].

[1]: /help
{{% /tab %}}
{{% tab "Agent v5" %}}

For Windows Server 2008, Vista, and newer, the Agent logs are located in `C:\ProgramData\Datadog\logs`.
**Note**: `ProgramData` is a hidden folder.

Need help? Contact [Datadog support][1].

[1]: /help
{{% /tab %}}
{{< /tabs >}}

### Send a flare
{{< tabs >}}
{{% tab "Agent v6" %}}

* Navigate to [http://127.0.0.1:5002][1] to display the Datadog Agent Manager.

* Select flare tab.

* Enter your ticket number (if you have one).

* Enter the email address you use to log in to Datadog.

* Press Submit.

{{< img src="agent/basic_agent_usage/windows/windows_flare_agent_6.png" alt="Windows flare with Agent 6" responsive="true" style="width:75%;">}}


[1]: http://127.0.0.1:5002
{{% /tab %}}
{{% tab "Agent v5" %}}

To send Datadog support a copy of your Windows logs and configurations, do the following:

* Open the Datadog Agent Manager.

* Select Actions.

* Select Flare.

* Enter your ticket number (if you don't have one, leave the value as zero).

* Enter the email address you use to log in to Datadog.

{{< img src="agent/faq/windows_flare.jpg" alt="Windows Flare" responsive="true" style="width:70%;">}}

The flare command is available for Powershell:

```
& "C:\Program Files\Datadog\Datadog Agent\embedded\python.exe" "C:\Program Files\Datadog\Datadog Agent\agent\agent.py" flare <CASE_ID>
```
or cmd.exe:
```
"C:\Program Files\Datadog\Datadog Agent\embedded\python.exe" "C:\Program Files\Datadog\Datadog Agent\agent\agent.py" flare <CASE_ID>
```

#### Flare Fails to Upload

On Linux and macOS, the output of the flare command tells you where the compressed flare archive is saved. In case the file fails to upload to Datadog, you can retrieve it from this directory and manually add as an attachment to an email.

For Windows, you can find the location of this file by running the following from the Agent's Python command prompt:

**Step 1**:

* Agent v5.12+:
    `"C:\Program Files\Datadog\Datadog Agent\dist\shell.exe" since`

* Older Agent versions:
    `"C:\Program Files (x86)\Datadog\Datadog Agent\files\shell.exe"`

**Step 2**:

```
import tempfile
print tempfile.gettempdir()
```

Example:

{{< img src="agent/faq/flare_fail.png" alt="Flare Fail" responsive="true" style="width:70%;">}}

{{% /tab %}}
{{< /tabs >}}

## Use Cases
###  Monitoring a Windows Service

On your target host, launch the Datadog Agent Manager and select the "Windows Service" Integration from the list. For this, there is an out-of-the-box example, however, this example uses DHCP.

To get the name of the service, open `services.msc` and locate your target service. Using DHCP as the target, you can see the service name at the top of the service properties window:

{{< img src="agent/faq/DHCP.png" alt="DHCP" responsive="true" style="width:75%;">}}

When adding your own services, be sure to follow the formatting exactly as shown. If formatting is not correct the integration fails.

{{< img src="agent/faq/windows_DHCP_service.png" alt="Windows DHCP Service" responsive="true" style="width:75%;">}}

Also, any time you modify an integration the Datadog service needs to be restarted. You can do this from services.msc or from the UI sidebar.

For Services, Datadog doesn't track the metrics, only their availability. (For metrics, use the [Process][5] or [WMI][6] integration). To set up a Monitor, select the [Integration monitor type][7] then search for **Windows Service**. From *Integration Status -> Pick Monitor Scope*, choose the service you would like to monitor.

### Monitoring system load for Windows

The Datadog Agent collects a large number of system metrics out of the box. One of the more commonly used system metrics is `system.load.*`.

| Metric                      | Description                                                                    |
|-----------------------------|--------------------------------------------------------------------------------|
| system.load.1 (gauge)       | The average system load over one minute.                                       |
| system.load.15 (gauge)      | The average system load over fifteen minutes.                                  |
| system.load.5 (gauge)       | The average system load over five minutes.                                     |
| system.load.norm.1 (gauge)  | The average system load over one minute normalized by the number of CPUs.      |
| system.load.norm.15 (gauge) | The average system load over fifteen minutes normalized by the number of CPUs. |
| system.load.norm.5 (gauge)  | The average system load over five minutes normalized by the number of CPUs.    |

The `system.load.*` metric is **Unix** specific: it conveys the average amount of resources either waiting to use or currently using the CPU. Each process waiting to use or using the CPU increases the load number by 1. The number at the end of the metric name indicates the average number of these processes in the previous X minutes. For example, `system.load.5` is the average over the last 5 minutes. A value of 0 indicates a completely idle CPU, and a number equal to the number of CPU cores in the environment indicates that the CPU can handle every request coming in with no delay. Any number greater than this means that processes are waiting to use the CPU.

#### Where is System Load for Windows?

While Windows does not offer this exact metric, there is an equivalent option that's available by default in the system metrics: `system.proc.queue.length`. The `system.proc.queue.length` metric allows you to see the number of threads that are observed as delayed in the processor ready queue and are waiting to be executed.

### Monitoring Windows Processes

You can monitor Windows processes with [Live Process Monitoring][8]. To enable this on Windows, edit the [Agent main configuration file][9] by setting the following parameter to true:

`datadog.yaml`:
```yaml
process_config:
  enabled: "true"
```

After configuration is complete, [restart the Agent][10].

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: https://app.datadoghq.com/account/settings#agent/windows
[2]: /agent/#supported-os-versions
[3]: /agent/faq/windows-agent-ddagent-user
[4]: /agent/proxy
[5]: /#monitoring-windows-processes
[6]: /integrations/wmi
[7]: https://app.datadoghq.com/monitors#create/integration
[8]: /graphing/infrastructure/process/?tab=linuxwindows#installation
[9]: /agent/guide/agent-configuration-files/#agent-main-configuration-file
[10]: /agent/guide/agent-commands/#restart-the-agent
