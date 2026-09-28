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
2. Deploy Portainer: docker volume create portainer_data && docker run -d --name portainer -p 9000:9000 --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer-ce:latest && sudo ufw allow 9000/tcp
3. Access Portainer at: http://<VM-IP>:9000
4. In Portainer: Environments -> Add environment -> Docker standalone -> Socket -> /var/run/docker.sock

### DBeaver
1. Download & install DBeaver CE from dbeaver.io/download
2. In DBeaver: New Connection -> MySQL -> Next
- Host: <VM-IP>
- Port: 3306
- Database: sampledb
- Username: appuser
- Password: set the lab password locally; do not commit it to Git
3. Test Connection (should succeed) -> Finish
4. Query your table: SELECT * FROM users;


## Security Review

**Review status:** Audited  
**Last reviewed:** September 2026

### Portainer Docker socket access

The documented Portainer deployment mounts the host Docker socket into the Portainer container:

```bash
-v /var/run/docker.sock:/var/run/docker.sock
```

Portainer uses this socket to manage the Docker environment. This is therefore a privileged management boundary, not an ordinary application data volume. Access to the Portainer interface should be treated as administrative access to the Docker host.

The repository also documents exposing Portainer on TCP port 9000 and adding a UFW rule for that port.

### Planned remediation

No immediate removal of the Docker socket is planned because the documented Portainer workflow depends on it.

Before changing the deployment, the live VM should be reviewed to determine:

- how Portainer is currently accessed;
- which hosts or users require administrative access;
- whether TCP/9000 is still required;
- whether access can be restricted to trusted LAN/VPN management sources;
- whether Portainer's Docker management capability is still needed for this project.

The preferred hardening direction is to preserve the required Portainer functionality while minimizing who can reach the Portainer interface.

**No Docker or firewall configuration change is being made as part of this documentation update.**

### Validation plan

After the access boundary is chosen:

1. Verify Portainer remains healthy.
2. Verify Portainer can still manage the intended Docker environment.
3. Verify authorized management access.
4. Verify unauthorized sources cannot reach the management interface.
5. Confirm the MySQL workload remains unaffected.

### Credential handling

The repository no longer documents a committed database password. The previous credential found during the security audit is considered compromised and must not be reused.
