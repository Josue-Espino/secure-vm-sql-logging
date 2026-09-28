This repository contains everything to:

1. **Secure an Ubuntu VM**
-SSH hardening, UFW firewall, Fail2Ban

2. **Deploy a MySQL container**
-Docker install, `mysql-server` container, sample date

3.**Capture and filter logs**
-Export container logs (`mysql-server.log`)
-Filter "Ready for start up" and other events into separate files

## Repository Structure

\`\`\`
labserver-project/
|-- README.md
|-- mysql-server.log
|-- startup.log
|-- entrypoint.log
|-- users-query-output.txt
`-- filter-logs.sh
