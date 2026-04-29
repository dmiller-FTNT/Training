#### Lab 4 - FortiAIOps Integration

| Device       | Username/PW        |
| ------------ | ------------------ |
| FortiAIOPS   | admin/fortinet4A!! |
| FortiAnalyzer| admin/fortinet4A!! |

> [!NOTE]
> **What is FortiAIOPS and why integrate it?**
>
> FortiAIOPS (AI Operations) is Fortinet's cloud-based AIOps platform for network monitoring, troubleshooting, and proactive assurance. AIOps — AI for IT Operations — applies machine learning to telemetry data to do things that traditional monitoring tools cannot: detect anomalies before users report them, correlate events across devices and layers, translate raw logs into actionable plain-language insights, and learn what "normal" looks like for your specific environment so it can identify when something deviates from it.
>
> **What FortiAIOPS needs to work:**
> FortiAIOPS collects data from two sources in this lab:
> - **FortiAnalyzer** — forwards logs via syslog, giving FortiAIOPS the historical event data it needs to build baselines and identify trends
> - **FortiGate direct integration** — FortiAIOPS connects directly to the FortiGate's HTTPS API to pull real-time telemetry: client associations, RF metrics, AP status, and configuration. This richer, real-time data is what powers the AI Insights and live visualisations you will see in later labs.
>
> **Why the CA certificate matters:**
> When FortiAIOPS communicates with the FortiGate over HTTPS, it needs to trust the FortiGate's certificate. By downloading the FortiGate's HTTPS CA and installing it in FortiAIOPS, you establish a trusted, verified connection. Without this step, FortiAIOPS cannot securely validate the FortiGate's identity and the integration will fail. This is the same principle as any trusted HTTPS connection — the client must trust the certificate authority that signed the server's certificate.

> [!NOTE]
> FortiAnalyzer in your lab is set up to forward all logs to FortiAIOPS.

1. Log into FortiAnalyzer and authorize your device
  - The device will be part of the root ADOM
  - Click OK, then Close

2. Configure log forwarding
  - Navigate to System Settings > Advanced > Log Forwarding tab and click Create New
  - Name: FortiAIOPS
  - Remote Server Type: Syslog
  - Click OK

3. Navigate to your FortiGate: https://172.31.1(PodID).200
  - Username: admin
  - Password: (none — change it to Fortinet4A!!)

  ![alt text](media/lab4-1.png)

  - Select Login Read Only

4. Download the HTTPS CA Certificate
  - Go to System > Settings and select the Access tab
  - Download the HTTPS CA Certificate

  ![alt text](media/lab4-2.png)

  - Log out of your FortiGate

5. Log into FortiAIOps and install the CA Certificate
  - Navigate to System > CA Certificates

  ![alt text](media/lab4-3.png)

  - Click Install CA Certificate and select the certificate from your FortiGate
  - Certificate Name: FGTBr01

> [!TIP]
> If you replace the FortiGate's HTTPS certificate in the future — due to a firmware upgrade or a new certificate being loaded — you will need to re-download and reinstall the CA here. Using a wildcard certificate on the FortiGate avoids this maintenance burden, as the CA does not change when the leaf certificate is renewed.

6. Add your FortiGate to inventory
  - Navigate to Inventory > Managed FortiGates and click Add
  - Enter the IP or hostname of your FortiGate: 172.31.1(PodID).200 (e.g., Pod 1: 172.31.101.200)
  - Username: admin
  - Password: Fortinet4A!!

  ![alt text](media/lab4-4.png)

  - Shortly, you should see the FortiGate come online

  ![alt text](media/lab4-5.png)

> [!TIP]
> With the FortiGate now in inventory and logs flowing from FortiAnalyzer, FortiAIOPS begins building its understanding of your environment immediately. The AI models need time to establish baselines — typically 24-48 hours of data before insights become meaningful. In a production deployment, plan to onboard FortiAIOPS well before you need it for active troubleshooting.

#### Lab complete — move on to Lab 5
