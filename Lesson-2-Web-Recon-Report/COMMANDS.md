# Lesson 2 Command Reference

## Service discovery

```bash
nmap TARGET
nmap -sV TARGET
```

## HTTP technology fingerprinting

```bash
whatweb http://TARGET
whatweb http://HOSTNAME
```

## HTTP inspection

```bash
curl http://HOST
curl -I http://HOST
curl -v http://HOST
```

## Local hostname mapping

```bash
cat /etc/hosts
sudo nano /etc/hosts
```

Example lab mapping:

```text
TARGET_IP target.htb
```

## Local Apache check

```bash
curl http://127.0.0.1
```

## Notes

Use only against authorized lab targets. Hostnames and IP addresses in screenshots are lab-specific and may change between spawned instances.
