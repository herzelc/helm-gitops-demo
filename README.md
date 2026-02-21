ArgoCD UI showing apps : 

<img width="781" height="410" alt="image" src="https://github.com/user-attachments/assets/8a17bfdc-e15f-4ca8-99e5-2d7e58b7ea9a" />


Dev namespace pods:
myapp-dev-65ff4f4f8f-8pld4              1/1     Running     0              28m
myapp-dev-65ff4f4f8f-hmpv9              1/1     Running     0              8m30s

<img width="627" height="139" alt="image" src="https://github.com/user-attachments/assets/4fbb3c48-a89f-4a0d-95f1-c2adae10c045" />


Prod namespace pods
PS C:\Users\Herzel\lesson8> kubectl get pods -n prod
NAME                          READY   STATUS    RESTARTS   AGE
myapp-prod-5f87f57555-d7sj9   1/1     Running   0          25m
myapp-prod-5f87f57555-hwm6m   1/1     Running   0          25m
myapp-prod-5f87f57555-l5xsd   1/1     Running   0          25m

<img width="449" height="92" alt="image" src="https://github.com/user-attachments/assets/4037a206-32a1-4dd0-a829-d5d06d2202e7" />



איך ArgoCD מתחבר ל-GitHub -

ה ArgoCD מתחבר ל-GitHub כדי לקרוא את הקוד של המתכנת.

אנחנו מגדירים ל-ArgoCD:

מה כתובת ה-Repository

איזה Branch לעקוב אחריו (למשל dev או prod)

איפה נמצא ה-Helm chart בתוך ה-repo

אחרי זה ArgoCD:

מוריד (clone) את הקבצים מ-GitHub

בודק אם יש שינויים חדשים

אם יש שינוי — הוא מעדכן את ה-Kubernetes לפי הקוד ב-Git

ה GitHub הוא ה"אמת". ArgoCD כל הזמן בודק שהוא מסונכרן איתו.




איך Helm יוצר את הקבצים?

ה Helm הוא כלי שמייצר קבצי Kubernetes בצורה דינמית.

ב-Helm יש:

קבצי תבנית (templates)

קבצי values (למשל values-dev.yaml, values-prod.yaml)

ה Helm לוקח את התבניות ואת הערכים (כמו replicaCount: 2) ומייצר קובץ Kubernetes אמיתי מוכן להרצה.

לדוגמה:
אם ב-values כתוב replicaCount: 2, ה Helm ייצור Deployment עם 2 replicas.
ה ArgoCD משתמש ב-Helm כדי לייצר את הקבצים לפני שהוא שולח אותם ל-Kubernetes.


איך עובד התהליך ?

ה Reconciliation זה התהליך שבו ArgoCD דואג שה-Cluster יהיה זהה ל-Git.

הוא כל הזמן משווה בין מה שיש ב-GitHub לבין מה שרץ בפועל ב-Kubernetes

אם יש הבדל (למשל שינינו replicaCount), ה ArgoCD מזהה שיש חוסר התאמה (OutOfSync)

הוא עושה Sync, ה Kubernetes מעדכן את ה-Deployment ונוצרים פודים חדשים לפי הערך החדש

ה  ArgoCD בודק כל הזמן שהמצב בפועל תואם למה שכתוב בקוד.
אם לא — הוא מתקן.




