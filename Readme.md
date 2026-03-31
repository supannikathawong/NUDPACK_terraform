# โครงการ Deploy Web Application ด้วย Terraform บน AWS

## ภาพรวมโครงการ

1. แนวคิดของโครงการ

โครงการนี้มีวัตถุประสงค์เพื่อพัฒนาและนำระบบ Web Application ขึ้นใช้งานบน Cloud โดยใช้แนวคิด Infrastructure as Code (IaC) ผ่าน Terraform เพื่อให้สามารถสร้างและจัดการโครงสร้างพื้นฐานได้อย่างอัตโนมัติ

ตัวระบบพัฒนาด้วย Python FastAPI สำหรับฝั่ง Backend และ HTML สำหรับฝั่ง Frontend พร้อมเชื่อมต่อฐานข้อมูลแบบ Cloud คือ Neon PostgreSQL

การ Deploy จะดำเนินการบน AWS EC2 โดย Terraform จะทำหน้าที่สร้างและตั้งค่าทุกองค์ประกอบของระบบ

2. ขอบเขตการทำงานของระบบ

Terraform จะดำเนินการอัตโนมัติในขั้นตอนต่อไปนี้

สร้าง EC2 Instance สำหรับรันแอปพลิเคชัน
กำหนด Security Group เพื่อควบคุมการเข้าถึง
สร้าง SSH Key Pair สำหรับการเชื่อมต่อ
ดึง Source Code จาก GitHub
ติดตั้ง Python และ Dependencies
เรียกใช้งาน FastAPI Server
3. โครงสร้างโปรเจค
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

รายละเอียดไฟล์สำคัญ

main.tf
ใช้กำหนด Resource หลัก เช่น EC2, Security Group และ Key Pair
variables.tf
ใช้กำหนดค่าพารามิเตอร์ เช่น Region และ Instance Type
outputs.tf
ใช้แสดงผลลัพธ์ เช่น Public URL ของระบบหลัง Deploy
4. เทคโนโลยีที่ใช้
Terraform (Infrastructure as Code)
AWS EC2 (Cloud Server)
Python FastAPI (Backend)
Uvicorn (Application Server)
PostgreSQL (Neon Database)
GitHub (Source Code Management)
5. ขั้นตอนการใช้งานระบบ
5.1 เตรียมเครื่องมือ

ติดตั้งโปรแกรมที่จำเป็น ได้แก่

Terraform
AWS CLI
Git

ตรวจสอบการติดตั้ง

terraform -version
aws --version
git --version
5.2 ตั้งค่า AWS CLI

กำหนดค่าการเชื่อมต่อ AWS

aws configure

กรอกข้อมูล

AWS Access Key ID
AWS Secret Access Key
Region (เช่น ap-southeast-1)
Output format (json)
5.3 เตรียมโปรเจค
git clone <repository-url>
cd NUDPACK/terraform
5.4 เริ่มต้น Terraform
terraform init

ใช้สำหรับเตรียมระบบและติดตั้ง Provider

5.5 ตรวจสอบแผนการสร้างระบบ
terraform plan

แสดงรายการ Resource ที่จะถูกสร้าง เช่น

EC2 Instance
Security Group
Key Pair
5.6 Deploy ระบบ
terraform apply

พิมพ์ yes เพื่อยืนยัน

ระบบจะดำเนินการ

สร้าง Infrastructure บน AWS
ติดตั้งระบบและ Dependencies
รัน FastAPI Server

เมื่อเสร็จสิ้น จะได้ URL สำหรับเข้าใช้งาน เช่น

http://<EC2_PUBLIC_IP>:8000
5.7 ทดสอบการทำงาน

เปิด Web Browser และเข้า URL ที่ได้

ระบบจะสามารถใช้งานได้ และเชื่อมต่อกับฐานข้อมูล Neon PostgreSQL ได้เรียบร้อย

5.8 ลบระบบเมื่อไม่ใช้งาน
terraform destroy

ยืนยันด้วย yes

ระบบจะลบ Resource ทั้งหมด เช่น

EC2 Instance
Security Group
Key Pair
6. ภาพรวมการทำงานของระบบ
Terraform
   │
   ▼
Provision Infrastructure (AWS EC2)
   │
   ▼
Deploy Application (FastAPI)
   │
   ▼
Connect Database (Neon PostgreSQL)
   │
   ▼
User Access via Web Browser

# หมายเหตุ

ก่อนการใช้งาน Terraform จำเป็นต้อง Login AWS CLI ก่อนทุกครั้ง หากยังไม่ได้ตั้งค่า AWS CLI ระบบจะไม่สามารถสร้าง Infrastructure บน AWS ได้
