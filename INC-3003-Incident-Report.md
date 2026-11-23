# INC-3003 — EC2 High CPU Utilization Investigation

## Incident Summary

**Incident ID:** INC-3003  
**Date:** November 23, 2026  
**Environment:** AWS EC2 — Amazon Linux 2023  
**Instance:** northstar-monitoring-web-server  
**Region:** us-east-2  
**Severity:** Low — Controlled Lab Simulation  
**Status:** Resolved — CPU returned to baseline

## 1. Incident Description

A controlled CPU stress test was performed on an AWS EC2 t3.micro instance to simulate elevated CPU utilization and evaluate the monitoring environment.

The objective was to observe how Amazon CloudWatch captured changes in server performance, investigate the results, and verify recovery.

## 2. Monitoring Configuration

- **Metric:** AWS/EC2 CPUUtilization
- **Statistic:** Average
- **Period:** 5 minutes
- **Alarm threshold:** Greater than 80%
- **Evaluation:** 2 out of 2 datapoints
- **Alarm name:** AWS-003-High-CPU-Alarm
- **Notification:** Amazon SNS email

## 3. Investigation and Troubleshooting

**Step 1 — Establish baseline**

Reviewed the CloudWatch monitoring dashboard before initiating the test. CPU utilization was approximately 1%.

**Step 2 — Generate CPU workload**

Installed and executed `stress-ng` using two CPU workers:

`stress-ng --cpu 2 --timeout 10m --metrics-brief`

The initial execution failed because the Session Manager working directory was not writable.

Resolved the issue by changing to `/tmp` and running the command again.

**Step 3 — Monitor performance**

Inspected the CPUUtilization metric in Amazon CloudWatch.

Observed an increase in CPU utilization, reaching approximately 71.75% on the displayed graph.

**Step 4 — Evaluate alarm behavior**

The CPU alarm was configured to trigger above 80% for two consecutive five-minute evaluation periods.

The stress tests were manually interrupted before the configured ten-minute timeout.

The recorded CPU utilization did not meet the alarm's required threshold and duration, so a natural high-CPU alarm was not observed.

**Step 5 — Verify notification functionality**

The alarm-to-SNS notification workflow had been tested separately by temporarily setting the CloudWatch alarm state to ALARM through the AWS CLI.

The notification email was successfully received, and the alarm subsequently returned to OK.

## 4. Findings

| Metric | Observation |
|---|---|
| CPU baseline | Approximately 1% |
| Peak observed CPU | Approximately 71.75% |
| CPU after test | Approximately 1% |
| Configured threshold | Greater than 80% |
| Natural alarm triggered | No |
| SNS email test | Successful |

## 5. Root Cause and Resolution

**Cause of increased CPU utilization:** An intentionally generated workload using `stress-ng`.

**Resolution:** The stress-testing processes were stopped, and subsequent CloudWatch measurements showed CPU utilization returning to its baseline.

No production service incident occurred. This was a controlled simulation.

## 6. Lessons Learned

- CloudWatch provides visibility into EC2 performance changes.
- Alarm thresholds and evaluation periods determine when alerts trigger.
- Short CPU spikes may not satisfy sustained-utilization alarm conditions.
- Linux directory permissions can prevent utilities or monitoring agents from functioning correctly.
- Monitoring, investigation, and recovery verification are essential parts of incident response.
- Successful SNS delivery testing does not necessarily mean a real metric threshold was breached.

## 7. Conclusion

The simulation demonstrated the ability to generate a controlled workload, investigate CPU utilization in CloudWatch, interpret alarm behavior, and verify recovery.

The monitoring and notification environment was functioning, with the alarm notification workflow validated separately.
