# Lab 03 - SSH Brute Force Detection

## Objective

Simulate and investigate an SSH brute force attack by generating multiple failed login attempts against an Ubuntu server and analyzing the authentication logs.

---

## Skills Demonstrated

- Linux log analysis
- SSH authentication monitoring
- Brute force attack detection
- Security event investigation
- Incident documentation
- SOC Level 1 analysis

---

## Lab Environment

| Component | Description |
|-----------|-------------|
| Attacker | Kali Linux |
| Target | Ubuntu Server |
| Service | OpenSSH |
| Hypervisor | VirtualBox |

---

## Attack Scenario

A threat actor attempts to gain unauthorized access to an Ubuntu server by repeatedly trying different usernames and incorrect passwords over SSH.

The SOC analyst investigates authentication logs to identify indicators of brute force activity.

---

## Tools Used

- Ubuntu
- Kali Linux
- OpenSSH
- grep
- cat
- wc
- Linux Terminal

---

## Steps Performed

### Step 1 – Verify SSH Service

Ensure the SSH service is installed and running.

Screenshot:

```
capturas/verificacion-servicio-ssh.png
```

---

### Step 2 – Identify Target IP Address

Display the server IP address.

Screenshot:

```
capturas/ip-servidor.png
```

---

### Step 3 – Generate Failed Login Attempts

From Kali Linux:

```
ssh admin@<Target-IP>
```

Enter an incorrect password several times.

Repeat using multiple usernames.

Screenshot:

```
capturas/generacion-ataque.png
```

---

### Step 4 – Review Authentication Logs

Execute:

```
sudo grep "Failed password" /var/log/auth.log
```

Screenshot:

```
capturas/revision-registros.png
```

---

### Step 5 – Count Failed Attempts

Execute:

```
sudo grep "Failed password" /var/log/auth.log | wc -l
```

Screenshot:

```
capturas/conteo-eventos.png
```

---

### Step 6 – Identify the Source IP

Locate the attacking IP address.

Screenshot:

```
capturas/ip-atacante.png
```

---

## Indicators of Compromise (IOCs)

- Multiple failed SSH logins
- Repeated authentication failures
- Same source IP
- Attempts against privileged accounts
- Consecutive login failures

---

## Findings

- Multiple failed SSH authentication attempts were detected.
- Authentication logs clearly recorded each attempt.
- The activity originated from a single IP address.
- Attack behavior is consistent with SSH brute force attacks.

---

## Recommendations

- Disable SSH root login.
- Use SSH key authentication.
- Implement Fail2Ban.
- Enforce strong password policies.
- Monitor authentication logs continuously.

---

## Repository Structure

```
Lab-03-SSH-Brute-Force-Detection/
│
├── README.md
├── Report/
│   └── Lab03_Report.pdf
│
└── Screenshots/
    ├── step1_ssh_service.png
    ├── step2_server_ip.png
    ├── step3_failed_login.png
    ├── step4_auth_log.png
    ├── step5_attempt_count.png
    └── step6_attacker_ip.png
```

---

## MITRE ATT&CK

| Technique | Description |
|------------|------------|
| T1110.001 | Password Guessing |

---

## Author

Juan Pablo González

SOC Analyst Portfolio
