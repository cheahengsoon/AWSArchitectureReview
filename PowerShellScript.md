Sure, here is a PowerShell script that audits an AWS architecture review for Reliability, Security, Cost Optimization, Operational Excellence, and Performance Efficiency. This script uses the AWS PowerShell module to gather relevant information.

```powershell
# Ensure AWS PowerShell module is installed
Install-Module -Name AWSPowerShell -Force -AllowClobber

# Import AWS PowerShell module
Import-Module AWSPowerShell

# Set AWS region
$region = "us-east-1"

# Reliability Audit
function Test-Reliability {
    Write-Host "Reliability Audit"
    # List all EC2 instances and their status
    Get-EC2Instance -Region $region | Select-Object InstanceId, State.Name

    # Check the status of RDS instances
    Get-RDSDBInstance -Region $region | Select-Object DBInstanceIdentifier, DBInstanceStatus

    # List S3 bucket configurations
    Get-S3Bucket | ForEach-Object {
        Get-S3BucketLocation -BucketName $_.BucketName
        Get-S3BucketVersioning -BucketName $_.BucketName
    }
}

# Security Audit
function Test-Security {
    Write-Host "Security Audit"
    # List IAM users
    Get-IAMUser | Select-Object UserName

    # List IAM roles
    Get-IAMRole | Select-Object RoleName

    # List IAM policies
    Get-IAMPolicy | Select-Object PolicyName, Arn

    # Check security groups
    Get-EC2SecurityGroup -Region $region | Select-Object GroupName, GroupId, Description
}

# Cost Optimization Audit
function Test-CostOptimization {
    Write-Host "Cost Optimization Audit"
    # List all EC2 instances and their types
    Get-EC2Instance -Region $region | Select-Object InstanceId, InstanceType

    # List all S3 buckets and their sizes
    Get-S3Bucket | ForEach-Object {
        $bucketName = $_.BucketName
        Get-S3Object -BucketName $bucketName | Measure-Object -Property Size -Sum
    }

    # List RDS instances and their types
    Get-RDSDBInstance -Region $region | Select-Object DBInstanceIdentifier, DBInstanceClass
}

# Operational Excellence Audit
function Test-OperationalExcellence {
    Write-Host "Operational Excellence Audit"
    # List CloudWatch alarms
    Get-CWAlarm | Select-Object AlarmName, StateValue

    # List CloudWatch log groups
    Get-CWLLogGroup | Select-Object LogGroupName, Arn

    # Check AWS Config rules
    Get-CFGRule | Select-Object ConfigRuleName, ConfigRuleState
}

# Performance Efficiency Audit
function Test-PerformanceEfficiency {
    Write-Host "Performance Efficiency Audit"
    # List EC2 instances and their CPU utilization
    Get-EC2Instance -Region $region | ForEach-Object {
        $instanceId = $_.InstanceId
        Get-CWMetricStatistic -Namespace AWS/EC2 -MetricName CPUUtilization -Dimensions @{Name="InstanceId";Value=$instanceId} -StartTime (Get-Date).AddDays(-7) -EndTime (Get-Date) -Period 3600 -Statistics Maximum
    }

    # List RDS instances and their read/write IOPS
    Get-RDSDBInstance -Region $region | ForEach-Object {
        $dbInstanceId = $_.DBInstanceIdentifier
        Get-CWMetricStatistic -Namespace AWS/RDS -MetricName ReadIOPS -Dimensions @{Name="DBInstanceIdentifier";Value=$dbInstanceId} -StartTime (Get-Date).AddDays(-7) -EndTime (Get-Date) -Period 3600 -Statistics Maximum
        Get-CWMetricStatistic -Namespace AWS/RDS -MetricName WriteIOPS -Dimensions @{Name="DBInstanceIdentifier";Value=$dbInstanceId} -StartTime (Get-Date).AddDays(-7) -EndTime (Get-Date) -Period 3600 -Statistics Maximum
    }
}

# Execute all audits
Test-Reliability
Test-Security
Test-CostOptimization
Test-OperationalExcellence
Test-PerformanceEfficiency
```

Save the above script to a file named `AWS_Audit_Script.ps1` and run it in a PowerShell terminal with the appropriate AWS credentials configured. This script will perform the audits and output relevant information for each category.
