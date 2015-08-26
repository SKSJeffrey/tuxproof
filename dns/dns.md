% DNS
% 2015-08-21
% Jeffrey Wen

## TXT Record
A TXT record is arbitrary text inserted into a DNS record 

### Example of TXT Record with Syntax
SPF domains have to publish at least two directives:

1. Version identifier
2. Default mechanism

The simplest possible SPF record: Your domain, mydomain.com, never sends mail
```bash
mydomain.com. TXT "v=spf1 -all"
```
It makes sense to have the above TXT record when a domain is only used for web services and does not send mail

MX server send mail, designate them
```bash
mydomain.com. TXT "v=spf1 mx ptr -all"
```
If mydomain.com has two MX servers, mx01 and mx02, they would both be allowed to send mail from mydomain.com from the above TXT record

If other machines in the domain also send mail, designate them
```bash
mydomain.com. TXT "v=spf1 mx ptr -all"
```
The above TXT record designates all the hosts whose PTR hostname match mydomain.com.

## SPF Results
**Pass** - This IP is authorized to send email from this domain.  
**Fail** - This IP is not authorized to send email from this domain.  
**SoftFail** - This IP probably is not authorized to send email from this domain, but the domain owners are not certain.  
**Neutral** - The domain does not know if the IP is allowed to send email or not.  
**TempError** - A temporary error occurred. The email should be retried later.  
**PermError** - A permanent error was encountered. The email should be rejected.  
**None** - No SPF record was found. It cannot be determined if the IP is allowed to send email from this domain.  
