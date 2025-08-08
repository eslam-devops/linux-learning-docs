# Day 2: bashrc Customization and Useful Aliases

## ⚙️ Open `.bashrc`

```bash
nano ~/.bashrc
```

## ➕ Add Custom Aliases

```bash
alias gs='git status'
alias ll='ls -la'
alias update='sudo apt update && sudo apt upgrade -y'
```

## ✅ Reload bashrc

```bash
source ~/.bashrc
```

## 🧠 Useful Notes

- Use `alias` to save time
- Put aliases at the bottom of the file
- Use `#` for comments
- Keep it clean and organized

## 💡 Tip

To see all aliases:

```bash
alias
```
