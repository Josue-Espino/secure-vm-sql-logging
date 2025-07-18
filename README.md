This repository contains everything to:

1. **Secure an Ubuntu VM**
-SSH hardening, UFW firewall, Fail2Ban

2. **Deploy a MySQL container**
-Docker install, `mysql-server` container, sample date

3. **Capture and filter logs**
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

## Portainer & DBeaver Setup

### Portainer
1. Find your VM's IP: hostname -I | awk '{print$1}'
2. Deploy Portainer: docker colume create portainer_data && docker run -d --name portainer -p 9000:9000 --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer-ce:latest && sudo ufw allow 9000/tcp
3. Access Portainer at: http://<VM-IP>:9000
4. In Portainer: Environments -> Add environment -> Docker standalone -> Socket -> /var/run/docker.sock

### DBeaver
1. Download & install DBeaver CE from dbeaver.io/download
2. In DBeaver: New Connection -> MySQL -> Next
- Host: <VM-IP>
- Port: 3306
- Database: sampledb
- Username: appuser
- Password: AppUserPass!
3. Test Connection (should succeed) -> Finish
4. Query your table: SELECT * FROM users;
