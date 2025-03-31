# Alert-Popup-
A warning and alert pop-up message in AWS console whenever anyone tries to allow public access to AWS resources before actually implementing it on any resrouce in AWS account

This project make use of AWS IAM policies (in case if you dont have SCP permissions), AWS Lmabda, AWS Cloudformation and AWS Chatbot services.

✅ **Use IAM policies** to block future public access (instead of SCPs).  
✅ **Deploy Lambda for auto-remediation** (without changing existing public resources).  
✅ **Automatically deploy the stack across n number of AWS accounts** using StackSets.  
✅ **Strictly ensure existing infrastructure is not modified.**  

### **✅ What This Updated CloudFormation Stack Does**
1️⃣ **Blocks future public access** using IAM policies (instead of SCPs).  
2️⃣ **Deploys a Lambda function for monitoring only (no auto-remediation)** to ensure existing infrastructure is not affected.  
3️⃣ **Sets up CloudTrail** to track security-related API calls.  
4️⃣ **Creates an SNS topic for security alerts.**  
5️⃣ **Configures AWS Chatbot** to display security alerts in the AWS Console only (not Slack or email).  

---

### **🚀 Deployment Across n number of AWS Accounts**
Since you don't have AWS Organizations access, deploy this template manually in each AWS account using:  
```
aws cloudformation create-stack \
  --stack-name SecurityEnforcementStack \
  --template-body file://template.yaml \
  --capabilities CAPABILITY_NAMED_IAM
```
OR  
Use **StackSets** (if you have permissions) to automate multi-account deployment.
-----
#### 🔹 Why Use StackSets?
✅ Deploys once, updates across all accounts
✅ Can be managed centrally from a single account
✅ Automatically applies new changes when updated

### 🔹 Steps to Deploy in All 50 AWS Accounts
1️⃣ Ensure You Have the Right Permissions
You need a management account (or a delegated administrator) in AWS Organizations.

2️⃣ Create a StackSet Using AWS Console or CLI
👉 AWS Console Method
1. Open the AWS CloudFormation console.

2. Go to StackSets > Create StackSet.

3. Upload the CloudFormation template.

4. Select "Deploy to AWS Organizations".

5. Choose "Deploy in all accounts" or manually select the 50 accounts.

6. Select regions where you want to deploy.

7. Review and create the StackSet.

👉 AWS CLI Method
1. Run the following command, replacing <YOUR_S3_URL> with the S3 URL where your CloudFormation template is stored:
```
aws cloudformation create-stack-set \
  --stack-set-name SecurityEnforcementStackSet \
  --template-url <YOUR_S3_URL> \
  --capabilities CAPABILITY_NAMED_IAM \
  --permission-model SERVICE_MANAGED
```
2. Then, create Stack Instances across all accounts:
```
aws cloudformation create-stack-instances \
  --stack-set-name SecurityEnforcementStackSet \
  --accounts <ACCOUNT_1> <ACCOUNT_2> ... <ACCOUNT_50> \
  --regions us-east-1 us-west-2
```

### **💡 Next Steps**
- ✅ Deploy and test in one AWS account first.  
- 🚀 If successful, deploy across all 50 accounts.
- 
