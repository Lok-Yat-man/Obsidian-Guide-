### 第一步：检查是否已有密钥

在终端执行：

Bash

```
ls -al ~/.ssh
```

- **如果有：** 看到 `id_ed25519.pub` 或 `id_rsa.pub`，跳到第三步。
    
- **如果没有：** 看到“No such file or directory”，执行第二步。
    

---

### 第二步：生成新的 SSH 密钥

在终端输入（将邮箱换成你的 GitHub 邮箱）：

Bash

```
ssh-keygen -t ed25519 -C "your_email@example.com"
```

- 一路 **回车** 到底（不需要设置密码，除非你希望每次推送都输入一次密码）。
    
- 完成后，你会得到两个文件：私钥（自己留着）和公钥（给 GitHub）。
    

---

### 第三步：将公钥添加到 GitHub

1. **复制公钥内容：**
    
    Bash
    
    ```
    # Windows 用户使用:
    cat ~/.ssh/id_ed25519.pub | clip
    
    # Linux 用户使用:
    cat ~/.ssh/id_ed25519.pub
    ```
    
    （如果输出是 `id_rsa.pub`，请相应替换文件名。复制那串以 `ssh-ed25519` 开头的长字符串。）
    
2. **去 GitHub 网页端配置：**
    
    - 点击右上角头像 -> **Settings**。
        
    - 左侧菜单找到 **SSH and GPG keys**。
        
    - 点击 **New SSH key**。
        
    - **Title** 随便起（如：`Workstation-Center-Node`）。
        
    - **Key** 粘贴你刚刚复制的内容。
        
    - 点击 **Add SSH key**。
        

---

### 第四步：测试连接（最关键的一步）

回到终端，输入：

Bash

```
ssh -T git@github.com
```

你会看到：

> `The authenticity of host 'github.com (...)' can't be established.`
> 
> `Are you sure you want to continue connecting (yes/no/[fingerprint])?`

**输入 `yes` 并回车。**

如果最后显示：

`Hi xxx! You've successfully authenticated...`

**恭喜你，权限打通了！**

---

### 第五步：重新尝试推送

现在你可以回到 Obsidian 库文件夹，继续之前的推送了：

Bash

```
# 1. 确保在 main 分支（之前那条命令其实是本地操作，不应报 SSH 错，推测你是和 push 一起执行的）
git branch -M main

# 2. 再次推送
git push -u origin main
```

### 💡 为什么会报错？

GitHub 就像一扇关着的门，你本地的 `id_ed25519` 是钥匙，而你在网页端添加的 `Public Key` 是锁。当你执行 `git push` 时，Git 会自动拿着钥匙去对锁。如果锁没装好（没添加 Key），门就打不开。