- Action group name : SearchOperationalIssuesActionGroup
- Description - optional : CloudWatch alarm state under the AWS account
- Action group type : Define with API schemas
- Action group invocation : Select an existing Lambda function
    - Select Lambda function : CheckCloudWatchAlarmsFunction
- Action group schema : Define via in-line schema editor
    - OpenAPI : JSON 

```json
{
    "openapi": "3.0.0",
    "info": {
        "title": "AWS Infrastructure & Operations Automation API",
        "version": "1.0.0",
        "description": "APIs for managing AWS Infrastructure by pulling a list of resources in alarm state"
    },
    "paths": {
        "/get_all_alarms": {
            "get": {
                "summary": "Get a list of all resources in AWS account in alarm state",
                "description": "Get a list of all resources in AWS account in alarm state based on cloud watch Metrics",
                "operationId": "get_all_alarms",
                "responses": {
                    "200": {
                        "description": "list of all resources in AWS account in alarm state based on cloud watch Metrics",
                        "content": {
                            "application/json": {
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "type": "object",
                                        "properties": {
                                            "ID": {
                                                "type": "string",
                                                "description": "unique ID of the AWS resource"
                                            },
                                            "ResourceType": {
                                                "type": "string",
                                                "description": "Type of resource ie. EC2, DynamoDB table, Lambda Function"
                                            },
                                            "State": {
                                                "type": "string",
                                                "description": "High CPU utilization, throttling or high error rate"
                                            }
                                        }
                                    }
                                }
                            }
                        }
                    }
                }
            }
        },
        "/send-Notification": {
            "post": {
                "summary": "API to send Notification to operations team with details about issues and remediation steps taken to fix operational issue",
                "description": "API to send Notification to operations team with details about issues and remediation steps taken to fix operational issue",
                "operationId": "sendNotification",
                "requestBody": {
                    "required": true,
                    "content": {
                        "application/json": {
                            "schema": {
                                "type": "object",
                                "properties": {
                                    "emailaddress": {
                                        "type": "string",
                                        "description": "email address to send notifications"
                                    },
                                    "subject": {
                                        "type": "string",
                                        "description": "subject of email notification"
                                    },
                                    "email_body": {
                                        "type": "string",
                                        "description": "body of email notification containing details of issue and all remediation steps taken by agent"
                                    }
                                },
                                "required": [
                                    "emailaddress",
                                    "subject",
                                    "email_body"
                                ]
                            }
                        }
                    }
                },
                "responses": {
                    "200": {
                        "description": "Notifications sent successfully",
                        "content": {
                            "application/json": {
                                "schema": {
                                    "type": "object",
                                    "properties": {
                                        "sendReminderTrackingId": {
                                            "type": "string",
                                            "description": "Unique Id to track the status of the send Notificaiton"
                                        },
                                        "sendReminderStatus": {
                                            "type": "string",
                                            "description": "Status of send notifications"
                                        }
                                    }
                                }
                            }
                        }
                    },
                    "400": {
                        "description": "Bad request. One or more required fields are missing or invalid."
                    }
                }
            }
        }
    }
}
```

---

- Action group name : RemediateEC2OperationalIssuesActionGroup
- Description - optional : remediation steps to fix operational issues
- Action group type : Define with API schemas
- Action group invocation : Select an existing Lambda function
    - Select Lambda function : RemediateEC2OperationalIssueFunction 
- Action group schema : Define via in-line schema editor
    - OpenAPI : JSON 

```json
{
    "openapi": "3.0.0",
    "info": {
        "title": "AWS EC2 Operations Automation API",
        "version": "1.0.0",
        "description": "APIs for managing EC2 and take remediation steps to fix operational issues"
    },
    "paths": {
        
        "/create_snapshot_of_EC2_volume": {
            "post": {
                "summary": "API to create snapshot of EBS volume of affected EC2 instance",
                "description": "API to create snapshot of EBS volume of affected EC2 instance",
                "operationId": "create_ebssnapshot",
                "requestBody": {
                    "required": true,
                    "content": {
                        "application/json": {
                            "schema": {
                                "type": "object",
                                "properties": {
                                    "instanceARN": {
                                        "type": "string",
                                        "description": "arn of affected EC2 instance"
                                    }
                                    
                                },
                                "required": [
                                    "instanceARN"
                                ]
                            }
                        }
                    }
                },
                "responses": {
                    "200": {
                        "description": "ARN of created snapshot of EBS volume of affected EC2 instance",
                        "content": {
                            "application/json": {
                                "schema": {
                                    "type": "object",
                                    "properties": {
                                        "snapshotARN": {
                                            "type": "string",
                                            "description": "arn of EBS volume's snapshot"
                                        }
                                    }
                                }
                            }
                        }
                    },
                    "400": {
                        "description": "Bad request. One or more required fields are missing or invalid."
                    }
                }
            }
        },
        
        "/restart_ec2_instance": {
            "post": {
                "summary": "API to restart affected ec2 instance",
                "description": "API to restart affected ec2 instance",
                "operationId": "restart_ec2",
                "requestBody": {
                    "required": true,
                    "content": {
                        "application/json": {
                            "schema": {
                                "type": "object",
                                "properties": {
                                    "instanceARN": {
                                        "type": "string",
                                        "description": "arn of affected EC2 instance"
                                    }
                                    
                                },
                                "required": [
                                    "instanceARN"
                                ]
                            }
                        }
                    }
                },
                "responses": {
                    "200": {
                        "description": "True if restarted affected ec2 instance",
                        "content": {
                            "application/json": {
                                "schema": {
                                    "type": "object",
                                    "properties": {
                                        "snapshotARN": {
                                            "type": "bool",
                                            "description": "True if restarted EC2 instance"
                                        }
                                    }
                                }
                            }
                        }
                    },
                    "400": {
                        "description": "Bad request. One or more required fields are missing or invalid."
                    }
                }
            }
        }
    }
}
```