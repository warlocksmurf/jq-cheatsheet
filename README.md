## Analyzing endpoints
`find . -type f -exec jq '.Records[] | .userIdentity.arn' {} \; | sort | uniq` <br>
`find . -type f -exec jq '.Records[] | select(.sourceIPAddress == "X.X.X.X")' {} \; | grep "userName"`

## Analyzing event details
`find . -type f -exec jq '.Records[] | [.eventTime, .sourceIPAddress, .userIdentity.arn, .eventName] | @tsv' {} \; | sort`

## Analyzing AWS enumeration events  
`find . -type f -exec jq '.Records[] | [.eventTime, .sourceIPAddress, .userIdentity.arn, .eventName] | @tsv' {} \; | sort | grep -iE '(List|Get|Describe)'` <br>
`find . -type f -exec jq '.Records[] | select(.sourceIPAddress == "X.X.X.X")' {} \; | grep "AccessDenied"` (Failed attempts)


## Example for selecting based on event details or username
`find . -type f -exec jq '.Records[] | select(.eventName == "ListAccessKeys")' {} \;` <br>
`find . -type f -exec jq '.Records[] | select(.eventName == "ListAccessKeys") | .userIdentity.arn' {} \;` (display ARN field of the list accesskey action) <br>
`find . -type f -exec jq '.Records[] | select(.eventName == "ListAccessKeys" and .userIdentity.arn == "arn:aws:iam::xxxxxxxxx:user/xxx")' {} \;` <br>
`find . -type f -exec jq '.Records[] | select(.userIdentity.arn == "arn:aws:iam::xxxxxxxxx:user/xxx") | [.eventTime, .sourceIPAddress, .userIdentity.arn, .eventName] | @csv' {} \; | sort` <br>
`find . -type f -exec jq '.Records[] | [.eventTime, .sourceIPAddress, .userIdentity.arn, .eventName, .requestParameters.key] | @tsv' {} \; | sort | grep -iE '(List|Get|Describe)' | grep "X.X.X.X"` <br>

## Find info related to the term 
`cat <logname> | jq | grep -hi -A 10 <term>`

EC2 
ModifyImageAttribute 

IAM persistence 
CreateUser, CreateAccessKey, CreateLoginProfile, UpdateLoginProfile, AttachUserPolicy, CreateKeyPair, CreateSecurityGroup, 

S3  
Putbucketversioning, GetObject, DeleteObject, RunInstances, 

Errors and Parameters
.errorCode, .errorMessage, .requestParameters.key (get file name), .requestParameters.filterSet (get executed commands in @json)

Logging 
StopLogging, DeleteTrail 
