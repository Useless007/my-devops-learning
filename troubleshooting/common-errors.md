# Troubleshooting & Common Errors

## 1. Status: `CrashLoopBackOff`
* **อาการ:** Pod รันแล้วตาย รันแล้วตาย วนไปเรื่อยๆ
* **สาเหตุที่เป็นไปได้:**
    * โค้ดพัง (Runtime Error)
    * หา Config ไม่เจอ (ConfigMap/Secret ผิด)
    * ต่อ Database ไม่ได้ (Network/Auth ผิด)
    * ไม่มี Command ที่รันค้างไว้ (Container จบการทำงานทันที)
* **วิธีแก้:**
    * เช็ค Log: `kubectl logs <pod-name>`
    * ถ้า Pod ตายเร็วมากจนดู Log ไม่ทัน ให้ใช้ `kubectl logs <pod-name> --previous`

## 2. Status: `Pending`
* **อาการ:** Pod ค้างเป็นสีเหลือง ไม่ยอมรันสักที
* **สาเหตุที่เป็นไปได้:**
    * **ทรัพยากรไม่พอ:** Node CPU/RAM เต็ม
    * **Scheduling ไม่ได้:** ติด NodeSelector, Taint/Toleration
    * **Storage:** PVC ยังไม่ได้ Bound กับ PV (หา Harddisk ไม่เจอ)
* **วิธีแก้:**
    * เช็ค Event: `kubectl describe pod <pod-name>` (ดูส่วน Events ท้ายสุด)

## 3. Status: `ImagePullBackOff` / `ErrImagePull`
* **อาการ:** ดึง Image ไม่มา
* **สาเหตุที่เป็นไปได้:**
    * ชื่อ Image ผิด / Tag ผิด
    * Image เป็น Private Registry แต่ลืมใส่ `imagePullSecrets`
    * Network ของ Node ออกเน็ตไม่ได้
* **วิธีแก้:**
    * เช็คชื่อ Image ใน YAML
    * เช็ค Secret สำหรับ Docker Registry

## 4. แก้ Secret/ConfigMap แล้วค่าไม่เปลี่ยน
* **อาการ:** แก้ไฟล์ YAML แล้ว Apply แล้ว แต่ใน Pod ยังเป็นค่าเดิม
* **สาเหตุ:**
    * ถ้าใช้ `envFrom` หรือ `valueFrom` ค่าจะถูก Inject ตอน Pod เริ่มต้นเท่านั้น ต่อให้แก้ต้นทาง Pod เดิมก็ไม่รู้เรื่อง
* **วิธีแก้:**
    * ต้อง Restart Pod: `kubectl rollout restart deployment/<deployment-name>`
