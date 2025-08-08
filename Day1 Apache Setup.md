# Day 1: Apache Setup and Systemd Basics

## 🔧 Installing Apache

```bash
sudo apt update
sudo apt install apache2
```

## 🧪 Testing Apache

- Open browser → `http://your-server-ip`
- You should see the Apache default page

## 🛠️ systemctl Basics

### Check Apache Status

```bash
sudo systemctl status apache2
```

### Start Apache

```bash
sudo systemctl start apache2
```

### Stop Apache

```bash
sudo systemctl stop apache2
```

### Restart Apache

```bash
sudo systemctl restart apache2
```

### Enable on Boot

```bash
sudo systemctl enable apache2
```

### Disable on Boot

```bash
sudo systemctl disable apache2
```

## 📌 Systemctl Cheatsheet

| Command | Description |
|--------|-------------|
| `systemctl status apache2` | Check status |
| `systemctl start apache2` | Start service |
| `systemctl stop apache2` | Stop service |
| `systemctl restart apache2` | Restart |
| `systemctl enable apache2` | Start on boot |
| `systemctl disable apache2` | Don't start on boot |
