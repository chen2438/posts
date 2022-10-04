```bash
version: '3.3'
services:
    gitlab:
        image: gitlab/gitlab-ce:latest
        container_name: "gitlab"
        ports:
            - "443:443"
            - "80:80"
            - "2222:22"
        restart: always
        hostname: xxx.xxx.xxx.xxx
        volumes:
            - /srv/gitlab/config:/etc/gitlab
            - /srv/gitlab/logs:/var/log/gitlab
            - /srv/gitlab/data:/var/opt/gitlab
				
```

