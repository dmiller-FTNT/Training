#### Lab 11 - FortiAIOPS AI Insights and Introduction to Troubleshooting

| Device     | Username/PW        |
| ---------- | ------------------ |
| FortiAIOPS | admin/fortinet4A!! |

> [!NOTE]
> **From reactive to proactive: what AI Insights changes**
>
> Traditional wireless troubleshooting is reactive — a user calls the help desk, a ticket is opened, someone logs into the controller and starts hunting through logs. By the time the issue is investigated, the conditions that caused it may have changed, the logs may have rolled over, and the client has moved on. You are left guessing.
>
> FortiAIOPS AI Insights flips this model. Instead of waiting for a user to report a problem, the platform continuously analyses telemetry — association events, DHCP transactions, roaming events, signal levels, airtime utilisation, SLA metrics — and surfaces anomalies automatically. Issues are presented in plain language with a severity score, a timeline, and suggested remediations. The AI model has already done the log correlation that would take a skilled engineer 30 minutes to do manually.
>
> **SLA scoring and dynamic baselines:**
> A fixed SLA threshold — "connection time must be under 3 seconds" — sounds reasonable, but ignores the reality that different APs in different environments will naturally have different connection characteristics. An AP in a high-density conference room will have different normal behaviour than one in a quiet executive office. Static thresholds either cry wolf constantly or miss real problems.
>
> Dynamic baselines solve this by letting FortiAIOPS learn what is normal for each individual AP. If AP-01 consistently takes 1.2 seconds for clients to connect, a 2.5-second connection is a meaningful deviation worth flagging. If AP-02 in the lobby regularly sees 2.8-second connections due to high client turnover, the same 2.5 seconds is unremarkable. The SLA adapts to the AP, not the other way around.
>
> **Spectrum analysis and duty cycle:**
> Channel utilisation — also called duty cycle in RF terms — measures what percentage of time the wireless medium is occupied. A channel at 80% duty cycle has very little airtime remaining for new transmissions, which causes queuing, retries, and poor performance for all clients on that channel. Spectrum analysis in FortiAIOPS lets you observe this in real time and over time, helping you identify whether poor performance is caused by your own traffic, interference from neighbouring networks, or non-Wi-Fi sources like Bluetooth, microwave ovens, or wireless cameras.

##### Network Assurance

1. Navigate to AI Insights > Network Assurance

  ![alt text](media/lab11-1.png)

  - Provides a wireless health score and identifies the lowest-performing AP
  - Also identifies the SLA score and shows successes, failures, and details of SLA violations
  - Explore this section and find details of the issues that have impacted the scores

  ![alt text](media/lab11-2.png)

  - The summary displays logs translated into plain English for your review and consideration

  ![alt text](media/lab11-3.png)

2. Navigate to Impacted SLAs

  ![alt text](media/lab11-4.png)

  - Compare what was previously available in FAZ and FortiOS alone — FortiAIOPS provides the insight needed for effective understanding of your environment
  - Issues are not only identified, but advice on how to resolve them is also provided

  ![alt text](media/lab11-5.png)

##### SLA Configuration

3. Configure SLA baselines
  - SLA is configured in the Configuration section within this menu
  - Move the "Time to Connect" SLA to Dynamic Baselines
  - FortiAIOPS learns what is normal for each AP by analyzing logs; since each AP is placed in a different location, baselines may differ per AP
  - Alternatively, you can configure this by ADOM or FortiGate as desired

  ![alt text](media/lab11-6.png)

> [!TIP]
> Start with Dynamic Baselines for all SLA metrics when first deploying FortiAIOPS. Once you have 2-4 weeks of baseline data, review whether the learned thresholds align with your expectations. You can then move specific metrics to static thresholds if you have a business-driven SLA requirement — for example, a contact centre environment where connection time must always be under 2 seconds regardless of what the AI considers normal.

##### Wireless Clients

4. Navigate to Wireless > Wireless Clients
  - Select a client and click View Details

  ![alt text](media/lab11-7.png)

  - You will see detailed information about that client and its current capabilities
  - Scroll down to AI Insights to find more details on issues and examine the resolution options offered by FortiAIOPS

  ![alt text](media/lab11-8.png)

  - Review the other menus: Performance, Application, Destinations, Policies, and Logs

> [!NOTE]
> Applications require App Inspection to be enabled on your security profile. FortiAIOPS also provides insight into SD-WAN and Forecasting.

> [!TIP]
> The client capability view is particularly useful for troubleshooting. If a client is connecting at lower data rates than expected, the capabilities section will show whether it supports the bands and features your AP is offering — revealing whether the issue is a client limitation (e.g., no Wi-Fi 6 support) or an environmental problem (e.g., poor signal causing rate downgrade).

##### Spectrum Analysis

5. Navigate to Wireless > Access Points and select your AP
  - Note the channel your AP is operating on for 5 GHz
  - Click View Details, then navigate to Spectrum Analysis
  - Select the band for 5 GHz and set the channel range to include your channel
  - Click Start

  ![alt text](media/lab11-9.png)

6. Run a speed test on your device connected to the AP and scroll down to Duty Cycle

  ![alt text](media/lab11-10.png)

  - You should notice the duty cycle increase — this is expected, as it is normal to consume airtime when moving data during a speed test
  - Consistently high channel utilization under normal operation is where it becomes a problem, reinforcing the need for capacity planning
  - Review the spectrogram to see the duty cycle over time

  ![alt text](media/lab11-11.png)

> [!NOTE]
> High amounts of red indicate a high duty cycle, which is the RF term for channel utilization. Wi-Fi uses unlicensed channels that are not dedicated solely to Wi-Fi.

> [!TIP]
> In production, use spectrum analysis as part of your regular capacity planning review rather than only during incidents. An AP consistently running above 70% duty cycle under normal load is a leading indicator of future congestion — add capacity before users start complaining. This is the shift from reactive to proactive that FortiAIOPS enables.

#### Lab complete — move on to Lab 12
