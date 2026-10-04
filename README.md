# Bao cao Bai 4: Quan ly .gitignore va Sua lich su (Amend)

## 1. Muc tieu
- Cau hinh `.gitignore` de tu dong bo qua file nhay cam / file rac.
- Go bo file da commit ra khoi cache theo doi cua Git mot cach an toan.
- Chinh sua thong diep commit gan nhat bang `--amend`.

## 2. Boi canh
Vo tinh commit nham file `credentials.txt` chua thong tin bao mat len Git voi message xau `"add credentials"`.
Can:
1. Go file khoi theo doi ma KHONG lam mat file vat ly.
2. Cau hinh Git bo qua file nay trong tuong lai.
3. Sua lai message commit gan nhat cho sach se.

Rang buoc: Khong dung lenh xoa vat ly cua OS (khong `del`, `rm` file), phai giu lai file o thu muc lam viec.

## 3. Cac buoc thuc hien

### Buoc 1: Tai hien loi (da lam)
```bash
# credentials.txt chua:
# username=admin / password=SuperSecret123 / api_key=...
git add credentials.txt
git commit -m "add credentials"
```

### Buoc 2: Go file khoi cache (khong mat file vat ly)
```bash
git rm --cached credentials.txt
```
- Giai thich: `git rm --cached <file>` chi xoa file khoi **index/staging area** (vung theo doi), giu nguyen file tren dia cung (working directory).
- Khac voi `git rm <file>` (xoa ca trong index + xoa file vat ly) va khac voi `del credentials.txt` (xoa vat ly bang OS, vi pham rang buoc de bai).
- Kiem chung sau lenh: `Test-Path credentials.txt` van `True`, noi dung van nguyen ven.
- Luc nay `git status` hien:
  ```
  deleted:    credentials.txt   (staged - chuan bi xoa khoi lich su)
  Untracked:  credentials.txt   (file vat ly van con, chua duoc ignore)
  ```

### Buoc 3: Cau hinh .gitignore de bo qua trong tuong lai
Noi dung file `.gitignore` (da nop kem):
```
# Bo qua file nhay cam chua thong tin bao mat
credentials.txt

# Bo qua file rac he thong / IDE (mo rong)
*.log
.env
__pycache__/
.vscode/
.DS_Store
Thumbs.db
```
```bash
git add .gitignore
```
- Tu gio Git se tu dong bo qua `credentials.txt` (khong hien Staged/Modified/Untracked).

### Buoc 4: Sua commit gan nhat bang --amend
```bash
git commit --amend -m "Initial commit: them cau hinh du an (da bao mat, loai bo credentials.txt)"
```
- `--amend` thay the commit gan nhat (HEAD) bang commit moi: giu nguyen parent, cap nhat noi dung staged hien tai (.gitignore them vao, credentials.txt bi loai ra) + message moi.
- Luu y: `--amend` viet lai lich su, chi nen dung cho commit chua push / lam viec ca nhan.

## 4. Kiem tra (lenh cua de bai)

### `git status`
```
On branch main
nothing to commit, working tree clean   # (truoc khi tao file bao cao README.md)
```
- Sau khi tao file bao cao `README.md` + file minh chung, `git status` se hien cac file bao cao o dang Untracked - day la binh thuong. Quan trong la `credentials.txt` KHONG xuat hien o dang Staged hay Modified (vi da bi ignore). Dat yeu cau.
- Kiem tra them:
  ```
  git check-ignore -v credentials.txt
  # .gitignore:2:credentials.txt  credentials.txt -> da bi ignore
  git ls-files
  # .gitignore (chi con .gitignore, khong con credentials.txt)
  ```

### `git log -n 1`
```
commit 54a2462d0505f676f9927cc0910f62848a4dccd9
Author: HocVien <hocvien@example.com>
Date:   Fri Oct 2 12:22:23 2026 +0700

    Initial commit: them cau hinh du an (da bao mat, loai bo credentials.txt)
```
- Message da duoc sua thanh cong tu `"add credentials"` -> message sach. Chung minh da dung `--amend`.

### File vat ly van ton tai
```
Test-Path credentials.txt -> True
```

## 5. File nop
- `.gitignore` (da tao).
- Bao cao nay (`README.md`) + ket qua `git log -n 1` va `git status` o tren.
- Luu y bao mat thuc te: neu da push credentials len remote, can xoay (rotate) mat khau/API key + xoa lich su bang BFG/filter-repo, vi amend chi sua local.
