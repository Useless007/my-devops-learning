# Kubectl Cheatsheet: คำสั่งเอาตัวรอด

## Basic Commands (พื้นฐาน)
| Action | Command | Note |
| :--- | :--- | :--- |
| **ดูสถานะ** | `kubectl get pods` | เติม `-w` เพื่อดูแบบ Real-time |
| **ดูที่อยู่ Pod** | `kubectl get pods -o wide` | **สำคัญ** ดูว่า Pod ไปลง Node เครื่องไหน |
| **ดูทุกอย่าง** | `kubectl get all` | ดู Pod, Svc, Deploy, RS ทั้งหมด |
| **ดูไส้ใน** | `kubectl describe pod <name>` | **สำคัญมาก** ใช้ดูว่าทำไม Pod ไม่ขึ้น (ดู Events ด้านล่างสุด) |
| **ดู Log** | `kubectl logs <name>` | ดูว่าแอปพ่น Error อะไรออกมา |
| **ดู Log (Live)**| `kubectl logs -f <name>` | ดู Log ไหลแบบ Real-time |
| **ดู Log ตัวก่อนหน้า**| `kubectl logs <name> --previous` | **Day 3** ดู Log ของ Pod ตัวที่เพิ่งตายไป (CrashLoopBackOff) |
| **เข้า Pod** | `kubectl exec -it <name> -- sh` | มุดเข้าไปรันคำสั่งข้างใน (บาง Image อาจต้องใช้ `/bin/bash`) |

## Scaling & Resources (ขยายกองทัพ & ทรัพยากร) ⚖️ (New!)
| Action | Command | Note |
| :--- | :--- | :--- |
| **ปรับจำนวน Pod** | `kubectl scale deploy/<name> --replicas=3` | **Day 3** เพิ่ม/ลดจำนวน Pod ด้วยมือ (Manual Scaling) |
| **ดูสเปคเครื่อง** | `kubectl describe node <name>` | **Day 3** ดู CPU/RAM Capacity และยอดที่ถูกจอง (Limits) |

## Node & Scheduling (จัดการเครื่อง) 🆕
| Action | Command | Note |
| :--- | :--- | :--- |
| **ดู Label** | `kubectl get nodes --show-labels` | เช็คยศ/ป้ายกำกับของ Node |
| **ติดป้าย** | `kubectl label nodes <name> key=val` | เช่น `hardware=gpu` |
| **ลบป้าย** | `kubectl label nodes <name> key-` | ใส่เครื่องหมายลบ `-` หลัง key |
| **แปะ Taint** | `kubectl taint nodes <name> k=v:Effect` | เช่น `security=high:NoSchedule` |
| **ลบ Taint** | `kubectl taint nodes <name> k:Effect-` | อย่าลืมเครื่องหมายลบ `-` ตอนท้าย |

## Creation & Deletion (สร้างและทำลาย)
| Action | Command | Note |
| :--- | :--- | :--- |
| **Apply File** | `kubectl apply -f <filename.yaml>` | สร้างหรืออัปเดต Resource จากไฟล์ |
| **Restart** | `kubectl rollout restart deploy/<name>` | ใช้เมื่อแก้ Secret/ConfigMap แบบ Env |
| **Delete Pod** | `kubectl delete pod <name>` | ลบ Pod (ถ้าเป็น Deploy มันจะงอกใหม่) |
| **Delete Deploy** | `kubectl delete deployment <name>` | **Day 3** ลบ Deployment (หายทั้งกองทัพ) |
| **Delete Service**| `kubectl delete service <name>` | **Day 3** ลบ Service (ทางเข้า) |
| **Delete File**| `kubectl delete -f <filename.yaml>` | ลบทุกอย่างที่ระบุในไฟล์ |

## Debugging Tips
* ใช้ `kubectl describe pod` เมื่อ Pod สถานะไม่เป็น Running (เช็ค Events)
* ใช้ `kubectl describe node` เมื่อ Pod ขึ้น Pending เพราะทรัพยากรไม่พอ (เช็ค CPU/RAM)
* ใช้ `kubectl logs` เมื่อ Pod Running แต่แอปทำงานผิดปกติ