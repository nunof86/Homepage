# Playbook - Ansible

## Ansible Playbook

1. Replace the <mark style="color:red;">`hosts:`</mark> option to <mark style="color:red;">**localhost**</mark> or other <mark style="color:red;">**host**</mark> of your choice.

```yaml
---

- name: Homepage Setup
  hosts: localhost
  become: true
  any_errors_fatal: true
  max_fail_percentage: 0

  tasks:
    - name: Update apt cache
      apt:
        update_cache: yes
      when: ansible_distribution in ['Ubuntu', 'Debian']

    - name: System update
      apt:
        update_cache: yes
      changed_when: false
      when: ansible_distribution in ['Ubuntu', 'Debian']

    - name: Dist upgrade
      apt:
        upgrade: dist
      register: upgrade_output
      when: ansible_distribution in ['Ubuntu', 'Debian']

    - name: Install required packages
      apt:
        name:
          - apt-transport-https
          - ca-certificates
          - software-properties-common
          - curl
        state: present
      when: ansible_distribution in ['Ubuntu', 'Debian']

    - name: Add Docker APT key to trusted.gpg.d
      shell: "curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/docker.gpg"
      when: ansible_distribution in ['Ubuntu', 'Debian']

    - name: Add Docker APT repository
      apt_repository:
        repo: deb [arch=amd64] https://download.docker.com/linux/ubuntu {{ ansible_lsb.codename }} stable
        state: present
      when: ansible_distribution in ['Ubuntu', 'Debian']

    - name: Install Docker
      apt:
        name: docker-ce
        state: latest
      when: ansible_distribution in ['Ubuntu', 'Debian']

    - name: Download and Install Docker Compose
      get_url:
        url: https://github.com/docker/compose/releases/latest/download/docker-compose-Linux-x86_64
        dest: /usr/bin/docker-compose
        mode: '0755'
      when: ansible_distribution in ['Ubuntu', 'Debian']

    - name: Create Homepage Directory
      file:
        path: /opt/homepage
        state: directory

    - name: Create .env File
      copy:
        content: |
          PUID=1000
          PGID=1000
        dest: /opt/homepage/.env  

    - name: Create docker-compose.yml File
      copy:
        content: |
          version: "3.3"
          services:
            homepage:
              image: ghcr.io/gethomepage/homepage:latest
              container_name: homepage
              ports:
                - 8000:3000
              env_file: .env
              volumes:
                - ./config:/app/config
              environment:
                PUID: $PUID
                PGID: $PGID
        dest: /opt/homepage/docker-compose.yml

    - name: Start Docker Container for Homepage
      command: docker-compose up -d
      args:
        chdir: /opt/homepage
```
