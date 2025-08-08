## 🧠 فهم ملف `.bashrc` في لينوكس

### ✅ يعني إيه `.bashrc`؟

ده سكريبت بيتنفذ أوتوماتيك كل مرة تفتح terminal جديدة لو شغال بـ Bash

---

## 💡 إزاي تستفيد منه؟

### 1. ✅ Aliases

```bash
alias ll='ls -alF'
alias gs='git status'
alias gco='git checkout'
alias ..='cd ..'
alias cls='clear'
```

---

### 2. ✅ Environment Variables

```bash
export EDITOR=vim
export PROJECTS_DIR=~/my_work
export PATH=$PATH:/opt/tools/bin
```

---

### 3. ✅ تخصيص الـ Prompt

```bash
export PS1="\[\e[32m\]\u@\h:\w\$ \[\e[m\]"
```

---

### 4. ✅ تشغيل سكريبت تلقائيًا

```bash
source ~/myenv/bin/activate
echo "أهلاً بيك يا زين! 👋"
```

---

### 5. ✅ Bash Functions

```bash
extract () {
  if [ -f $1 ]; then
    case $1 in
      *.tar.bz2)   tar xjf $1   ;;
      *.tar.gz)    tar xzf $1   ;;
      *.bz2)       bunzip2 $1   ;;
      *.rar)       unrar x $1   ;;
      *.gz)        gunzip $1    ;;
      *.tar)       tar xf $1    ;;
      *.tbz2)      tar xjf $1   ;;
      *.tgz)       tar xzf $1   ;;
      *.zip)       unzip $1     ;;
      *)           echo "نوع الملف مش معروف" ;;
    esac
  else
    echo "الملف غير موجود"
  fi
}
```

---

### 6. ✅ إضافة التاريخ للأوامر السابقة

```bash
export HISTTIMEFORMAT="%F %T "
```

---

### 7. ✅ Aliases لأدوات خارجية

```bash
alias dcu='docker compose up -d'
alias dcd='docker compose down'
alias awslogin='aws sso login --profile work'
```

---

### 8. ✅ تشغيل tmux تلقائيًا

```bash
if command -v tmux &> /dev/null && [ -z "$TMUX" ]; then
    tmux attach -t default || tmux new -s default
fi
```

---

## 🧠 نصايح مهمة

* أي تعديل اعمله reload:

```bash
source ~/.bashrc
```

* خليك منظم بكتابة تعليقات
* اعمل نسخة احتياطية للملف

---

## 🎁 Terminal Welcome Message (عربي + إنجليزي)

```bash
echo -e "\e[1;36m----------------------------------------\e[0m"
echo -e "\e[1;33mWelcome, \e[1;32m$(whoami)\e[0m!"
echo -e "\e[1;34mDate: \e[1;35m$(date)\e[0m"
echo -e "\e[1;34mHostname: \e[1;35m$(hostname)\e[0m"
echo -e "\e[1;34mOS: \e[1;35m$(uname -srm)\e[0m"
echo -e "\e[1;34mCurrent dir: \e[1;35m$(pwd)\e[0m"
echo -e "\e[1;36m----------------------------------------\e[0m"
```

---

## ✅ إضافة فولدر للـ PATH

### الطريقة الصح:

```bash
export PATH="$PATH:$HOME/zain_folder"
```

أو:

```bash
export PATH="$PATH:/opt/zain_folder"
```

### الطريقة الغلط:

```bash
export PATH="$PATH:/zain_folder "
```

ليه؟ لأنه في root وفيه مسافة في الآخر

---

## 🧪 تجربة سريعة

```bash
mkdir ~/zain_folder
```

```bash
echo -e '#!/bin/bash\necho "اهلا يا زين"' > ~/zain_folder/hello
chmod +x ~/zain_folder/hello
```

```bash
echo 'export PATH="$PATH:$HOME/zain_folder"' >> ~/.bashrc
source ~/.bashrc
```

```bash
hello
```

---

## 🔧 سكريبت لإضافة فولدر للـ PATH تلقائيًا

```bash
#!/bin/bash
FOLDER=$1
if [[ -d $FOLDER ]]; then
  echo "export PATH=\"\$PATH:$FOLDER\"" >> ~/.bashrc
  echo "تمت الإضافة بنجاح. نفذ source ~/.bashrc"
else
  echo "الفولدر مش موجود: $FOLDER"
fi
```

نفذه كده:

```bash
bash add_to_path.sh ~/zain_folder
```

---

## 📁 اسم السكربت: `check_internet.sh`

```
#!/bin/bash

# نجرب نعمل ping على جوجل
ping -c 1 8.8.8.8 > /dev/null 2>&1

# لو النت شغال
if [ $? -eq 0 ]; then
    echo "✅ النت شغال"
else
    echo "❌ مفيش نت"
fi

```

---

## 🧠 شرح

* `ping -c 1 8.8.8.8`: بنبعت Ping مرة واحدة على جوجل
* `> /dev/null 2>&1`: نخفي كل الإخراج (النجاح والخطأ)
* `if [ $? -eq 0 ]`: بنسأل هل الأمر نجح؟
* بنطبع رسالة على حسب النت شغال ولا لأ

---

## ✅ تجربة السكربت:

1. احفظه في ملف:

```
vim ~/zain_folder/check_internet.sh

```

2. خليه قابل للتنفيذ:

```
chmod +x ~/zain_folder/check_internet.sh

```

3. شغله:

```
./zain_folder/check_internet.sh

```

---

## ⏱ استخدامه في كرون:

عايز كل 5 دقايق يتشيك على النت، ولو وقع يكتب في لوج:

```
*/5 * * * * /home/zain/zain_folder/check_internet.sh >> /var/log/net_check.log 2>&1

```

---

