#### Lab 9 - AIOPS Licensing Review

| Device     | Username/PW        |
| ---------- | ------------------ |
| FortiAIOPS | admin/fortinet4A!! |

> [!NOTE]
> **FortiAIOPS and what your licences unlock**
>
> FortiAIOPS is a cloud-delivered AIOps platform that ingests telemetry from your Fortinet infrastructure and applies machine learning to deliver proactive monitoring, root cause analysis, and actionable insights. Unlike traditional monitoring dashboards that show you what is happening right now, FortiAIOPS learns what is *normal* for your specific environment and alerts you when behaviour deviates from that baseline — often before users even notice a problem.
>
> **Licence types and what they cover:**
> - **SD-WAN Licence** — Enables FortiAIOPS to monitor SD-WAN link health, path selection, SLA performance, and application experience across WAN links. It includes forecasting capabilities that predict future link utilisation based on historical patterns.
> - **Monitoring and Analytics Licence** — Covers wireless and wired monitoring: AP health, client experience, SLA scoring, RF analysis, and AI Insights for the wireless environment. Each managed FortiGate with associated APs consumes one licence.
>
> **Why licencing via FortiGuard matters:**
> FortiGuard is Fortinet's global threat intelligence and service delivery network. Routing licence updates through FortiGuard means FortiAIOPS always has the latest feature entitlements without requiring manual licence file management. For air-gapped or restricted environments where FortiGuard access is not available, the manual upload option via FortiCare provides an alternative.

1. Navigate to System > Licensing

  ![alt text](media/Lab9-Skeleton-6.png)

  - You will see 1 SD-WAN license and 2 Monitoring and Analytics licenses consumed

2. Navigate to System > FortiGuard

  ![alt text](media/Lab9-Skeleton-8.png)

  - FortiAIOPS updates the license via FortiGuard; however, you can upload the license file from FortiCare using the manual update file option

3. When you register a new FortiAIOPS, you will need the System ID
  - Click the Manual Update button, then select "Click here to know how to get a license file?"
  - In step one, you are presented with the System ID, which is required for initial registration

  ![alt text](media/Lab9-Skeleton-9.png)

4. Confirm that you are getting logs from FAZ to your AIOPS
  - Navigate to Logs and Reports. Under FortiGate, you should see Event logs
  - Check that you have Wi-Fi Events and FortiSwitch Events

> [!TIP]
> If you do not see log data flowing, go back to FortiAnalyzer and verify the log forwarding configuration from Lab 4. FortiAIOPS needs a consistent stream of logs to build its baselines — gaps in log data will affect the quality of AI Insights in the later labs.

#### Lab complete — move on to Lab 10
