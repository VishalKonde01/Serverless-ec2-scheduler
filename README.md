# 🚀 Serverless EC2 Scheduler

> Automate AWS EC2 instance start/stop operations using **AWS Lambda, EventBridge Scheduler, IAM Least Privilege, and EC2 Tags.**

---

## 📌 Overview

This project automates the **start and stop operations of EC2 instances** based on a predefined schedule.

EC2 tags are used to dynamically identify target instances instead of hardcoding EC2 Instance IDs.

### ⏰ Automation Schedule

| Action | Schedule |
|---|---|
| 🟢 Start EC2 | Every day at 6:00 AM |
| 🔴 Stop EC2 | Every day at 12:00 AM |

---

## 🏗️ Architecture

```text
              ┌─────────────────────────┐
              │   EventBridge Scheduler │
              └────────────┬────────────┘
                           │
                           │ Invoke
                           ▼
              ┌─────────────────────────┐
              │       AWS Lambda        │
              │       Python 3.13       │
              └────────────┬────────────┘
                           │
                           │ EC2 API
                           ▼
              ┌─────────────────────────┐
              │        AWS EC2          │
              │    Tagged Instances     │
              └─────────────────────────┘

              ┌─────────────────────────┐
              │        AWS IAM          │
              │    Least Privilege      │
              └─────────────────────────┘
```

---

## 🔄 Workflow

```text
EventBridge Scheduler
          ↓
      AWS Lambda
          ↓
 Find EC2 instances
    using EC2 Tags
          ↓
   Start / Stop EC2
```

---

## ☁️ AWS Services Used

| AWS Service | Purpose |
|---|---|
| **Amazon EC2** | Compute instances |
| **AWS Lambda** | Performs EC2 start/stop operations |
| **EventBridge Scheduler** | Automatically invokes Lambda |
| **AWS IAM** | Controls permissions |
| **EC2 Tags** | Dynamically identifies target instances |

---

## 🔑 Key Features

- ✅ Serverless EC2 automation
- ✅ Automatic start/stop scheduling
- ✅ IAM Least Privilege
- ✅ Tag-based EC2 instance selection
- ✅ No hardcoded Instance IDs
- ✅ Cost optimization
- ✅ Event-driven architecture
- ✅ Python + Boto3

---

## 💡 Real-World Use Case

Development and testing servers do not always need to run 24/7.

For example:

```text
6:00 AM
   ↓
Start Development EC2
   ↓
Developers use the server
   ↓
12:00 AM
   ↓
Stop Development EC2
```

This reduces unnecessary EC2 runtime during non-working hours and helps optimize AWS costs.

---

## 🏷️ Dynamic EC2 Instance Selection

Instead of hardcoding an EC2 Instance ID:

```text
i-0123456789abcdef
```

the project uses an EC2 tag.

### Example Tag

```text
Key   : AutoSchedule
Value : true
```

Lambda identifies EC2 instances using the configured tag and performs the requested operation.

### Benefits

- No hardcoded Instance IDs
- Easy to add or remove instances
- Reusable automation
- Flexible resource management
- Better scalability

---

## ⚡ AWS Lambda

The Lambda function is developed using:

- **Python 3.13**
- **Boto3**

Lambda receives an action through the event payload.

### 🟢 Start

```json
{
  "action": "start"
}
```

### 🔴 Stop

```json
{
  "action": "stop"
}
```

Lambda identifies the tagged EC2 instances and performs the corresponding start or stop operation.

---

## 🔐 IAM Least Privilege

The Lambda function uses an IAM Execution Role with the required EC2 permissions.

The IAM policy uses a resource/tag-based condition to restrict which EC2 instances can be managed.

### Security Principle

> Give the Lambda function only the permissions it needs.

This reduces the risk of accidentally modifying unrelated EC2 resources.

---

## ⏰ EventBridge Scheduler

Amazon EventBridge Scheduler automatically invokes the Lambda function.

### 🟢 Start Schedule

Every day at 6:00 AM

Payload:

```json
{
  "action": "start"
}
```

### 🔴 Stop Schedule

Every day at 12:00 AM

Payload:

```json
{
  "action": "stop"
}
```

---

## 🧪 Testing

The Lambda function was tested using two test events.

### 🟢 Start Test

```json
{
  "action": "start"
}
```

Expected result:

```text
EC2 Instance → Running
```

### 🔴 Stop Test

```json
{
  "action": "stop"
}
```

