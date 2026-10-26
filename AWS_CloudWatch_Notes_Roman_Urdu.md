# AWS CloudWatch — Simple Roman Urdu Notes

## 1. CloudWatch kya hai?
Amazon CloudWatch AWS ki monitoring service hai. Ye AWS resources aur applications ke logs, metrics aur alarms ko monitor karne mein help karti hai. Dashboards par data ek jagah dekha ja sakta hai aur Logs Insights se logs search/analyze kiye ja sakte hain.

### Analogy: Ghar ka monitoring system
- **Metrics** = meter jo numbers dikhata hai.
- **Logs** = diary/record jahan likha hota hai kya hua.
- **Alarm** = smoke detector jo condition puri hone par warning deta hai.
- **Dashboard** = ek screen jahan important readings ek saath nazar aati hain.
- **Logs Insights** = diary mein search karne wala tool.

## 2. CloudWatch Metrics
Metrics time ke saath measure hone wale numbers hain.

Examples:
- EC2 CPU utilization: CPU kitna busy hai.
- Network traffic: kitna network data aa raha ya ja raha hai.
- Lambda invocations/errors: function kitni baar chala aur kitni baar error hua.

**Analogy:** Car ka speedometer aur fuel meter numbers dikhata hai.

Yaad rakho: metric batata hai *kitna* ya *kitni baar*; zaroori nahi ke error ka exact message bhi bataye.

## 3. CloudWatch Alarms
Alarm kisi metric ko tumhari set ki hui condition/threshold ke against check karta hai.

Example:
- Condition: EC2 CPU utilization 5 minutes tak 80% se upar rahe.
- `ALARM`: condition meet ho gayi.
- `OK`: condition normal hai.
- `INSUFFICIENT_DATA`: data abhi kam ya unavailable hai.
- Alarm SNS ko notify kar sakta hai agar SNS action configure ho.

**Analogy:** Temperature alarm 30°C se upar hone par warning deta hai.

Important: Alarm khud har issue automatically fix nahi karta. Ye alert de sakta hai; automatic action ke liye alag configuration chahiye.

## 4. CloudWatch Dashboard
Dashboard ek customizable screen hai jahan multiple metrics, alarms aur supported widgets ek jagah dekhe ja sakte hain.

Example: EC2 CPU graph, network traffic, Lambda errors aur alarm status.

**Analogy:** Car ke dashboard par speed, fuel aur warning lights ek hi jagah hoti hain.

## 5. CloudWatch Logs
Logs timestamp wale messages/events hote hain jo application ya service ka behavior aur errors samajhne mein help karte hain.

- **Log event:** Ek individual log message, aam tor par timestamp aur message ke saath.
- **Log stream:** Ek source se related log events ka sequence.
- **Log group:** Related log streams ka collection.

Example messages:
- `Lambda execution started`
- `S3 object uploaded`
- `Access denied`
- `Task failed to start`

**Analogy:** Ek diary entry log event hai; ek source ki entries ka silsila log stream hai; related diaries ka folder log group hai.

Important: Har AWS/server log automatically CloudWatch mein nahi aata. Logging settings ya CloudWatch Agent ki zaroorat ho sakti hai.

## 6. CloudWatch Logs Insights — Search Logs
Logs Insights CloudWatch Logs mein stored logs ko query/search aur analyze karne ka tool hai.

Example: errors search karne ki query:

```sql
fields @timestamp, @message
| filter @message like /ERROR/
| sort @timestamp desc
| limit 20
```

Iska matlab:
- `fields @timestamp, @message` = time aur message show karo.
- `filter @message like /ERROR/` = sirf woh messages jahan `ERROR` ho.
- `sort @timestamp desc` = latest logs pehle.
- `limit 20` = maximum 20 results.

**Analogy:** Agar diary mein 1,000 entries hon, Logs Insights sirf woh entries nikal sakta hai jin mein “ERROR” likha ho.

Note: Sirf selected log groups aur available logs search honge.

## 7. Example: ECS + Nginx project
1. ECS task Nginx container run karta hai.
2. Task definition mein CloudWatch logging configure ho to container logs log group mein aa sakte hain.
3. **Logs** mein messages/errors check karo.
4. **Logs Insights** se errors search karo.
5. **Metrics** mein available CPU/memory ya service metrics dekho.
6. **Alarm** set karo taake metric threshold meet hone par alert mile.
7. **Dashboard** par important graphs aur alarm status ek screen par rakho.

## 8. Quick comparison

| Feature | Kya karta hai? | Analogy |
|---|---|---|
| Metrics | Numbers over time | Meter |
| Alarms | Condition par alert/state | Smoke detector |
| Dashboard | Data ek screen par | Car dashboard |
| Logs | Events/messages ka record | Diary |
| Logs Insights | Logs search/analyze | Diary search |

## 9. Security best practices
- Logs aur dashboards ka access IAM permissions se control karo.
- Passwords, secret keys ya sensitive data logs mein mat likho.
- Zaroorat ke mutabiq log retention set karo.
- Meaningful alarm thresholds set karo warna unnecessary alerts aa sakte hain.
- Troubleshooting mein timestamp aur AWS Region verify karo.

## Short summary
- **Metrics** = numbers.
- **Alarms** = condition par warning.
- **Dashboard** = summary ek screen par.
- **Logs** = kya hua uska record.
- **Logs Insights** = logs mein search aur analysis.

CloudWatch ka goal AWS resources monitor karna aur issues ko jaldi samajhne mein help karna hai.
