![[Pasted image 20250518130612.png]]

პირველ რიგში, ვნახოთ მინიშნება.

![[Pasted image 20250518130653.png]]

knocking on heaven's door? შეიძლება Heaven's door რაიმე ტიპის Cloud სერვისს ნიშნავდეს.

მოცემული IP შევამოწმოთ nmap-ით.

![[Pasted image 20250518131008.png]]

IP მისამართზე გახსნილია რამდენიმე პორტი.

ვნახოთ რა ხდება 80 პორტზე.

![[Pasted image 20250518131113.png]]

Main page-ზე არის Apache-ს სერვერის default გვერდი. შეიძლება არის სხვა route-ებიც ვებგვერდზე. bruteforce-სთვის გამოვიყენებ ffuf ხელსაწყოს.

![[Pasted image 20250518131559.png]]

აღმოვაჩინეთ ახალი route - /cloud

![[Pasted image 20250518131657.png]]


index /cloud-ში არის რამდენიმე ფაილი, ყველა ფაილი საჭიროა cloud.php-ს მუშაობისთვის. როცა file.txt დავინახე მეგონა რომ flag ამ ფაილში იქნებოდა, მაგრამ აღმოჩნდა რომ ფიალის შიგთავსი სხვა იყო - „Feel the cloud„

გადავიდეთ cloud.php-ზე.

![[Pasted image 20250518131914.png]]

ერთი შეხედვით, ვებგვერდი არის ძალიან მარტივი - გამოაქვს წარწერა რომელი მოთავსებული იყო file.txt-ში, ფონად არის ის ფოტო რომელიც შეგვხვდა index-ში, და აქედანვე აღებული mp3 ფაილიდან იკვრება მუსიკა - ისევ Knocking on Heaven's door.

თუ დავაკვირდებით ვებგვერდის URL-ს, დავინახავთ შესაძლო სისუსტეს - LFI-ს (Local file inclusion)

მოდი ვცადოთ მარტივი payload:

```
http://172.105.92.188/cloud/cloud.php?welcome=../../../../etc/passwd
```

![[Pasted image 20250518132359.png]]

როგორც ჩანს, LFI ნამდვილად აქვს ამ საიტს.

თუ გავიხსენებთ nmap-ის შედეგებს, დავინახავთ რომ 22 პორტი (SSH) გახსნილი არის. ამიტომ შევეცადე id_rsa-ს ამოღებას (რათა შევსულიყავი სერვერზე SSH-ის გამოყენებით და სავარაუდოდ, ამეღო flag). მაგრამ ყველა ჩემი მცდელობა უშედეგო გამოდგა. 

ამიტომ ვცადე თვითონ cloud.php-ს კოდი წამეკითხა LFI-ს გამოყენებით, იქნებ იქ მენახა რაიმე საინტერესო.

![[Pasted image 20250518132840.png]]

browser-ში საინტერესო ვერაფერი ვნახე, მაგრამ ვნახე curl-ითაც შემემოწმებინა იგივე მისამართი.

![[Pasted image 20250518133009.png]]

წინსვლა გვაქვს. ვნახეთ მინიშნება - 

```
$hinti="portze unda daakakuno megobaro"
```

პირველი მინიშნებაც უკვე გასაგებია - knocking on heaven's door სერვერის პორტებზე knocking-ს / დაკაკუნებას ნიშნავდა.

მოდი კიდევ ერთხელ გავუშვათ nmap, ახლა უფრო მეტი პორტის შესამოწმებლად.

![[Pasted image 20250518133400.png]]

ვიპოვეთ კიდევ ერთი პორტი - 65124 , რომელიც დახურულია. პორტის გასახსნელად არის საჭირო სხვა პორტებზე რაიმე კონკრეტული პატერნით "დაკაკუნება". სწორი პატერნით დაკაკუნების შემდეგ პორტი უნდა გაიხსნას მცირე დროის განმავლობაში.

თავიდან ვცადე bash სკრიპტის დაწერა რომელიც ავტომატურად სხვადასხვა პატერნებით დააკაკუნებდა იმ პორტებზე რომლებიც უკვე ვნახეთ nmap-ში - 23; 25; 22; 80. თუმცა ეს ცდა უშედეგოდ დასრულდა, ვერანაირად ვერ გავხსენი პორტი.

შემდეგ გამახსენდა LFI, და ის, რომ სერვერზე აყენია ლინუქსი. ლინუქსის ოპერაციულ სისტემაში ხშირად შევხვდებით ასეთ config ფაილს - knockd.conf

ვცადე ამ ფაილის კონტენტის წაკითხვა და გამომივიდა.

![[Pasted image 20250518134055.png]]

ვნახეთ ის პორტები და ის დროის ინტერვალი, რომელზე დაკაკუნებაცაა საჭირო 65124 პორტის გასახსნელად.

ამის გასაკეთებლად დავწერე bash სკრიპტი.

![[Pasted image 20250518134338.png]]

გავუშვი სკრიპტი და მომცა flag

![[Pasted image 20250518134501.png]]


