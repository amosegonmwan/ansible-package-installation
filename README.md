# Ansible Package Installation Playbooks

This repository contains Ansible playbooks for installing packages on target hosts. The playbooks support installation of both single and multiple packages, and handle different Linux distributions (Debian and RedHat-based).

## Repository Name

**`ansible-package-installation`**

## Playbooks

### 1. Install Single Package

This playbook installs a single package on the target hosts based on the OS family.

#### Playbook File: `install_single_package.yml`

```yaml
- hosts: "{{ group }}"
  become: yes

  vars_prompt:
    - name: group 
      prompt: Enter target group
      private: no

    - name: package 
      prompt: Enter packages to install
      private: no

  gather_facts: true

  tasks:
    - name: Install package with apt
      apt:
        name: "{{ package }}"
        state: present
      when: ansible_facts['os_family'] == "Debian"
      
    - name: Install package with yum
      yum:
        name: "{{ package }}"
        state: present
      when: ansible_facts['os_family'] == "RedHat"
```
---

### 2. Install Multiple Packages
This playbook installs multiple packages on the target hosts based on the OS family.

#### Playbook File: install_multiple_packages.yaml

```yaml
- hosts: "{{ group }}"
  become: yes

  vars_prompt:
    - name: group
      prompt: Enter target group
      private: no

    - name: packages
      prompt: Enter packages to install (comma-separated)
      private: no

  gather_facts: true

  tasks:
    - name: Install packages with apt
      apt:
        name: "{{ item }}"
        state: present
      with_items: "{{ packages.split(',') }}"
      when: ansible_facts['os_family'] == "Debian"

    - name: Install packages with yum
      yum:
        name: "{{ item }}"
        state: present
      with_items: "{{ packages.split(',') }}"
      when: ansible_facts['os_family'] == "RedHat"
```
