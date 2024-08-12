# Networking Security

## ip-ranges.json

- There is a file called `ip-ranges.json` that contains all the IP ranges, region that AWS services uses. It's managed by AWS and updated regularly.
- You can download it from [here](https://ip-ranges.amazonaws.com/ip-ranges.json)
- User guide is [here](https://docs.aws.amazon.com/general/latest/gr/aws-ip-ranges.html) && [here](https://docs.aws.amazon.com/vpc/latest/userguide/aws-ip-ranges.html#aws-ip-egress-control)
- Whenever there is a change to the AWS IP address ranges, we send notifications to subscribers of the AmazonIpSpaceChanged topic. The payload contains information in the following format:

```json
{
  "create-time":"yyyy-mm-ddThh:mm:ss+00:00",
  "synctoken":"0123456789",
  "md5":"6a45316e8bc9463c9e926d5d37836d33",
  "url":"https://ip-ranges.amazonaws.com/ip-ranges.json"
}
```

- 
