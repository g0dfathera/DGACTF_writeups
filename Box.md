![[Pasted image 20250517154247.png]]

![[Pasted image 20250517154457.png]]

საიტზე არის linux-ის CLI (Command line interface), ჩვენს user-ს არ აქვს suid, რის გამოც გვაქვს შეზღუდული უფლებები.

მინიშნებაში ნახსენებია სერვისები რომელიც მუშაობს სერვერზე. ვნახოთ ეს სერვისები top ბრძანებით.

![[Pasted image 20250517154730.png]]

ყველა აქტიური სერვისი არის ნაცნობი გარდა ერთისა - 

```
/opt/service/vulnservice
```

ვცადოთ სერვისის გათიშვა - kill 980 (980 არის სერვისის PID) 

![[Pasted image 20250517154953.png]]

სერვისის გათიშვა არ შეგვიძლია, ამისთვის root-ის პრივილეგია არის საჭირო.

მოდი ვნახოთ რა ფუნქციას ასრულებს სერვისი strings-ის საშუალებით.

![[Pasted image 20250517155455.png]]

ძალიან მნიშვნელოვანი ინფორმაცია გავიგეთ strings-ით.
პირველ რიგში ის რომ სერვისი უსმენს localhost-ზე 4444 პორტს.
ასევე ის იყენებს cowsay ბრძანებას. მოდი netcat-ით მოვუსმინოთ 4444 პორტს და გამოვიყენოთ სერვისი.

![[Pasted image 20250517155725.png]]

სერვისი უბრალოდ იძახებს cowsay ბრძანებას და ამბობს იმას, რასაც ჩვენ მივუთითებთ. შეიძლება ამის გამოყენება Privilege escalation-ისათვის.
შევეცადოთ თავი დავაღწიოთ cowsay-ს და მივიღოთ წვდომა კონსოლთან.

![[Pasted image 20250517155923.png]]


როგორც ჩანს, სერვისს აქვს root პრივილეგია, გამოვიყენოთ ეს სერვისი და ავიღოთ flag.txt რომელიც root დირექტორიაშია.

![[Pasted image 20250517160145.png]]
