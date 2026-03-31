# Deploy Web Application ด้วย Terraform บน AWS

## ภาพรวมโปรเจกต์

โปรเจกต์นี้เป็นการนำ Web Application ที่พัฒนาด้วย **Python (FastAPI)** ร่วมกับ **HTML Frontend** ไปติดตั้งบนระบบ Cloud โดยใช้แนวคิด **Infrastructure as Code (IaC)** ผ่าน Terraform เพื่อจัดการและสร้างโครงสร้างพื้นฐานแบบอัตโนมัติบน AWS

ภายในระบบจะมีการทำงานดังนี้:

* สร้าง EC2 Instance สำหรับรันแอป
* ตั้งค่า Security Group เพื่อเปิดพอร์ตที่จำเป็น
* สร้าง SSH Key สำหรับเชื่อมต่อ
* ดึงซอร์สโค้ดจาก GitHub
* ติดตั้ง Dependencies ที่จำเป็น
* เปิดใช้งาน FastAPI Server

ในส่วนของฐานข้อมูล ระบบจะเชื่อมต่อกับ **Neon PostgreSQL** ซึ่งเป็นฐานข้อมูลแบบ Cloud

---

## โครงสร้างโปรเจกต์

```
NUDPACK/
│
├── terraform/
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   ├── terraform.tfstate
│   └── terraform.tfstate.backup
│
├── deploy.sh
└── README.md
```

### 🔎 รายละเอียดไฟล์สำคัญ

* **main.tf**
  ใช้กำหนด Resource ต่าง ๆ ที่จะสร้างบน AWS เช่น EC2, Security Group และ Key Pair

* **variables.tf**
  ใช้เก็บค่าตัวแปร เช่น region หรือ instance type เพื่อให้ปรับค่าได้ง่าย

* **outputs.tf**
  ใช้แสดงผลลัพธ์หลังจาก deploy เช่น URL สำหรับเข้าใช้งานระบบ

---

## 🛠 เทคโนโลยีที่ใช้

* Terraform
* AWS EC2
* FastAPI
* Uvicorn
* PostgreSQL (Neon)
* GitHub

---

## ขั้นตอนการติดตั้งและใช้งาน

### 1 ติดตั้งเครื่องมือพื้นฐาน

ก่อนเริ่มใช้งาน ต้องติดตั้งโปรแกรมเหล่านี้ให้เรียบร้อย:

* Terraform
* AWS CLI
* Git

ตรวจสอบว่าใช้งานได้ด้วยคำสั่ง:

```
terraform -version
aws --version
git --version
```

---

### 2️ ตั้งค่า AWS CLI

ต้องเชื่อมต่อ AWS ก่อนใช้งาน Terraform

ใช้คำสั่ง:

```
aws configure
```

แล้วกรอกข้อมูล:

```
AWS Access Key ID
AWS Secret Access Key
Region (เช่น ap-southeast-1)
Output format (json)
```

---

### 3️ Clone โปรเจกต์

ดาวน์โหลดโปรเจกต์จาก GitHub:

```
git clone <repository-url>
cd NUDPACK/terraform
```

---

### 4️ เริ่มต้น Terraform

รันคำสั่ง:

```
terraform init
```

เพื่อให้ Terraform เตรียม environment และดาวน์โหลด provider ที่จำเป็น

---

### 5️ ตรวจสอบแผนการสร้างระบบ

ใช้คำสั่ง:

```
terraform plan
```

คำสั่งนี้จะแสดงรายการสิ่งที่ Terraform จะสร้าง เช่น:

* EC2 Instance
* Security Group
* Key Pair

---

### 6️ Deploy ระบบจริง

ใช้คำสั่ง:

```
terraform apply
```

เมื่อระบบถามให้ยืนยัน ให้พิมพ์:

```
yes
```

Terraform จะทำงานดังนี้:

1. สร้าง EC2 Instance
2. ตั้งค่า Security Group
3. ดึงโค้ดจาก GitHub
4. ติดตั้ง Python และ Dependencies
5. รัน FastAPI Server

หลังจากเสร็จ จะได้ URL เช่น:

```
app_public_url = http://<EC2_PUBLIC_IP>:8000
```

---

### 7️ ตรวจสอบการทำงาน

เปิด Web Browser แล้วเข้าไปที่:

```
http://<EC2_PUBLIC_IP>:8000

    หลัง :8000 ให้ต่อด้วย 
      /client (สำหรับคนส่งพัสดุ)
      /admin (สำหรับแอดมิน) แต่เนื่องจากระบบมีการใช้งานจริงที่สำนักงานกองกิจ ดังนั้นจึงไม่สามารถให้รหัสผ่านของ admin ได้ค่ะ
      /recipient (สำหรับคนรับพัสดุ)
```

หากระบบทำงานถูกต้อง จะสามารถใช้งานแอปได้ และเชื่อมต่อฐานข้อมูล Neon ได้เรียบร้อย

ตัวอย่าง
---
สำหรับคนส่งพัสดุ(ขนส่งเอกชน)
```
  http://13.212.105.160:8000/client
```
สำหรับแอดมิน
```
  http://13.212.105.160:8000/admin
```
สำหรับคนรับพัสดุ
```
  http://13.212.105.160:8000/recipient
```

### 8️ ลบระบบเมื่อใช้งานเสร็จ

เพื่อป้องกันค่าใช้จ่าย ควรลบ resource หลังใช้งาน

ใช้คำสั่ง:

```
terraform destroy
```

แล้วพิมพ์:

```
yes
```

Terraform จะลบทุกอย่างที่สร้างไว้ เช่น:

* EC2 Instance
* Security Group
* Key Pair

---

## ภาพรวมการทำงาน

```
Terraform
   │
   ▼
AWS EC2
   │
   ├── ติดตั้ง Python
   ├── Clone โค้ดจาก GitHub
   ├── ติดตั้ง Dependencies
   └── รัน FastAPI
            │
            ▼
      Neon PostgreSQL
```

ผู้ใช้สามารถเข้าถึงระบบผ่าน Browser โดยใช้ IP ของ EC2

---

## หมายเหตุสำคัญ

ก่อนใช้งาน Terraform ทุกครั้ง จำเป็นต้องตั้งค่า AWS CLI ให้เรียบร้อย หากยังไม่ได้ตั้งค่า ระบบจะไม่สามารถสร้าง Infrastructure บน AWS ได้

---
