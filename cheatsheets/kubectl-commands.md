# Kubectl Cheatsheet: คำสั่งเอาตัวรอด

## Basic Commands (พื้นฐาน)
| Action | Command | Note |
| :--- | :--- | :--- |
| **ดูสถานะ** | `kubectl get pods` | เติม `-w` เพื่อดูแบบ Real-time |
| **ดูทุกอย่าง** | `kubectl get all` | ดู Pod, Svc, Deploy, RS ทั้งหมด |
| **ดูไส้ใน** | `kubectl describe pod <name>` | **สำคัญมาก** ใช้ดูว่าทำไม Pod ไม่ขึ้น (ดู Events ด้านล่างสุด) |
| **ดู Log** | `kubectl logs <name>` | ดูว่าแอปพ่น Error อะไรออกมา |
| **ดู Log (Live)**| `kubectl logs -f <name>` | ดู Log ไหลแบบ Real-time |
| **เข้า Pod** | `kubectl exec -it <name> -- sh` | มุดเข้าไปรันคำสั่งข้างใน (บาง Image อาจต้องใช้ `/bin/bash` หรือ `bash`) |

## Creation & Updates (สร้างและแก้ไข)
| Action | Command | Note |
| :--- | :--- | :--- |
| **Apply File** | `kubectl apply -f <filename.yaml>` | สร้างหรืออัปเดต Resource จากไฟล์ |
| **Restart** | `kubectl rollout restart deploy/<name>` | ใช้เมื่อแก้ Secret/ConfigMap แบบ Env |
| **Delete** | `kubectl delete pod <name>` | ลบ Pod (ถ้าเป็น Deploy มันจะงอกใหม่) |
| **Delete File**| `kubectl delete -f <filename.yaml>` | ลบทุกอย่างที่ระบุในไฟล์ |

## Debugging Tips
* ใช้ `kubectl describe` เมื่อ Pod สถานะไม่เป็น Running
* ใช้ `kubectl logs` เมื่อ Pod Running แต่แอปทำงานผิดปกติ
