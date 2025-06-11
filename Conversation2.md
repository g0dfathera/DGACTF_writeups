წინა დავალებაში ნაპოვნი .pcap ფაილი გავხსენი wireshark-ით, და ვნახე Protocol hierarchy.

![[Pasted image 20250518163603.png]]

როგორც ჩანს, პაკეტების ძირითადი ნაწილი არის DNS (Domain Name system) ტიპის.

გავფილტროთ პაკეტები DNS ტიპზე.

![[Pasted image 20250518163739.png]]

თუ დავაკვირდებით domain-ებს, დავინახავთ რომ მხოლოდ ერთი domain არის ვალიდური და ჩვენთვის ნაცნობი - office365.com 

დანარჩენი domain-ები არის შემდეგი ტიპის - 

```
(hex_string).dgactf.com
```

ამოვიღე ყველა Hex string და გარდავქმენი ერთ ბინარულ ფაილად, რომელიც შემდგომში გავაანალიზე სხვადასხვა ხელსაწყოთი.

ამოსაღებად გამოვიყენე tshark (Wireshark-ის CLI ვერსია)

```
tshark -r 1d5015ed19a962bda9bec653e2d72e243ccc30658dd8d4e0_crosstalk.pcap -Y "dns.qry.name" -T fields -e dns.qry.name | uniq
```

შედეგიდან ამოვიღე ისეთი დომეინები რომელიც არ შეიცავდა hex-ს და შევინახე domains.txt-ში.

რადგან domains.txt-ში ყველა hex-ს მიწერილი ქონდა dgactf.com, გავფილტრე ჰექსები და წერტილის შემდეგ დაწერილი domain მოვაშორე.

```
awk -F '.' '{print $1}' domains.txt > clean_domains.txt
```

შემდეგ, ამ hex-ების ბინარულ ფაილად გადასაქცევად დავწერე კოდი პითონში - 

![[Pasted image 20250518164953.png]]

output.bin გავაანალიზე binwalk-ით.

![[Pasted image 20250518165057.png]]

როგორც აღმოჩნდა, ფაილი .zip ტიპის არის.

შევინახოთ .bin ფაილი როგორც .zip

![[Pasted image 20250518165145.png]]

ამოვაარქივოთ zip.

![[Pasted image 20250518165301.png]]

ამოვიდა pdf ფაილი. 

![[Pasted image 20250518165351.png]]


