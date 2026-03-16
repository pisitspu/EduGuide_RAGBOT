# 🎓 EduGuide: บอทแนะแนวการศึกษาต่อ (University Info RAG Chatbot)

**EduGuide** คือระบบแชทบอทอัจฉริยะที่ออกแบบมาเพื่อช่วยเหลือนักเรียนชั้น ม.ปลาย (TCAS) และผู้ปกครอง ในการสืบค้นข้อมูลประกอบการตัดสินใจเข้าศึกษาต่อระดับมหาวิทยาลัย โดยขับเคลื่อนด้วยเทคโนโลยี **RAG (Retrieval-Augmented Generation)** เพื่อให้คำตอบที่ถูกต้อง แม่นยำ และอ้างอิงจากข้อมูลจริง (Hard Data) เสมอ

## 🌟 Problem Statement
ข้อมูลสำคัญสำหรับการตัดสินใจเรียนต่อมหาวิทยาลัย เช่น ค่าเทอมตลอดหลักสูตร, ยอดกู้ กยศ., และทุนการศึกษา มักกระจัดกระจายอยู่ในหลายแพลตฟอร์ม (เว็บไซต์มหาวิทยาลัย, ไฟล์ PDF) ทำให้การค้นหาและเปรียบเทียบข้อมูลทำได้ยาก ระบบนี้จึงถูกสร้างขึ้นเพื่อรวบรวมข้อมูลที่มีโครงสร้าง (Structured Data) และให้บริการตอบคำถามแบบอัตโนมัติในที่เดียว

## ✨ Key Features (จุดเด่นของระบบ)
- **ETL Data Pipeline:** ระบบทำความสะอาดและจัดโครงสร้างข้อมูล (Data Cleansing & Structuring) จากไฟล์ PDF และ Web Data ให้พร้อมใช้งาน
- **Semantic Search:** ค้นหาข้อมูลมหาวิทยาลัยด้วยความหมายและบริบท ผ่าน Vector Database
- **No-Code/Low-Code Orchestration:** จัดการ Workflow การทำงานของ AI Agent ด้วย n8n
- **User-Friendly Interface:** ถาม-ตอบง่ายๆ ผ่านแอปพลิเคชัน LINE ที่ทุกคนคุ้นเคย

## 🏗️ System Architecture

ระบบของเราทำงานผ่าน Pipeline ดังนี้:

```mermaid
graph TD
    classDef user fill:#e1bee7,stroke:#8e24aa,stroke-width:2px;
    classDef line fill:#c8e6c9,stroke:#388e3c,stroke-width:2px;
    classDef n8n fill:#ffecb3,stroke:#ffa000,stroke-width:2px;
    classDef ai fill:#bbdefb,stroke:#1976d2,stroke-width:2px;
    classDef db fill:#ffcdd2,stroke:#d32f2f,stroke-width:2px;

    User(["👨‍🎓 นักเรียน / ผู้ปกครอง"]):::user
    LINE["💬 LINE Official Account"]:::line
    Webhook[["⚡ Webhook Trigger"]]:::n8n
    AIAgent{"🤖 AI Agent (n8n)"}:::ai
    VectorDB[("🗄️ Vector Database")]:::db
    LLM["🧠 LLM API"]:::ai

    User -- 1. พิมพ์สอบถามข้อมูล --> LINE
    LINE -- 2. ส่ง Event --> Webhook
    Webhook -- 3. ส่งข้อความ --> AIAgent
    AIAgent -- 4. ค้นหาบริบท (Retrieval) --> VectorDB
    VectorDB -- 5. คืนค่าข้อมูล (ค่าเทอม/ทุน) --> AIAgent
    AIAgent -- 6. ส่ง Prompt + Context --> LLM
    LLM -- 7. ประมวลผลและสรุปคำตอบ --> AIAgent
    AIAgent -- 8. ส่งข้อความกลับ --> LINE
    LINE -- 9. ตอบกลับผู้ใช้งาน --> User