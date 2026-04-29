#### Lab 12 - FortiAIOPS SD-WAN Insights

| Device     | Username/PW        |
| ---------- | ------------------ |
| FortiAIOPS | admin/fortinet4A!! |

> [!NOTE]
> **What FortiAIOPS adds to SD-WAN visibility**
>
> FortiManager already gives you SD-WAN configuration and basic link monitoring. FortiAIOPS extends this with a layer of intelligence that standard monitoring cannot provide: it correlates SD-WAN performance data with application experience, identifies patterns across time, and uses AI to forecast future behaviour based on what it has learned about your environment.
>
> **Insights vs. Forecasting:**
> - **Insights** — Shows you what is happening and what has happened: link health scores, SLA performance, path selection events, and application quality on each WAN link. Anomalies are surfaced automatically — if a link that normally performs well suddenly degrades, you see it immediately without having to watch a dashboard.
> - **Forecasting** — Uses historical patterns to predict future utilisation and performance. If your primary WAN link consistently saturates at 08:30 on Monday mornings, FortiAIOPS will surface this trend before it becomes a problem, giving you the data to make an informed capacity decision or adjust SD-WAN policy to pre-emptively shift traffic.
>
> **FortiAI — what the AI model is actually doing:**
> FortiAI is the embedded AI engine within FortiAIOPS. Rather than applying simple static thresholds ("alert if latency exceeds 100ms"), it builds a statistical model of normal behaviour for each link, each application, and each time window. When it detects a deviation from that model — even if the absolute value is still within acceptable limits — it flags it as an anomaly worth investigating. This catches gradual degradation that static thresholds miss entirely: a link whose latency slowly drifts from 12ms to 28ms over two weeks will never cross a 50ms threshold, but FortiAI will notice the trend and alert you long before users are impacted.

1. Navigate to SD-WAN > Insights

  ![alt text](media/lab12-1.png)

  - Review what is shown and consider the use cases

> [!TIP]
> As you explore the Insights view, think about what this would mean in a real multi-site deployment. Every branch FortiGate with an SD-WAN licence is feeding telemetry into this view. Instead of logging into each FortiGate individually to check link health, you have a single consolidated view across your entire WAN estate — with AI surfacing only the issues that need attention.

2. Navigate to Forecasting

  ![alt text](media/lab12-2.png)

  - Review what is shown for today

> [!TIP]
> Forecasting is most valuable when combined with a capacity review process. Schedule a monthly review of the forecasting data for your highest-utilisation links and use it to drive purchasing and circuit sizing decisions before performance degrades. This converts network capacity planning from a reactive, incident-driven process into a data-driven operational discipline.

3. Click Show FortiAI Insights and select an option to find out more

  ![alt text](media/lab12-3.png)

> [!NOTE]
> FortiAI Insights translates complex SD-WAN telemetry into plain-language explanations of what is happening and why. In a support context, these insights can be attached directly to a ticket or shared with a circuit provider as evidence of degradation — removing the need to extract and interpret raw metrics manually.

#### Lab complete
