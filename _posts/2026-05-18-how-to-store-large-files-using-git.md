## Hugging Face to store large files in git

# 1. Create Account

Go to:

[Hugging Face](https://huggingface.co/join)

Create an account.

---

# 2. Install Git LFS

On Ubuntu/Debian:

```bash
sudo apt update
sudo apt install git-lfs
```

Initialize:

```bash
git lfs install
```

---

# 3. Create a Dataset Repository

Open:

[New Dataset Repo](https://huggingface.co/new-dataset)

---

# 4. Generate Access Token

Open:

[Access Tokens](https://huggingface.co/settings/tokens)

Create token with:

* Write permission

Copy the token. This token can be used when push it git repo

---


# 5. Clone Dataset Repo

Replace USERNAME and DATASET:

```bash
git clone https://huggingface.co/datasets/USERNAME/DATASET
```

Example:

```bash
git clone https://huggingface.co/datasets/suhailvs/kerala-html-dataset
```

---


# 6. Commit and Push

```bash
cd kerala-html-dataset

git add .
git commit -m "Add batch 001"
git push
```

Large files are automatically handled by Git LFS.


# 7. optional login from terminal

```
pip install huggingface_hub
huggingface-cli login
```