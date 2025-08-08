# Day 3: Timedatectl, `at`, and `cron`

## 🕒 timedatectl

### Check Time Status

```bash
timedatectl status
```

### Set Timezone

```bash
sudo timedatectl set-timezone Africa/Cairo
```

---

## 🧨 `at` Command (Run once at specific time)

### Example

```bash
echo 'echo hello >> /tmp/test.txt' | at now + 1 minute
```

### Check Jobs

```bash
atq
```

### Remove Job

```bash
atrm <job-id>
```

---

## ⏰ cron (Repeat jobs)

### Edit User Cron Jobs

```bash
crontab -e
```

### Syntax

```bash
* * * * * command
```

| Field | Meaning        |
|-------|----------------|
| *     | minute         |
| *     | hour           |
| *     | day of month   |
| *     | month          |
| *     | day of week    |

### Example: Run every minute

```bash
* * * * * echo "Hello" >> /tmp/test.log
```

### List Cron Jobs

```bash
crontab -l
```

### Remove All Cron Jobs

```bash
crontab -r
```
