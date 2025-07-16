[Home](https:lonestarcoder.github.io) | **[Blog](https://lonestarcoder.github.io/blog/home)**
# Blog Home
## [Query Windows Firewall Rules ONLY from GPO](https://brodykilpatrickblog.wordpress.com/2022/08/18/query-windows-firewall-rules-only-from-gpo/)
***August 18, 2022***
> If a computer has a GPO or a local security policy that defines windows firewall rules, AND there are also local firewall rules, it is difficult to discern between them unless you open each rule. The below PowerShell script will only retrieve rules that were created via an Active Directory GPO or the local security policy (gpedit.msc).