Expected result:

```text
EC2 Instance → Stopped
```

The Lambda execution was successfully verified by checking the EC2 instance state.

---

## 📁 Project Structure

```text
ec2-scheduler-project/
│
├── README.md
├── .gitignore
├── LICENSE
│
├── lambda_function.py
├── iam-policy.json
│
└── screenshots/
    ├── 01-ec2-instance.png
    ├── 02-ec2-tag.png
    ├── 03-lambda-function.png
    ├── 04-iam-policy.png
    ├── 05-lambda-test-start.png
    ├── 06-lambda-test-stop.png
    ├── 07-eventbridge-start.png
    ├── 08-eventbridge-stop.png
    └── 09-successful-execution.png
```

---

# 📸 Project Screenshots

### 1️⃣ EC2 Instance

The EC2 instance used for the automation.

![EC2 Instance](screenshots/01-ec2-instance.png)

---

### 2️⃣ EC2 Tag

Tag added to the EC2 instance for dynamic instance selection.

![EC2 Tag](screenshots/02-ec2-tag.png)

---

### 3️⃣ Lambda Function

AWS Lambda function created using Python 3.13.

![Lambda Function](screenshots/03-lambda-function.png)

---

### 4️⃣ IAM Policy

IAM policy attached to the Lambda execution role.

![IAM Policy](screenshots/04-iam-policy.png)

---

### 5️⃣ Lambda Start Test

Lambda tested using the start action.

![Lambda Start Test](screenshots/05-lambda-test-start.png)

---

### 6️⃣ Lambda Stop Test

Lambda tested using the stop action.

![Lambda Stop Test](screenshots/06-lambda-test-stop.png)

---

### 7️⃣ EventBridge Start Schedule

EventBridge Scheduler configured to invoke Lambda every day at 6:00 AM.

![EventBridge Start Schedule](screenshots/07-eventbridge-start.png)

---

### 8️⃣ EventBridge Stop Schedule

EventBridge Scheduler configured to invoke Lambda every day at 12:00 AM.

![EventBridge Stop Schedule](screenshots/08-eventbridge-stop.png)

---

### 9️⃣ Successful Execution

EC2 instance successfully started and stopped according to the configured schedule.

![Successful Execution](screenshots/09-successful-execution.png)

---

## 💰 Benefits

### 1. Cost Optimization

Automatically stopping development instances during non-working hours helps avoid unnecessary compute usage.

### 2. Automation

No need to manually start or stop EC2 instances every day.

### 3. Security

IAM policies follow the principle of least privilege.

### 4. Flexibility

EC2 tags allow Lambda to dynamically identify target instances.

### 5. Scalability

Additional EC2 instances can be included by applying the appropriate tag.

---

## 🛠️ Technologies

| Technology | Purpose |
|---|---|
| **AWS EC2** | Compute |
| **AWS Lambda** | Start/Stop automation |
| **EventBridge Scheduler** | Scheduling |
| **AWS IAM** | Access control |
| **Python 3.13** | Lambda runtime |
| **Boto3** | AWS SDK for Python |
| **EC2 Tags** | Dynamic resource targeting |

---

## 📚 Documentation

Detailed project documentation containing the implementation steps and AWS Console screenshots is available in:

```text
docs/project-documentation.pdf
```

---

## 🎓 Learning Outcomes

Through this project, I gained practical experience with:

- AWS Lambda
- Amazon EC2
- EventBridge Scheduler
- IAM Roles and Policies
- IAM Least Privilege
- EC2 Resource Tags
- Boto3
- Serverless Architecture
- Event-Driven Architecture
- AWS Cost Optimization

---

## 🚀 Future Improvements

- Add CloudWatch monitoring and alarms
- Add SNS notifications
- Support multiple scheduling windows
- Add Terraform / CloudFormation
- Improve error handling and retry mechanisms
- Support different schedules for different environments

---

## 👨‍💻 Author

### Vishal Konde

**Java Full Stack Developer | AWS | DevOps**

🔗 LinkedIn:

[https://www.linkedin.com/in/vishal-konde-dev](https://www.linkedin.com/in/vishal-konde-dev)

---

## ⭐ Conclusion

This project demonstrates how AWS Lambda, EventBridge Scheduler, IAM Least Privilege, and EC2 Tags can be combined to automate EC2 instance management.

The solution reduces manual operations, improves resource management, and helps optimize AWS infrastructure costs.
